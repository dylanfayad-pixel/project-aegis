# Lab 5 – Interview Notes

## Amazon EC2

### What is Amazon EC2?

Amazon EC2 is an AWS service that provides virtual compute instances in the cloud.

In this lab, I deployed an Amazon Linux 2023 EC2 instance inside a private subnet.

---

### Why did you place the EC2 instance in a private subnet?

The instance did not need to be directly accessible from the Internet.

Placing it in a private subnet reduced its external attack surface while still allowing secure administration through AWS Systems Manager.

---

### Did your EC2 instance have a public IP address?

No.

The instance was intentionally deployed without a public IPv4 address.

---

## Secure Administration

### How did you access the EC2 instance without SSH?

I used AWS Systems Manager Session Manager.

Session Manager provided browser-based shell access without requiring a public IP address, SSH key pair, bastion host, or inbound SSH rule.

---

### Why did you not open port 22?

SSH was unnecessary because administrative access was provided through Session Manager.

The EC2 Security Group therefore contained zero inbound rules.

---

### What user did Session Manager connect as?

The Session Manager shell connected as:

`ssm-user`

I validated this using the `whoami` command.

---

## IAM

### How did the EC2 instance authenticate to AWS Systems Manager?

I created an IAM role called:

`ProjectAegis-EC2-SSM-Role`

The role used the `AmazonSSMManagedInstanceCore` policy and was attached to the EC2 instance through an instance profile.

---

### Why use an IAM role instead of storing AWS access keys on EC2?

IAM roles allow EC2 instances to obtain temporary AWS credentials.

This avoids storing long-lived access keys directly on the server and provides a more secure workload identity model.

---

### What is an EC2 instance profile?

An instance profile is the mechanism used to attach an IAM role to an EC2 instance.

It allows applications and AWS services running on the instance to use the permissions provided by the role.

---

## Networking

### What inbound rules did the EC2 Security Group contain?

None.

The Security Group contained zero inbound rules because the instance was administered through Systems Manager rather than direct inbound network access.

---

### If the Security Group allowed outbound traffic, why could the instance not access the Internet?

Security Groups control whether traffic is permitted, while route tables determine where traffic can actually travel.

The private subnet did not have a default route through a NAT Gateway or Internet Gateway, so the instance had no route to the public Internet.

---

### How did Systems Manager work if the instance had no Internet access?

I created VPC interface endpoints for:

- `ssm`
- `ssmmessages`

These provided private connectivity between the VPC and AWS Systems Manager.

---

### What is AWS PrivateLink?

AWS PrivateLink provides private connectivity to supported services using interface VPC endpoints.

This allows service traffic to remain private without requiring general Internet connectivity.

---

### Why did you create a separate Security Group for the VPC endpoints?

It allowed me to separate workload security from endpoint security.

The endpoint Security Group allowed HTTPS on TCP port 443 specifically from the EC2 Security Group.

This created a more narrowly scoped communication path.

---

## Storage Security

### How did you protect the EC2 instance's storage?

The root EBS volume was encrypted.

I used:

- 8 GiB gp3 EBS
- Encryption enabled
- AWS-managed `aws/ebs` KMS key

---

### What does EBS encryption protect?

EBS encryption protects data stored on the volume at rest.

---

## Instance Metadata

### What is the EC2 Instance Metadata Service?

The Instance Metadata Service provides information about the running EC2 instance from inside the instance.

This can include instance configuration and information associated with the instance's IAM role.

---

### What is IMDSv2?

IMDSv2 is the token-based version of the EC2 Instance Metadata Service.

A client must first obtain a session token and include that token when requesting metadata.

---

### How did you validate IMDSv2?

I first attempted an unauthenticated metadata request.

The request returned:

`HTTP 401`

I then requested an IMDSv2 token and used the token to successfully retrieve the EC2 instance ID.

This demonstrated that tokenless metadata access was rejected while IMDSv2 authentication succeeded.

---

## Validation

### How did you prove that the EC2 instance was isolated from the Internet?

I attempted an HTTPS request to a public Internet destination from the instance.

The connection timed out.

At the same time, Systems Manager remained functional through the private VPC endpoints.

---

### How did you verify the operating system?

I ran:

`cat /etc/os-release`

The output confirmed that the instance was running Amazon Linux 2023.

---

### How did you verify the instance was using private addressing?

I ran:

`ip addr show`

The network interface showed a private `10.0.x.x` IPv4 address inside the Project Aegis VPC.

---

## Architecture

### Describe the Lab 5 architecture in an interview.

I deployed an Amazon Linux EC2 instance into a private subnet with no public IP address, SSH key, or inbound Security Group rules.

I attached an IAM role for Systems Manager and administered the instance through Session Manager. Because the subnet had no NAT or general Internet route, I used private VPC interface endpoints for Systems Manager connectivity.

I also encrypted the EBS root volume and required IMDSv2 for instance metadata access.

---

### What was the most important lesson from this lab?

A cloud workload does not need to be publicly accessible to be manageable.

By combining private networking, IAM roles, Systems Manager, VPC endpoints, encrypted storage, and IMDSv2, I could securely administer the EC2 instance while significantly reducing its external attack surface.

---

## Key Interview Talking Points

- Private EC2 deployment
- No public IPv4 address
- Zero inbound Security Group rules
- No SSH key pair
- AWS Systems Manager Session Manager
- IAM role-based workload identity
- Temporary AWS credentials
- AWS PrivateLink
- VPC interface endpoints
- Security Group referencing
- Encrypted EBS storage
- IMDSv2 enforcement
- Amazon Linux administration
- Network isolation validation
- Security control testing
