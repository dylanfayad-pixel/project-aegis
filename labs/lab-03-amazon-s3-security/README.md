# Lab 3 – Amazon S3 Security

## Executive Summary

Designed and secured an Amazon S3 bucket following AWS security best practices by implementing Block Public Access, Bucket Versioning, Server-Side Encryption (SSE-S3), and resource-based bucket policies. Validated authorization behavior through IAM policy integration and explicit Deny testing.

---

## Objective

Build and secure an Amazon S3 bucket while understanding object storage, bucket policies, and AWS authorization.

---

## AWS Services

- Amazon S3

---

## Skills Learned

- Amazon S3
- Buckets
- Objects
- Server-Side Encryption
- Bucket Versioning
- Block Public Access
- Bucket Policies
- IAM Integration
- Resource-Based Policies
- Explicit Deny

---

## Security Concepts

### Buckets

Containers that store objects.

### Objects

Individual files stored inside buckets.

### Block Public Access

Protects buckets from accidental public exposure.

### Versioning

Protects against accidental deletion and overwrites.

### Server-Side Encryption

Automatically encrypts objects at rest.

### Bucket Policies

Resource-based policies controlling access directly to the bucket.

### Explicit Deny

Explicit Deny always overrides Allow during AWS authorization.

---

## Validation

### Successful

- Secure bucket created
- Versioning enabled
- Default encryption enabled
- Block Public Access enabled
- Uploaded objects successfully
- Bucket policy validated
- Explicit Deny tested successfully

---

## Lessons Learned

- Buckets store objects.
- IAM and Bucket Policies work together.
- Explicit Deny overrides Allow.
- Default encryption protects data at rest.
- Versioning protects against accidental data loss.
