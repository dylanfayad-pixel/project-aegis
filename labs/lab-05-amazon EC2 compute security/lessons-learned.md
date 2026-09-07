# Lab 5 – Lessons Learned

## Private EC2 Architecture

EC2 instances do not need public IP addresses to be securely administered.

Deploying the instance inside a private subnet reduced its external attack surface while AWS Systems Manager provided administrative access.

---

## Systems Manager Session Manager

Session Manager can replace traditional SSH administration for supported management workflows.

This allowed the EC2 instance to be managed without:

- A public IPv4 address
- An SSH key pair
- Inbound TCP port 22
- A bastion host

---

## IAM Roles for EC2

IAM roles provide EC2 workloads with temporary AWS credentials.

Using an instance role is more secure than storing long-lived AWS access keys directly on a server.

---

## Security Groups and Route Tables

Security Groups and route tables perform different functions.

Security Groups determine whether network traffic is permitted.

Route tables determine where network traffic can travel.

An outbound Security Group rule alone does not provide Internet connectivity if the subnet has no route to the Internet.

---

## VPC Interface Endpoints

VPC interface endpoints allow private resources to communicate with supported AWS services without requiring general Internet access.

The `ssm` and `ssmmessages` endpoints allowed the private EC2 instance to communicate with AWS Systems Manager while remaining isolated from the public Internet.

---

## Security Group Referencing

Security Groups can reference other Security Groups instead of relying only on IP addresses.

The Systems Manager endpoint Security Group allowed HTTPS traffic specifically from the EC2 Security Group.

This provided a more narrowly scoped communication path.

---

## EBS Encryption

Amazon EBS encryption protects EC2 storage at rest.

The EC2 root volume was encrypted using the AWS-managed `aws/ebs` KMS key.

---

## IMDSv2

IMDSv2 strengthens access to EC2 instance metadata by requiring a session token.

Testing demonstrated that an unauthenticated metadata request returned HTTP `401`, while a token-authenticated IMDSv2 request successfully retrieved instance metadata.

---

## Security Validation

Security controls should not simply be assumed to work because they are configured.

In this lab, I validated:

- Private IPv4 addressing
- Lack of general Internet connectivity
- Systems Manager connectivity
- Session Manager shell access
- IMDSv2 enforcement
- IAM role attachment
- Encrypted EBS storage
- Zero inbound Security Group rules

Testing both allowed and blocked behavior provided stronger evidence that the architecture worked as intended.

---

## Cost Awareness

Secure cloud architecture also requires cost awareness.

VPC interface endpoints are billable resources, so temporary lab resources should be removed after validation and documentation are complete.

---

## Final Takeaway

Secure EC2 architecture requires multiple layers of protection working together.

This lab combined:

- Private networking
- Least-privilege Security Groups
- IAM workload identity
- Secure administrative access
- Private AWS service connectivity
- Encryption at rest
- Metadata protection
- Validation testing

The result was an EC2 workload that remained securely manageable without exposing the instance directly to the public Internet.
