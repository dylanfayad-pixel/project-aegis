# Lab 2 – IAM & Least Privilege

## Executive Summary

Designed, implemented, and validated a customer-managed AWS IAM policy following the Principle of Least Privilege. Permissions were verified using a dedicated test account to confirm expected authorization behavior.

---

# Objective

Understand how AWS IAM evaluates permissions by creating custom policies, assigning them through IAM groups, and validating access using a dedicated test user.

---

# AWS Services

- AWS IAM

---

# Skills Learned

- IAM Users
- IAM Groups
- Customer-Managed Policies
- JSON
- Principle of Least Privilege
- Permission Validation

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
# Lessons Learned

IAM policies should always be validated using test accounts instead of assuming permissions behave as expected.

---

# Resume Impact

This sprint demonstrates practical experience designing and validating least-privilege IAM policies using AWS IAM.
