# Project Aegis

## Enterprise Cloud Security Engineering Portfolio

Project Aegis is a hands-on AWS cloud engineering and security portfolio focused on designing, implementing, securing, monitoring, and validating enterprise-style cloud infrastructure.

The project follows a lab-based progression covering identity and access management, storage security, networking, compute security, auditing, monitoring, detection, automation, and incident response.

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
- Encryption and key management
- Cloud auditing and logging
- Monitoring and detection
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
- AWS CloudTrail
- AWS Key Management Service (KMS)
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
- SSE-KMS Encryption
- CloudTrail Log File Validation

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

### Monitoring & Auditing

- AWS CloudTrail
- CloudTrail Event History
- Multi-Region Trails
- AWS API Activity Auditing
- Management Event Logging
- Persistent Audit Log Storage
- CloudTrail Log File Validation
- Security Event Investigation

### Development & Documentation

- Git
- GitHub
- JSON
- Markdown
- Architecture Documentation
- Validation Testing
- Security Incident Documentation

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

### Lab 6 – AWS CloudTrail Auditing & Security Investigation

Implemented persistent AWS API auditing and performed a controlled security investigation using:

- Multi-Region AWS CloudTrail
- Read and write management event logging
- Persistent CloudTrail log delivery to Amazon S3
- SSE-KMS encrypted audit logs
- CloudTrail log file validation
- CloudTrail digest files
- AWS API activity analysis
- IAM activity attribution
- EC2 security group auditing
- Controlled SSH exposure simulation
- Security-event reconstruction
- Remediation verification

During the investigation, CloudTrail was used to identify an `AuthorizeSecurityGroupIngress` event that temporarily permitted SSH access from `0.0.0.0/0`. The corresponding `RevokeSecurityGroupIngress` event confirmed that the insecure rule was removed 43 seconds later.

---

## Project Roadmap

- [x] Lab 1 – AWS Account Hardening
- [x] Lab 2 – Enterprise IAM & Least Privilege
- [x] Lab 3 – Amazon S3 Security
- [x] Lab 4 – Amazon VPC & Network Architecture
- [x] Lab 5 – Amazon EC2 Compute Security
- [x] Lab 6 – AWS CloudTrail Auditing & Security Investigation
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
- Auditability and accountability
- Log integrity
- Incident reconstruction
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

The goal is not only to configure AWS services, but to understand why each security control exists, validate that the control works as intended, and document evidence of the resulting security posture.

---

## Current Focus

Project Aegis has completed its foundational AWS identity, storage, networking, compute-security, and cloud-auditing phases.

The current phase focuses on expanding cloud visibility from API auditing into operational monitoring and observability using **Amazon CloudWatch**.

Future labs will introduce threat detection with Amazon GuardDuty, configuration monitoring with AWS Config, centralized security findings with AWS Security Hub, Infrastructure as Code with Terraform, Python automation, cloud incident response, and a final enterprise cloud security capstone.
