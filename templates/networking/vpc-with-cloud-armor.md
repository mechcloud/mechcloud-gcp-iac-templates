# Deploy Cloud Armor Security Policy with Load Balancer on GCP

This guide demonstrates how to use MechCloud's stateless IaC to provision a Cloud Armor security policy attached to an HTTP(S) Load Balancer for DDoS protection and WAF capabilities.

## Scenario Overview
**Use Case:** Protecting web applications from DDoS attacks, SQL injection, XSS, and other OWASP threats using Cloud Armor WAF rules — while also enabling geo-based access control and rate limiting at the edge.
**Key MechCloud Features Highlighted:**
- Cross-resource referencing (`ref:`)
- Security policy rules as clean YAML
- WAF rule tuning and custom rules

### Architecture Diagram

```mermaid
flowchart TB
    Internet((Internet)) -->|HTTPS| LB[HTTP(S) Load Balancer]
    LB -->|Cloud Armor| Armor[Security Policy: waf-policy]
    Armor -->|Allowed| Backend[Backend Service]
    Armor -->|Blocked| Block[Blocked Requests]
    Backend --> MIG[Managed Instance Group]
```

***

### Complete Unified Template

```yaml
resources:
  - type: gcp_compute_security_policy
    name: waf-policy
    props:
      name: "mc-waf-policy"
      description: "Cloud Armor WAF policy"
      rule:
        - action: deny(403)
          priority: 1000
          match:
            expr:
              expression: "evaluatePreconfiguredExpr('sqli-v33-stable')"
          description: "Block SQL injection"
        - action: deny(403)
          priority: 1001
          match:
            expr:
              expression: "evaluatePreconfiguredExpr('xss-v33-stable')"
          description: "Block XSS attacks"
        - action: deny(403)
          priority: 2000
          match:
            versioned_expr: SRC_IPS_V1
            config:
              src_ip_ranges:
                - "203.0.113.0/24"
          description: "Block known bad IP range"
        - action: allow
          priority: 2147483647
          match:
            versioned_expr: SRC_IPS_V1
            config:
              src_ip_ranges:
                - "*"
          description: "Default allow"
      adaptive_protection_config:
        layer_7_ddos_defense_config:
          enable: true

  - type: gcp_compute_network
    name: vpc1
    props:
      auto_create_subnetworks: false
    resources:
      - type: gcp_compute_subnetwork
        name: subnet1
        props:
          ip_cidr_range: "10.0.1.0/24"
          region: "{{CURRENT_REGION}}"
      - type: gcp_compute_firewall
        name: fw-health-check
        props:
          direction: INGRESS
          allow:
            - protocol: tcp
              ports:
                - "80"
          source_ranges:
            - "130.211.0.0/22"
            - "35.191.0.0/16"

  - type: gcp_compute_instance_template
    name: web-template
    props:
      machine_type: "e2-standard-2"
      disk:
        - source_image: "ubuntu-os-cloud/ubuntu-2404-lts-amd64"
          auto_delete: true
          boot: true
      network_interface:
        - subnetwork: "ref:vpc1/subnet1"

  - type: gcp_compute_instance_group_manager
    name: web-mig
    props:
      zone: "{{CURRENT_REGION}}-a"
      base_instance_name: "mc-web"
      version:
        - instance_template: "ref:web-template"
      target_size: 2
      named_port:
        - name: http
          port: 80

  - type: gcp_compute_health_check
    name: http-hc
    props:
      http_health_check:
        port: 80
        request_path: "/"

  - type: gcp_compute_backend_service
    name: web-backend
    props:
      protocol: HTTP
      port_name: http
      health_checks:
        - "ref:http-hc"
      backend:
        - group: "ref:web-mig.instance_group"
          balancing_mode: UTILIZATION
          max_utilization: 0.8
      security_policy: "ref:waf-policy"

  - type: gcp_compute_url_map
    name: web-url-map
    props:
      default_service: "ref:web-backend"

  - type: gcp_compute_target_http_proxy
    name: web-proxy
    props:
      url_map: "ref:web-url-map"

  - type: gcp_compute_global_forwarding_rule
    name: web-fwd
    props:
      target: "ref:web-proxy"
      port_range: "80"
```
