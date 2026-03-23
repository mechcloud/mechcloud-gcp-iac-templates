# Deploy Cloud CDN with HTTP(S) Load Balancer on GCP

This guide demonstrates how to use MechCloud's stateless IaC to provision Cloud CDN with an HTTP(S) Load Balancer for global content caching and acceleration.

## Scenario Overview
**Use Case:** Accelerating web application delivery by caching static and dynamic content at Google's global edge POPs — reducing origin load, improving page load times by up to 10x, and lowering bandwidth costs.
**Key MechCloud Features Highlighted:**
- Cross-resource referencing (`ref:`)
- CDN cache policy configuration as clean YAML
- Backend service with CDN enabled

### Architecture Diagram

```mermaid
flowchart LR
    Users((Global Users)) -->|HTTPS| LB[HTTP(S) LB + Cloud CDN]
    LB -->|Cache hit| Edge[Edge Cache]
    LB -->|Cache miss| Backend[Backend Service]
    Backend --> MIG[Instance Group]
```

***

### Complete Unified Template

```yaml
resources:
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
      base_instance_name: "mc-cdn-web"
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
    name: cdn-backend
    props:
      protocol: HTTP
      port_name: http
      health_checks:
        - "ref:http-hc"
      backend:
        - group: "ref:web-mig.instance_group"
          balancing_mode: UTILIZATION
          max_utilization: 0.8
      enable_cdn: true
      cdn_policy:
        cache_mode: CACHE_ALL_STATIC
        default_ttl: 3600
        max_ttl: 86400
        client_ttl: 600
        negative_caching: true
        signed_url_cache_max_age_sec: 0
        cache_key_policy:
          include_host: true
          include_protocol: true
          include_query_string: true

  - type: gcp_compute_url_map
    name: cdn-url-map
    props:
      default_service: "ref:cdn-backend"

  - type: gcp_compute_target_http_proxy
    name: cdn-proxy
    props:
      url_map: "ref:cdn-url-map"

  - type: gcp_compute_global_forwarding_rule
    name: cdn-fwd
    props:
      target: "ref:cdn-proxy"
      port_range: "80"
```
