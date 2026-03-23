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
| **networking** | [multi-subnet-vpc.md](templates/networking/multi-subnet-vpc.md) | Two-tier VPC with frontend and backend subnetworks with separate firewall rules |
| | [vpc-peering.md](templates/networking/vpc-peering.md) | Two VPC Networks peered together with bidirectional peering and VMs in each |
| | [cloud-nat-private-vm.md](templates/networking/cloud-nat-private-vm.md) | Private VM with outbound-only internet access via Cloud NAT and Cloud Router |
| | [cloud-dns-with-vm.md](templates/networking/cloud-dns-with-vm.md) | VM with Cloud DNS managed zone and A record pointing to a Static External IP |
| | [vpc-with-cloud-vpn.md](templates/networking/vpc-with-cloud-vpn.md) | VPC with Cloud VPN Gateway and site-to-site VPN tunnel to on-premises network |
| **load-balancing** | [vm-with-load-balancer.md](templates/load-balancing/vm-with-load-balancer.md) | VMs behind a global HTTP(S) Load Balancer with health checks and backend service |
| | [instance-group-autoscaler.md](templates/load-balancing/instance-group-autoscaler.md) | Managed Instance Group with Autoscaler and HTTP(S) Load Balancer for auto-scaling |
| **storage** | [cloud-storage-bucket.md](templates/storage/cloud-storage-bucket.md) | Cloud Storage bucket with lifecycle rules for automated tiering and data retention |
| | [vm-with-filestore.md](templates/storage/vm-with-filestore.md) | VM with Filestore instance for shared NFS file storage across compute instances |
| **database** | [vm-with-cloud-sql.md](templates/database/vm-with-cloud-sql.md) | VM with Cloud SQL MySQL instance connected via private IP networking |
| | [vm-with-memorystore.md](templates/database/vm-with-memorystore.md) | VM with Memorystore for Redis for in-memory session and data caching |
| **messaging** | [vm-with-pub-sub.md](templates/messaging/vm-with-pub-sub.md) | VM with Pub/Sub topic, subscription, and dead-letter topic for async messaging |

## Contributing

Contributions are welcome! If you have a useful GCP infrastructure pattern, feel free to submit a pull request. Please follow the existing template structure which includes:

1. A descriptive title and introduction
2. A scenario overview with use case and key features
3. A Mermaid architecture diagram
4. Step-by-step YAML code blocks with explanations
5. A complete unified template at the end

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file.
