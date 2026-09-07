Lab 5 – Amazon EC2 Compute Security

Executive Summary

Designed and deployed a hardened Amazon EC2 workload inside the private network architecture created in Lab 4.

The EC2 instance was deployed without a public IPv4 address, SSH key pair, or inbound Security Group rules. Administrative access was provided through AWS Systems Manager Session Manager using an EC2 IAM role and private VPC interface endpoints.

Additional security controls included encrypted Amazon EBS storage, IMDSv2 enforcement, private subnet isolation, least-privilege IAM permissions, and validation testing to confirm that the implemented controls behaved as intended.

⸻

Objective

Deploy and secure an Amazon EC2 instance using cloud security principles rather than exposing the workload directly to the public Internet.

The lab focused on:

* Private compute architecture
* Secure administrative access
* IAM roles for AWS workloads
* Least-privilege network access
* Encryption at rest
* Instance Metadata Service security
* Private AWS service connectivity
* Security control validation

⸻

AWS Services

* Amazon EC2
* Amazon VPC
* AWS Identity and Access Management (IAM)
* AWS Systems Manager
* AWS Systems Manager Session Manager
* AWS PrivateLink / VPC Interface Endpoints
* Amazon Elastic Block Store (EBS)
* AWS Key Management Service (KMS)
* Security Groups

⸻

Skills Learned

* Amazon EC2 deployment
* Amazon Linux 2023
* EC2 IAM instance profiles
* AWS Systems Manager Session Manager
* VPC interface endpoints
* AWS PrivateLink
* Security Group referencing
* Private subnet compute deployment
* Amazon EBS encryption
* AWS KMS managed keys
* Instance Metadata Service Version 2 (IMDSv2)
* Linux command-line administration
* Network isolation testing
* Security control validation

⸻

Security Concepts

Private Compute

The EC2 instance was deployed into a private subnet in the Project Aegis VPC.

The instance was configured with:

* No public IPv4 address
* No SSH key pair
* No inbound Security Group rules
* No direct Internet route

This reduced the external attack surface of the workload.

Secure Administration with Session Manager

Traditional SSH administration normally requires inbound TCP port 22, network connectivity to the instance, and management of SSH keys.

Instead, AWS Systems Manager Session Manager was used to provide administrative shell access.

This allowed the instance to be administered without:

* Opening SSH port 22
* Assigning a public IP address
* Managing an SSH key pair
* Deploying a bastion host

The Session Manager connection successfully provided a shell as the ssm-user account.

IAM Role for EC2

A dedicated IAM role was created:

ProjectAegis-EC2-SSM-Role

The role uses the AWS-managed:

AmazonSSMManagedInstanceCore

policy.

The role was attached to the EC2 instance through an instance profile, allowing the workload to obtain temporary AWS credentials rather than storing long-lived access keys on the server.

Security Groups

The EC2 Security Group:

ProjectAegis-EC2-Private-SG

contains zero inbound rules.

A separate Security Group was created for the Systems Manager VPC endpoints:

ProjectAegis-SSM-Endpoint-SG

The endpoint Security Group permits HTTPS traffic on TCP port 443 from the EC2 Security Group.

This created a least-privilege communication path between the private workload and the Systems Manager interface endpoints.

Private AWS Service Connectivity

Because the private subnet does not have a NAT Gateway or default Internet route, VPC interface endpoints were used to provide private connectivity to AWS Systems Manager.

The following interface endpoints were deployed:

* com.amazonaws.us-east-2.ssm
* com.amazonaws.us-east-2.ssmmessages

Private DNS was enabled for the endpoints.

This allowed Systems Manager traffic to use private connectivity while the EC2 workload remained isolated from general Internet access.

Encryption at Rest

The EC2 root volume was configured as an encrypted Amazon EBS gp3 volume.

Configuration:

* Size: 8 GiB
* Volume type: gp3
* Encryption: Enabled
* KMS key: AWS-managed aws/ebs key
* Delete on termination: Enabled

This protects data stored on the EC2 root volume through encryption at rest.

Instance Metadata Service

The EC2 instance was configured to require:

IMDSv2

IMDSv2 requires session-oriented authentication using a metadata token before instance metadata can be retrieved.

An unauthenticated IMDSv1-style request was tested and returned:

HTTP 401

An IMDSv2 token was then successfully acquired and used to retrieve instance metadata.

This validated that IMDSv2 enforcement was functioning correctly.

⸻

Architecture

                         AWS Systems Manager
                                  ▲
                                  │
                         Private AWS Traffic
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
               SSM Endpoint              SSMMessages Endpoint
                    │                           │
                    └─────────────┬─────────────┘
                                  │
                         HTTPS / TCP 443
                                  │
                    SSM Endpoint Security Group
                                  ▲
                                  │
                     EC2 Private Security Group
                     (Zero Inbound Rules)
                                  │
                                  │
                    ProjectAegis-Private-EC2
                    Amazon Linux 2023
                                  │
                         Private IPv4 Only
                                  │
                    Private Subnet – us-east-2a
                                  │
                       project-aegis-vpc
                           10.0.0.0/16

