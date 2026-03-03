# Production-Style Web Infrastructure on AWS
## Overview

This repository documents the deployment of a highly available, multi-tier web infrastructure on AWS.

The architecture isolates application servers in private subnets while exposing traffic through an Application Load Balancer.

The design follows common production networking patterns.

## Architecture Summary

Custom VPC

2 Public Subnets (Multi-AZ)

2 Private Subnets (Multi-AZ)

Internet Gateway

NAT Gateway

Public & Private Route Tables

Application Load Balancer

Target Group with health checks

2 EC2 instances (private)

NGINX web server

AWS Systems Manager (Session Manager)

IAM Role for instance management

## Network Design
### Public Layer

Application Load Balancer

Internet Gateway

Public route table → 0.0.0.0/0 → IGW

### Private Layer

EC2 application instances

No public IP addresses

Private route table → 0.0.0.0/0 → NAT Gateway

Outbound internet access is provided via NAT Gateway only.

## Security Model

ALB allows inbound HTTP (80) from internet.

EC2 instances allow HTTP only from ALB security group.

No SSH exposed to internet.

Instance access handled via AWS Systems Manager.

IAM role attached to instances.

This enforces network segmentation and least-privilege access.

## Compute Layer

2 EC2 instances (Amazon Linux)

Deployed across multiple Availability Zones

NGINX installed

Each instance serves a distinct response

Target group health checks enabled

## Load Balancing

Application Load Balancer deployed in public subnets

HTTP listener (port 80)

Traffic distributed across private instances

Health checks ensure availability

Access via ALB DNS successfully distributes traffic between instances.

## Operational Management

Remote access via AWS Systems Manager Session Manager

No key pairs required

No bastion host

No exposed SSH ports

Repository Contents

Architecture screenshots

Networking configuration

Route table configuration

Target group status

Load balancer behavior

SSM session access proof
