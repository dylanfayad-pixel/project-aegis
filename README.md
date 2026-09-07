# Project Aegis

## Enterprise Cloud Security Engineering Portfolio

Project Aegis is a hands-on AWS cloud engineering and security portfolio focused on designing, implementing, securing, and validating enterprise-style cloud infrastructure.

The project follows a lab-based progression covering identity and access management, storage security, networking, compute security, monitoring, detection, automation, and incident response.

Each lab documents the architecture, security decisions, implementation, validation testing, lessons learned, and interview preparation associated with the environment.

---

## Project Objectives

Project Aegis is designed to develop practical experience with:

- AWS cloud infrastructure
- Cloud security architecture
- Identity and Access Management
- Least-privilege access control
- Secure networking
- Private cloud workloads
- Linux administration
- Secure AWS service connectivity
- Encryption
- Monitoring and logging
- Infrastructure as Code
- Cloud automation
- Incident response

---

## Current Technologies

### Cloud

- Amazon Web Services (AWS)
- Amazon EC2
- Amazon S3
- Amazon VPC
- Amazon EBS
- AWS Systems Manager
- AWS PrivateLink
- VPC Interface Endpoints

### Identity & Security

- AWS Identity and Access Management (IAM)
- IAM Roles
- IAM Policies
- Role-Based Access Control (RBAC)
- Principle of Least Privilege
- Multi-Factor Authentication (MFA)
- Security Groups
- S3 Bucket Policies
- IMDSv2
- AWS Key Management Service (KMS)

### Networking

- VPCs
- Public and Private Subnets
- Multiple Availability Zones
- Internet Gateways
- Route Tables
- Security Groups
- Private IPv4 Addressing
- VPC Interface Endpoints
- Security Group Referencing

### Compute & Systems

- Amazon EC2
- Amazon Linux 2023
- AWS Systems Manager Session Manager
- EC2 IAM Instance Profiles
- Encrypted EBS Storage
- Linux Command Line

### Development & Documentation

- Git
- GitHub
- JSON
- Markdown
- Architecture Documentation
- Validation Testing

---

## Completed Labs

### Lab 1 – AWS Account Hardening & Security Baseline

Implemented foundational AWS account security including:

- Root account MFA
- IAM administrative access
- AWS Budgets
- Secure administrative workflow

### Lab 2 – Enterprise IAM & Least Privilege

Designed an enterprise IAM architecture including:

- Administrator, Developer, Security, and Auditor roles
- Role-Based Access Control
- Customer-managed IAM policies
- Principle of Least Privilege
- Dedicated IAM validation accounts

### Lab 3 – Amazon S3 Security

Secured Amazon S3 storage using:

- Block Public Access
- Bucket Versioning
- Server-Side Encryption
- IAM identity-based policies
- Resource-based bucket policies
- Explicit Deny validation

### Lab 4 – Amazon VPC & Network Architecture

Designed a production-style AWS network using:

- Multiple Availability Zones
- Public and private subnets
- Internet Gateway
- Route Tables
- Security Groups
- Network segmentation
- Restricted administrative access

### Lab 5 – Amazon EC2 Compute Security

Deployed and secured a private Amazon EC2 workload using:

- Amazon Linux 2023
- Private subnet deployment
- No public IPv4 address
- Zero inbound Security Group rules
- AWS Systems Manager Session Manager
- EC2 IAM role and instance profile
- AWS PrivateLink
- Systems Manager VPC interface endpoints
- Encrypted EBS storage
- IMDSv2 enforcement
- Security validation testing

---

## Project Roadmap

- [x] Lab 1 – AWS Account Hardening
- [x] Lab 2 – Enterprise IAM & Least Privilege
- [x] Lab 3 – Amazon S3 Security
- [x] Lab 4 – Amazon VPC & Network Architecture
- [x] Lab 5 – Amazon EC2 Compute Security
- [ ] Lab 6 – AWS CloudTrail
- [ ] Lab 7 – Amazon CloudWatch
- [ ] Lab 8 – Amazon GuardDuty
- [ ] Lab 9 – AWS Config
- [ ] Lab 10 – AWS Security Hub
- [ ] Lab 11 – Infrastructure as Code with Terraform
- [ ] Lab 12 – Python AWS Security Automation
- [ ] Lab 13 – Cloud Incident Response
- [ ] Lab 14 – Project Aegis Capstone

---

## Security Principles

Project Aegis applies several core cloud security principles throughout the environment:

- Least privilege
- Defense in depth
- Secure-by-default configuration
- Identity-based access control
- Network segmentation
- Private workload architecture
- Encryption at rest
- Temporary credentials
- Reduced attack surface
- Security control validation
- Cost-aware cloud engineering

---

## Documentation

Each completed lab contains documentation covering:

- Executive summary
- Objectives
- AWS services
- Skills learned
- Security concepts
- Architecture
- Validation testing
- Lessons learned
- Interview preparation

The goal is not only to configure AWS services, but to understand why each security control exists and demonstrate that the control works as intended.

---

## Current Focus

Project Aegis has completed its foundational AWS identity, storage, networking, and compute-security phases.

The next phase focuses on cloud visibility, logging, monitoring, and threat detection beginning with **AWS CloudTrail**.

Future phases will introduce Infrastructure as Code, Python automation, incident response, and a final enterprise cloud security capstone.
