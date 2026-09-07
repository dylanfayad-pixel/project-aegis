Lab 5 – Amazon EC2 Compute Security

Executive Summary

Designed and deployed a hardened Amazon EC2 instance inside a private subnet using AWS Systems Manager Session Manager, IAM roles, encrypted EBS storage, IMDSv2, Security Groups, and private VPC endpoints.

The instance was intentionally deployed without a public IPv4 address, SSH key pair, or inbound Security Group rules.

⸻

Objective

Secure an Amazon EC2 workload by implementing private compute architecture, least-privilege access, secure administrative management, encrypted storage, and hardened instance metadata.

⸻

AWS Services

* Amazon EC2
* Amazon VPC
* AWS Identity and Access Management (IAM)
* AWS Systems Manager
* AWS Systems Manager Session Manager
* AWS PrivateLink
* VPC Interface Endpoints
* Amazon Elastic Block Store (EBS)
* AWS Key Management Service (KMS)
* Security Groups

⸻

Skills Learned

* Amazon EC2
* Amazon Linux 2023
* EC2 IAM Roles
* IAM Instance Profiles
* AWS Systems Manager
* Session Manager
* AWS PrivateLink
* VPC Interface Endpoints
* Security Group Referencing
* Private Compute Architecture
* EBS Encryption
* IMDSv2
* Linux Administration
* Security Validation

⸻

Security Concepts

Private EC2

The EC2 instance was deployed inside a private subnet with no public IPv4 address.

This prevents direct Internet access to the workload.

Systems Manager Session Manager

AWS Systems Manager Session Manager provides secure administrative access to EC2 without requiring:

* SSH port 22
* SSH key pairs
* Public IP addresses
* Bastion hosts

IAM Roles

A dedicated IAM role was attached to the EC2 instance using an instance profile.

This allows the workload to use temporary AWS credentials instead of storing long-lived access keys.

Security Groups

The EC2 Security Group contains zero inbound rules.

A separate Security Group protects the Systems Manager VPC endpoints and allows HTTPS traffic only from the EC2 Security Group.

VPC Interface Endpoints

Private interface endpoints were created for:

* Systems Manager (ssm)
* Systems Manager Messages (ssmmessages)

These endpoints allow the private EC2 instance to communicate with Systems Manager without requiring general Internet access.

EBS Encryption

The EC2 root volume was encrypted using the AWS-managed aws/ebs KMS key.

IMDSv2

Instance Metadata Service Version 2 (IMDSv2) was required on the EC2 instance.

IMDSv2 uses token-based authentication to provide stronger protection for instance metadata.

⸻

Architecture

                    AWS Systems Manager
                            ▲
                            │
                     AWS PrivateLink
                            │
              ┌─────────────┴─────────────┐
              │                           │
        SSM Endpoint              SSMMessages Endpoint
              │                           │
              └─────────────┬─────────────┘
                            │
                       HTTPS :443
                            │
                 SSM Endpoint Security Group
                            ▲
                            │
                   EC2 Security Group
                  (0 Inbound Rules)
                            │
                            │
                ProjectAegis-Private-EC2
                   Amazon Linux 2023
                            │
                     Private IPv4 Only
                            │
                    Private Subnet A
                       us-east-2a
                            │
                  project-aegis-vpc
                     10.0.0.0/16

⸻

EC2 Security Configuration

Control	Configuration
Public IPv4	Disabled
SSH Key Pair	None
Inbound Rules	0
Operating System	Amazon Linux 2023
Instance Type	t3.micro
Root Storage	8 GiB gp3
EBS Encryption	Enabled
KMS Key	aws/ebs
Metadata Service	IMDSv2 Required
Administration	Systems Manager Session Manager
Internet Route	None

⸻

Validation

Successfully validated:

* EC2 instance running inside the private subnet
* No public IPv4 address assigned
* Zero inbound Security Group rules
* No SSH key pair configured
* Systems Manager instance status online
* Session Manager shell successfully established
* Shell access authenticated as ssm-user
* Amazon Linux 2023 operating system
* Private IPv4 addressing
* General Internet access unavailable
* IMDSv1-style metadata request rejected with HTTP 401
* IMDSv2 token successfully acquired
* Instance metadata successfully retrieved using IMDSv2
* EC2 IAM role successfully attached
* Encrypted EBS root volume
* Systems Manager connectivity through private VPC endpoints

⸻

Security Architecture

The final EC2 workload uses multiple layers of protection:

Private Subnet
      │
      ▼
Private EC2
      │
      ├── No Public IP
      ├── No SSH Key
      ├── Zero Inbound Rules
      ├── Encrypted EBS
      ├── IMDSv2 Required
      └── IAM Instance Role
              │
              ▼
       Session Manager
              │
              ▼
     Private VPC Endpoints

⸻

Lessons Learned

* EC2 instances do not require public IP addresses for secure administration.
* Session Manager can provide administrative access without opening inbound SSH.
* IAM roles provide temporary credentials to EC2 workloads.
* Security Groups and route tables perform different networking functions.
* VPC interface endpoints provide private connectivity to supported AWS services.
* Security Groups can reference other Security Groups to implement least-privilege communication.
* EBS encryption protects EC2 storage at rest.
* IMDSv2 strengthens protection of EC2 instance metadata.
* Cloud security controls should be validated through testing rather than assumed to work.
* Secure EC2 architecture combines identity, networking, compute, storage, and management controls.
