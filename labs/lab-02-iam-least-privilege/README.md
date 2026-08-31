# Lab 2 – IAM & Least Privilege

## Executive Summary

Designed and implemented an enterprise-style AWS Identity and Access Management (IAM) environment using Role-Based Access Control (RBAC), customer-managed policies, AWS-managed policies, and dedicated test accounts to validate least-privilege access across Administrator, Developer, Security, and Auditor roles.
---

# Objective

Understand how AWS IAM evaluates permissions by creating custom policies, assigning them through IAM groups, and validating access using a dedicated test user.

---

# AWS Services

- AWS IAM
- AWS Console

---

# Skills Learned

- IAM Users
- IAM Groups
- Customer-Managed Policies
- AWS-Managed Policies
- JSON
- Principle of Least Privilege
- Role-Based Access Control (RBAC)
- Permission Validation
- IAM Policy Evaluation

---

# Security Concepts

## Least Privilege

Grant users only the permissions required to perform their jobs.

## Role-Based Access Control (RBAC)

Permissions are assigned to groups rather than directly to users.

## Customer-Managed Policies

Organizations create custom IAM policies instead of relying solely on AWS-managed policies.

---

# Policy Created

### Developer-S3-Basic-Access
- Customer-managed IAM policy allowing developers to list S3 buckets and upload/download objects while following the Principle of Least Privilege.

### Security-Operations-ReadOnly
- Customer-managed IAM policy providing read-only access to AWS security services including CloudTrail, GuardDuty, AWS Config, Security Hub, and CloudWatch Logs.

### ReadOnlyAccess (AWS Managed)
- AWS-managed policy attached to the Auditors group to provide enterprise-wide read-only visibility for auditing and compliance.

---

# Validation

## Enterprise IAM Environment

The AWS account now follows a role-based access control (RBAC) model with four distinct job functions.

| Group | Purpose | Status |
|--------|---------|:------:|
| Administrators | Full AWS administration | ✅ |
| Developers | Application development using least privilege | ✅ |
| Security | Security monitoring and investigation | ✅ |
| Auditors | Read-only access for compliance and review | ✅ |

Dedicated IAM test accounts were created to validate each role and verify that permissions behaved as expected.
---
## Enterprise Architecture

```text
AWS Account
│
├── Administrators
│   └── dylan-admin
│
├── Developers
│   └── developer1
│
├── Auditors
│   └── auditor1
│
└── Security
    └── security1
```
## Key Takeaways

- Enterprise IAM should separate permissions by job function.
- Least privilege reduces the attack surface of cloud environments.
- Customer-managed IAM policies provide greater flexibility than AWS-managed policies.
- IAM policies should always be validated using dedicated test accounts.

---

## Future Improvements

- Replace AWS-managed ReadOnlyAccess with a custom Auditor policy.
- Restrict S3 permissions to specific bucket ARNs instead of using "*".
- Implement IAM conditions for additional security controls.
- Integrate IAM roles with future AWS services.

---

# Resume Impact

- Enterprise IAM Design
- Role-Based Access Control (RBAC)
- Least Privilege Implementation
- Customer-Managed IAM Policies
- Security Validation
- Cloud Security Documentation

---

## Reflection

This lab demonstrated that secure cloud environments rely on well-designed identity and access management. Separating permissions by job function and validating authorization through dedicated test accounts reinforced the importance of the Principle of Least Privilege and Role-Based Access Control in enterprise AWS environments.
