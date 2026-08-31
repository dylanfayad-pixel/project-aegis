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

Developer-S3-Basic-Access

---

# Validation

A dedicated IAM user named **developer1** was created and assigned only to the Developers group.

### Successful

- Access Amazon S3
- View available buckets

### Blocked

- IAM Administration
- Bucket Creation

The observed behavior matched the intended least-privilege design.

---

# Lessons Learned

IAM policies should always be validated using test accounts instead of assuming permissions behave as expected.

---

# Resume Impact

This sprint demonstrates practical experience designing and validating least-privilege IAM policies using AWS IAM.
