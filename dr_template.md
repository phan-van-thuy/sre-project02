# Infrastructure

## AWS Zones
Primary Region: us-east-1:
    Availability Zone 1: us-east-1a
    Availability Zone 2: us-east-1b
DR Region: us-west-1:
    Availability Zone 1: us-west-1a
    Availability Zone 2: us-west-1b
    Availability Zone 3: us-west-1c

## Servers and Clusters

### Table 1.1 Summary
| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Web Server | Hosts the frontend application	 | t3.medium	                                          | 3	                                                            | Deployed in multiple AZs in both regions                                                                     |
| DB Cluster | Holds all application data (MySQL RDS)	 | db.m5.large	                                  | 1                                                               | Replicated with read replicas in DR                                                                          |
| S3 Buckets | Stores application files and media | 5Gi                                                   | 1                                                               | Cross-region replication enabled                                                                             |
| Load Balancer | Distributes traffic across web servers | 1                                              | 1                                                               | Configured in both regions                                                                                   |

### Descriptions
 * **Web Server**: These are the EC2 instances running the web application. They are distributed across three availability zones to ensure high availability. In case of failure, they are autoscaled and failover is managed via Route 53.
 * **DB Cluster**: A MySQL RDS database cluster deployed in us-east-1. It has multi-AZ failover enabled, and read replicas are deployed in us-west-2 for DR.
 * **S3 Buckets**: S3 buckets store user-uploaded content and are replicated across regions for durability.
 * **Load Balancer**: The load balancer handles distributing traffic between the web servers. Elastic Load Balancer (ELB) is deployed in both us-east-1 and us-west-2 to handle failover.

## DR Plan
### Pre-Steps:
 * **Set up VPC and Subnets**:: Ensure that a matching VPC, subnets, and security groups are configured in the DR region (us-west-2).
 * **Replicate DB Cluster**:: Set up read replicas of the RDS instances in the DR region and enable automatic failover.
 * **S3 Cross-Region Replication**:: Enable cross-region replication for all critical S3 buckets to replicate data to the DR region.
 * **DNS Configuration**:: Configure Route 53 with failover routing policies to redirect traffic to the DR region if the primary region becomes unavailable.
 * **EC2 AMI Backups**:: Ensure that regularly updated AMIs of critical EC2 instances are available in the DR region for quick deployment.

## Steps:
* **Failover DNS**:: Update Route 53 failover routing policies to redirect traffic to the DR region (us-west-2) for all critical services.
* **Launch EC2 Instances in DR**:: Use the AMIs stored in the DR region to launch the necessary EC2 instances (web servers, application servers).
* **Activate Database in DR**:: Promote the read replicas in us-west-2 to be the new primary database.
* **Reconnect Services**:: Ensure all services (e.g., load balancers, application instances) are reconnected to the new primary database.
* **Monitor and Validate**:: Ensure all services are functioning in the DR region, and monitor for any issues.
* **Notify Stakeholders**:: Once the failover is complete, inform stakeholders and begin the process of recovering the primary region.