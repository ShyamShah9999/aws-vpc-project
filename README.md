# AWS VPC Project: Custom Multi-AZ VPC with Public Web Server

## Overview
This project builds a production-style network on AWS using Infrastructure as Code (CloudFormation). A custom VPC is split into public and private subnets across two Availability Zones, and a web server in a public subnet serves a live website.

**Live Link:** http://CHANGE-THIS-TO-YOUR-WEBSITEURL

## Architecture

```
                        Internet
                           |
                   [Internet Gateway]
                           |
  +------------------- VPC 10.0.0.0/16 -------------------+
  |                                                       |
  |  AZ-a                           AZ-b                  |
  |  Public 10.0.1.0/24             Public 10.0.2.0/24    |
  |   - Web Server (Apache)          (route to IGW)       |
  |                                                       |
  |  Private 10.0.11.0/24           Private 10.0.12.0/24  |
  |   (no direct internet access)                         |
  |                                                       |
  |  S3 Gateway Endpoint (both route tables)              |
  +-------------------------------------------------------+
```

## AWS Services Used
VPC, Subnets, Internet Gateway, Route Tables, Security Groups, VPC Endpoint (S3), EC2, IAM (SSM role), CloudFormation.

## Components
| Component | Details |
|---|---|
| VPC | 10.0.0.0/16, DNS hostnames enabled |
| Public subnets | 10.0.1.0/24 (AZ-a), 10.0.2.0/24 (AZ-b), auto-assign public IP |
| Private subnets | 10.0.11.0/24 (AZ-a), 10.0.12.0/24 (AZ-b) |
| Internet Gateway | Attached to the VPC, used by the public route table |
| NAT Gateway | Optional (disabled to save cost) |
| Security Groups | Web SG: HTTP 80 from anywhere. Private SG: only from Web SG |
| Web server | Amazon Linux 2023 + Apache, page created through user data |
| Access | AWS Systems Manager Session Manager (no SSH keys, no port 22 open) |

## How to Deploy
1. Sign in to the AWS Console and select a region.
2. Go to **CloudFormation > Create stack > With new resources**.
3. Choose **Upload a template file** and upload `vpc-project.yaml`.
4. Stack name: `my-vpc-project`. Keep `EnableNatGateway = false`.
5. Tick the IAM acknowledgement and click **Submit**.
6. When the status is `CREATE_COMPLETE`, open the **Outputs** tab and copy **WebsiteURL**.

## Screenshots
See the `screenshots` section of this repository for the VPC resource map, subnets, route tables, security groups, EC2 instance, stack outputs and the live website.

## What I Learned
- Designing a VPC CIDR plan and subnet layout across multiple Availability Zones
- Difference between public and private subnets (route to Internet Gateway vs NAT)
- Controlling traffic with security group rules
- Using a VPC gateway endpoint to reach S3 privately
- Deploying repeatable infrastructure with CloudFormation

## Author
CHANGE THIS: Your Name | Your Student ID
