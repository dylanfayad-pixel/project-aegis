# Lab 4 – Amazon VPC & Network Architecture

## Executive Summary

Designed and implemented a production-style Amazon Virtual Private Cloud (VPC) using multiple Availability Zones, public and private subnets, route tables, an Internet Gateway, and Security Groups following AWS networking best practices.

---

# Objective

Understand AWS networking by designing a secure Virtual Private Cloud (VPC) that separates public-facing resources from private backend resources.

---

# AWS Services

- Amazon VPC
- Internet Gateway
- Route Tables
- Security Groups

---

# Skills Learned

- Amazon VPC
- CIDR Blocks
- Public Subnets
- Private Subnets
- Internet Gateway
- Route Tables
- Security Groups
- Availability Zones
- Network Segmentation

---

# Security Concepts

## Virtual Private Cloud (VPC)

An isolated virtual network inside AWS.

## Public Subnet

A subnet capable of communicating with the Internet.

## Private Subnet

A subnet isolated from direct Internet access.

## Internet Gateway

Allows Internet connectivity for public resources.

## Security Groups

Instance-level virtual firewall.

## Network ACLs

Subnet-level virtual firewall.

---

# Architecture

```text
                    Internet
                        │
                Internet Gateway
                        │
        ┌───────────────┴───────────────┐
        │                               │
 Public Subnet A                  Public Subnet B
        │                               │
   Future EC2                     Future EC2

        │                               │

 Private Subnet A                 Private Subnet B
        │                               │
 Future Database             Future Database
```

---

# Validation

Successfully created:

- Amazon VPC
- Two Public Subnets
- Two Private Subnets
- Internet Gateway
- Route Tables
- Security Group

Configured Security Group rules:

- HTTP (80)
- HTTPS (443)
- SSH (22 restricted to My IP)

---

# Lessons Learned

- Public resources belong in public subnets.
- Databases belong in private subnets.
- Security Groups protect instances.
- Network ACLs protect subnets.
- VPCs provide network isolation inside AWS.
