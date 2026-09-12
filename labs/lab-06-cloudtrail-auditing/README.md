# Lab 6 — AWS CloudTrail Auditing & Security Investigation

## Overview

In this lab, I implemented an AWS auditing solution using AWS CloudTrail, Amazon S3, and AWS Key Management Service (KMS).

The goal was to establish persistent logging of AWS account activity, protect audit logs using encryption and integrity validation, and demonstrate how CloudTrail can be used to investigate changes to cloud infrastructure.

To validate the solution, I performed a controlled security test by temporarily allowing SSH access from `0.0.0.0/0` on an EC2 security group. I then removed the rule and used CloudTrail to reconstruct both the configuration change and its remediation.

---

## Architecture

```text
              AWS Account Activity
                      |
                      v
              +----------------+
              | AWS CloudTrail |
              +----------------+
                      |
          Management Events
            (Read + Write)
                      |
                      v
               +-----------+
               | AWS KMS   |
               | SSE-KMS   |
               +-----------+
                      |
                      v
               +-----------+
               | Amazon S3 |
               +-----------+
                  |       |
                  |       |
                  v       v
           CloudTrail   CloudTrail-Digest
              Logs      Validation Files
```

### AWS Services Used

- AWS CloudTrail
- Amazon S3
- AWS Key Management Service (KMS)
- Amazon EC2
- AWS Identity and Access Management (IAM)

---

## CloudTrail Configuration

I created a multi-Region CloudTrail trail named:

`ProjectAegis-CloudTrail`

The trail was configured to capture both read and write management events.

Security controls included:

- Multi-Region logging
- Persistent log delivery to Amazon S3
- SSE-KMS encryption
- CloudTrail log file validation
- Management event auditing
- MFA-authenticated administrative access

![CloudTrail Configuration](screenshots/01-cloudtrail-configuration.png)

---

## Understanding CloudTrail Events

Before creating the persistent trail, I examined CloudTrail Event History and investigated an existing EC2 `RunInstances` API event.

The event demonstrated how CloudTrail can identify:

- The IAM identity performing an action
- The AWS API operation
- Event timestamp
- AWS Region
- Source of the request
- Resources affected by the request
- Request parameters
- Whether the operation modified AWS resources

The event also reflected security controls implemented in the previous EC2 lab, including an EC2 instance launched without a public IP address, encrypted EBS storage, IMDSv2 enforcement, and an IAM instance profile.

---

## Controlled Security Incident

To demonstrate CloudTrail's auditing capabilities, I performed a controlled security-group modification.

An inbound rule was temporarily created allowing:

```text
Protocol: TCP
Port:     22 (SSH)
Source:   0.0.0.0/0
```

Allowing SSH from `0.0.0.0/0` permits connection attempts from any IPv4 address and represents an unnecessarily broad network exposure.

The rule was immediately removed after the test.

---

## Detection with CloudTrail

CloudTrail recorded the security-group modification as:

`AuthorizeSecurityGroupIngress`

The event record showed that TCP port 22 had been authorized from `0.0.0.0/0`.

CloudTrail also provided attribution information including the authenticated IAM identity, MFA status, event timestamp, AWS Region, API operation, and affected security group.

![SSH Exposure Detected](screenshots/02-ssh-exposure-detected.png)

---

## Remediation Verification

After the insecure rule was removed, CloudTrail recorded:

`RevokeSecurityGroupIngress`

The event confirmed that the same TCP port 22 rule allowing `0.0.0.0/0` had been revoked.

![SSH Exposure Remediated](screenshots/03-ssh-exposure-remediated.png)

### Incident Timeline

| Event | Time (UTC) |
|---|---|
| SSH rule authorized | 23:22:53 |
| SSH rule revoked | 23:23:36 |
| Exposure duration | **43 seconds** |

This demonstrated how CloudTrail can reconstruct the sequence of infrastructure changes during an investigation.

---

## Persistent Audit Logging

CloudTrail was configured to deliver logs to an Amazon S3 bucket.

I verified that compressed `.json.gz` CloudTrail log objects were successfully delivered under the regional and date-based S3 log hierarchy.

![CloudTrail S3 Logs](screenshots/04-cloudtrail-s3-logs.png)

This provides persistent audit records beyond the CloudTrail Event History interface.

---

## Audit Log Encryption

CloudTrail log objects stored in S3 were protected using server-side encryption with AWS Key Management Service keys (SSE-KMS).

I verified the encryption configuration directly from the properties of a delivered CloudTrail log object.

![CloudTrail KMS Encryption](screenshots/05-cloudtrail-kms-encryption.png)

Encrypting audit logs helps protect the confidentiality of security-sensitive activity records stored in S3.

---

## Log File Validation

CloudTrail log file validation was enabled for the trail.

I verified that CloudTrail generated digest objects under the `CloudTrail-Digest` S3 hierarchy.

![CloudTrail Digest Validation](screenshots/06-cloudtrail-digest-validation.png)

CloudTrail digest files provide integrity information that can be used to validate delivered log files and detect modification or deletion after delivery.

---

## Security Findings

The controlled investigation demonstrated that CloudTrail can answer several important incident-response questions:

| Question | Evidence |
|---|---|
| What happened? | Security-group ingress was modified |
| Which API operation caused it? | `AuthorizeSecurityGroupIngress` |
| What access was introduced? | TCP/22 from `0.0.0.0/0` |
| Who performed the action? | Authenticated IAM identity |
| Was MFA used? | Yes |
| Was the operation successful? | Yes |
| Was the change remediated? | Yes |
| Which event proved remediation? | `RevokeSecurityGroupIngress` |
| How long did the exposure exist? | 43 seconds |

---

## Security Considerations

Raw CloudTrail events can contain security-sensitive metadata such as:

- AWS account IDs
- IAM ARNs
- Source IP addresses
- Temporary access key IDs
- Resource identifiers
- Request parameters

For this public repository, sensitive and account-specific information shown in screenshots was redacted. Raw CloudTrail JSON records were not committed to the repository.

---

## Key Takeaways

This lab demonstrated several core cloud engineering and security concepts:

1. CloudTrail provides an audit history of AWS API activity.
2. Management events provide visibility into infrastructure and account-level operations.
3. CloudTrail can attribute configuration changes to authenticated AWS identities.
4. Persistent S3 delivery provides longer-term audit storage beyond Event History.
5. SSE-KMS can protect CloudTrail logs at rest.
6. Log file validation provides a mechanism for verifying the integrity of delivered audit logs.
7. CloudTrail records can be used to reconstruct infrastructure changes during security investigations.
8. Audit logging is an important complement to preventative controls such as IAM policies and security groups.

---

## Skills Demonstrated

- AWS CloudTrail configuration
- Cloud auditing and API activity analysis
- Amazon S3 log storage
- AWS KMS encryption
- CloudTrail log file validation
- EC2 security-group auditing
- IAM activity investigation
- Security incident reconstruction
- Cloud infrastructure monitoring
