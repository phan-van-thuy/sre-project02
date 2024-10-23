# Deploying HA Infrastructure

The first step in this project will be deploying infrastructure that you can run Prometheus and Grafana on. You will then use the servers you deployed to create an SLO/SLI dashboard. Next, you will modify existing infrastructure templates and deploy a highly-available infrastructure to AWS in multiple zones using Terrafrom. With this you will also deploy a RDS database cluster that has a replica in the alternate zone.

## Getting Started

Clone the appropriate git repo with the starter code. There will be 2 folders. Zone1 and zone2. This is where you will run the code from in your AWS Cloudshell terminal.

### Dependencies

```
- helm
- Terraform
- Postman
- kubectl
- Prometheus and Grafana
```

### Project Detail

1. All images taken are stored in the screenshot folder, including SLI/SLO, zone1 and zone2.

#### SLI
The dashboard image identifies the service SLI/SLO on the system

#### Zone1
In zone 1, we will build VPC, EKS, Instance WEB, RDS Cluster, LoadBalancer WEB infrastructure

#### Zone2
In zone 2, we will build VPC, EKS, Instance WEB, RDS Replicate, LoadBalancer WEB infrastructure