EC2 Security Posture

ProjectAegis-Private-EC2
│
├── Public IPv4: None
├── SSH Key Pair: None
├── Inbound Security Group Rules: 0
├── Operating System: Amazon Linux 2023
├── Root Storage: Encrypted gp3 EBS
├── Metadata Service: IMDSv2 Required
├── IAM Role: ProjectAegis-EC2-SSM-Role
│
└── Administration
     └── AWS Systems Manager Session Manager
          └── Private VPC Interface Endpoints

⸻

Implementation

1. EC2 IAM Role

Created the IAM role:

ProjectAegis-EC2-SSM-Role

The role trusts the EC2 service and uses AmazonSSMManagedInstanceCore to provide the permissions required for Systems Manager management.

2. EC2 Security Group

Created:

ProjectAegis-EC2-Private-SG

The Security Group contains no inbound rules.

This prevents direct inbound network connections to the EC2 instance.

3. Systems Manager Endpoint Security Group

Created:

ProjectAegis-SSM-Endpoint-SG

Inbound access was restricted to:

* Protocol: TCP
* Port: 443
* Source: ProjectAegis-EC2-Private-SG

4. VPC Interface Endpoints

Created private interface endpoints in us-east-2a for:

* Systems Manager (ssm)
* Systems Manager Messages (ssmmessages)

Private DNS was enabled.

5. EC2 Deployment

Deployed:

ProjectAegis-Private-EC2

Configuration included:

* Amazon Linux 2023
* t3.micro
* Private subnet
* No public IPv4 address
* No key pair
* ProjectAegis-EC2-Private-SG
* ProjectAegis-EC2-SSM-Role
* Encrypted 8 GiB gp3 EBS volume
* IMDSv2 required

⸻

Validation

EC2 Health

The EC2 instance successfully entered the Running state and passed all AWS instance status checks.

Session Manager

AWS Systems Manager reported the instance as online and available for Session Manager.

A browser-based Session Manager shell was successfully established without SSH.

The session returned:

whoami
ssm-user

The operating system was verified as Amazon Linux 2023.

Public Exposure

The instance was verified to have no public IPv4 address.

The EC2 Security Group was verified to contain zero inbound rules.

No SSH key pair was configured.

Private Network Isolation

The private subnet route table contained only the local VPC route and did not contain a default route through a NAT Gateway or Internet Gateway.

An outbound request to a public Internet destination was tested from the instance and timed out as expected.

This demonstrated that general Internet access was unavailable while Systems Manager connectivity continued to function through the private endpoints.

Private IPv4 Address

Linux network-interface inspection confirmed that the workload was using a private 10.0.x.x address inside the Project Aegis VPC.

IMDSv2

An unauthenticated metadata request returned:

401

An IMDSv2 token was then successfully acquired.

The token was used to make an authenticated metadata request and retrieve the EC2 instance ID.

This demonstrated that IMDSv1-style access was rejected while IMDSv2 token-authenticated access succeeded.

IAM Role

Instance metadata confirmed that the EC2 workload was associated with:

ProjectAegis-EC2-SSM-Role

This demonstrated the use of an EC2 IAM role rather than manually storing long-lived AWS credentials on the instance.

⸻

Security Results

The final EC2 architecture demonstrated:

* Private compute deployment
* No public IPv4 exposure
* Zero inbound Security Group rules
* No inbound SSH
* No SSH key management
* IAM role-based workload identity
* Temporary AWS credentials
* Private Systems Manager connectivity
* Encrypted EBS storage
* IMDSv2 enforcement
* Private subnet isolation
* Linux administration through Session Manager
* Validation of both permitted and blocked behavior

⸻

Lessons Learned

* EC2 workloads do not require public IP addresses to be securely administered.
* Systems Manager Session Manager can replace traditional inbound SSH administration for supported management workflows.
* IAM roles allow EC2 workloads to use temporary AWS credentials instead of long-lived access keys.
* Security Groups and route tables perform different security and networking functions.
* Allowing outbound traffic in a Security Group does not create Internet connectivity when no network route exists.
* VPC interface endpoints can provide private access to supported AWS services without giving a workload general Internet access.
* Security Groups can reference other Security Groups to create narrowly scoped communication paths.
* EBS encryption protects EC2 storage at rest.
* IMDSv2 uses token-based access to strengthen EC2 instance metadata security.
* Security controls should be validated through testing rather than assumed to work based only on configuration.
* Secure cloud architecture requires coordinating identity, networking, compute, storage, and management controls.

⸻

Cost & Resource Management

VPC interface endpoints are billable resources.

The Systems Manager interface endpoints were deployed only for the duration necessary to implement and validate the lab.

Temporary EC2 resources should also be terminated when they are no longer required.

This lab reinforces that cloud engineering includes both security architecture and responsible resource/cost management.
