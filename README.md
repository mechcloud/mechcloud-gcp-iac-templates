# MechCloud GCP IaC Templates

A curated collection of sample Infrastructure-as-Code (IaC) templates for provisioning Google Cloud Platform resources using [MechCloud's](https://mechcloud.io) stateless IaC engine.

## Overview

This repository contains ready-to-use MechCloud templates that demonstrate how to provision common GCP infrastructure patterns — from simple single-VM deployments to multi-tier architectures with load balancers, managed databases, and autoscaling instance groups. Each template includes a scenario overview, an architecture diagram, a step-by-step walkthrough, and a complete unified YAML template.

These templates are designed to be used with [MechCloud's Stateless IaC](https://docs.mechcloud.io) feature, which allows you to define and deploy cloud infrastructure using a declarative YAML syntax without managing any state files.

## Key Features

- **Hierarchical resource nesting** — Define parent-child relationships naturally (e.g., VPC → Subnetwork & Firewall)
- **Zonal defaults injection** — Set defaults like `zone: us-central1-a` once and apply them across all resources
- **Automatic parent-link inference** — Nested resources automatically inherit parent references
- **Cross-resource referencing** — Reference resources across the hierarchy using `ref:` syntax
- **No state management** — MechCloud handles state automatically; just define your desired infrastructure

## Getting Started

1. Sign up or log in at [MechCloud Portal](https://portal.mechcloud.io)
2. Connect your GCP project
3. Create a new stack and paste any template from this repository
4. Deploy and manage your infrastructure through MechCloud

## Templates

| Folder | Template | Description |
|--------|----------|-------------|
| **compute** | [public-web-vm-with-static-ip.md](templates/compute/public-web-vm-with-static-ip.md) | Public web application VM with a Static External IP, VPC, Subnetwork, and Firewall rules |
| | [private-vm-no-external-ip.md](templates/compute/private-vm-no-external-ip.md) | Private VM with no external IP, accessible only within the VPC |
| | [vm-with-persistent-disk.md](templates/compute/vm-with-persistent-disk.md) | VM with an additional 100GB SSD persistent disk for separate data storage |
| | [cloud-run-with-cloud-sql.md](templates/compute/cloud-run-with-cloud-sql.md) | Serverless Cloud Run service connected to Cloud SQL via VPC connector |
| | [cloud-run-service.md](templates/compute/cloud-run-service.md) | Cloud Run service with auto-scaling and public access for serverless containers |
| | [vm-with-startup-script.md](templates/compute/vm-with-startup-script.md) | VM with startup script for automated Nginx installation |
| | [managed-instance-group.md](templates/compute/managed-instance-group.md) | Managed Instance Group with autoscaler and auto-healing health checks |
| | [preemptible-instance-group.md](templates/compute/preemptible-instance-group.md) | Spot (preemptible) VM instance group for cost-optimized batch workloads |
| | [vm-with-gpu.md](templates/compute/vm-with-gpu.md) | VM with NVIDIA GPU accelerator for ML training and HPC workloads |
| | [sole-tenant-node.md](templates/compute/sole-tenant-node.md) | Sole-tenant node for dedicated hardware isolation and BYOL compliance |
| **containers** | [gke-cluster.md](templates/containers/gke-cluster.md) | GKE cluster with managed node pool and private cluster configuration |
| | [artifact-registry.md](templates/containers/artifact-registry.md) | Artifact Registry for container images with cleanup policies and IAM |
| **database** | [vm-with-cloud-sql.md](templates/database/vm-with-cloud-sql.md) | VM with Cloud SQL MySQL instance connected via private IP networking |
| | [vm-with-memorystore.md](templates/database/vm-with-memorystore.md) | VM with Memorystore for Redis for in-memory session and data caching |
| | [cloud-spanner.md](templates/database/cloud-spanner.md) | Cloud Spanner instance for globally distributed, strongly consistent databases |
| | [cloud-sql-ha.md](templates/database/cloud-sql-ha.md) | Cloud SQL PostgreSQL with HA failover and read replica |
| | [bigquery-dataset.md](templates/database/bigquery-dataset.md) | BigQuery dataset with tables and views for data warehousing and analytics |
| | [firestore.md](templates/database/firestore.md) | Firestore database with composite indexes for serverless NoSQL |
| | [alloydb-cluster.md](templates/database/alloydb-cluster.md) | AlloyDB cluster with primary instance and read pool for high-performance PostgreSQL |
| **load-balancing** | [vm-with-load-balancer.md](templates/load-balancing/vm-with-load-balancer.md) | VMs behind a global HTTP(S) Load Balancer with health checks and backend service |
| | [instance-group-autoscaler.md](templates/load-balancing/instance-group-autoscaler.md) | Managed Instance Group with Autoscaler and HTTP(S) Load Balancer for auto-scaling |
| **messaging** | [vm-with-pub-sub.md](templates/messaging/vm-with-pub-sub.md) | VM with Pub/Sub topic, subscription, and dead-letter topic for async messaging |
| | [cloud-tasks.md](templates/messaging/cloud-tasks.md) | Cloud Tasks queues for reliable asynchronous task execution with rate limiting |
| **monitoring** | [cloud-monitoring-alerts.md](templates/monitoring/cloud-monitoring-alerts.md) | Cloud Monitoring alert policies with notification channels for proactive monitoring |
| | [cloud-logging-sink.md](templates/monitoring/cloud-logging-sink.md) | Cloud Logging sinks to Storage, BigQuery, and Pub/Sub for centralized log management |
| **networking** | [multi-subnet-vpc.md](templates/networking/multi-subnet-vpc.md) | Two-tier VPC with frontend and backend subnetworks with separate firewall rules |
| | [vpc-peering.md](templates/networking/vpc-peering.md) | Two VPC Networks peered together with bidirectional peering and VMs in each |
| | [cloud-nat-private-vm.md](templates/networking/cloud-nat-private-vm.md) | Private VM with outbound-only internet access via Cloud NAT and Cloud Router |
| | [cloud-dns-with-vm.md](templates/networking/cloud-dns-with-vm.md) | VM with Cloud DNS managed zone and A record pointing to a Static External IP |
| | [vpc-with-cloud-vpn.md](templates/networking/vpc-with-cloud-vpn.md) | VPC with Cloud VPN Gateway and site-to-site VPN tunnel to on-premises network |
| | [vpc-with-cloud-armor.md](templates/networking/vpc-with-cloud-armor.md) | Cloud Armor security policy with HTTP(S) LB for DDoS and WAF protection |
| | [private-service-connect.md](templates/networking/private-service-connect.md) | Private Service Connect for private access to Google APIs |
| | [vpc-shared.md](templates/networking/vpc-shared.md) | Shared VPC for centralized multi-project network management |
| | [cloud-cdn-with-lb.md](templates/networking/cloud-cdn-with-lb.md) | Cloud CDN with HTTP(S) Load Balancer for global content caching |
| | [internal-load-balancer.md](templates/networking/internal-load-balancer.md) | Internal TCP/UDP Load Balancer for private microservices communication |
| | [vpc-flow-logs.md](templates/networking/vpc-flow-logs.md) | VPC Flow Logs with BigQuery sink for network traffic analysis |
| | [cloud-interconnect.md](templates/networking/cloud-interconnect.md) | Cloud Router with Partner Interconnect for dedicated private connectivity |
| | [cloud-dns-private-zone.md](templates/networking/cloud-dns-private-zone.md) | Cloud DNS private zone for internal service discovery within a VPC |
| **security** | [iam-service-account.md](templates/security/iam-service-account.md) | IAM service accounts with fine-grained role bindings for workload identity |
| | [secret-manager.md](templates/security/secret-manager.md) | Secret Manager with secrets, versions, and IAM-based access control |
| | [kms-encryption.md](templates/security/kms-encryption.md) | Cloud KMS key rings and crypto keys for CMEK encryption |
| | [vpc-service-controls.md](templates/security/vpc-service-controls.md) | VPC Service Controls perimeters for data exfiltration prevention |
| | [organization-policy.md](templates/security/organization-policy.md) | Organization Policy constraints for governance and compliance |
| **serverless** | [cloud-functions-http.md](templates/serverless/cloud-functions-http.md) | Cloud Functions (2nd gen) with HTTP trigger for serverless APIs |
| | [cloud-functions-pubsub.md](templates/serverless/cloud-functions-pubsub.md) | Cloud Functions with Pub/Sub trigger for event-driven processing |
| | [cloud-scheduler.md](templates/serverless/cloud-scheduler.md) | Cloud Scheduler with Cloud Functions for automated cron-style tasks |
| **storage** | [cloud-storage-bucket.md](templates/storage/cloud-storage-bucket.md) | Cloud Storage bucket with lifecycle rules for automated tiering and data retention |
| | [vm-with-filestore.md](templates/storage/vm-with-filestore.md) | VM with Filestore instance for shared NFS file storage across compute instances |
| | [gcs-versioning-lifecycle.md](templates/storage/gcs-versioning-lifecycle.md) | Cloud Storage with versioning and lifecycle policies for cost-optimized data management |
| | [gcs-signed-url-cdn.md](templates/storage/gcs-signed-url-cdn.md) | Cloud Storage with service account for signed URL content distribution |

## Contributing

Contributions are welcome! If you have a useful GCP infrastructure pattern, feel free to submit a pull request. Please follow the existing template structure which includes:

1. A descriptive title and introduction
2. A scenario overview with use case and key features
3. A Mermaid architecture diagram
4. Step-by-step YAML code blocks with explanations
5. A complete unified template at the end

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file.
