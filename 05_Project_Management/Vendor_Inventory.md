# Vendor Inventory

**Version:** 1.0  
**Last Updated:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Quarterly

## Overview

This document maintains the inventory of all third-party vendors and service providers used by EcoMetricx, with risk classifications per the Vendor Management Policy.

## Critical Vendors

### Cloud Infrastructure

| Vendor | Service | Classification | Data Access | Certifications | Contract Expiry | Last Review |
|--------|---------|---------------|-------------|----------------|-----------------|-------------|
| Amazon Web Services (AWS) | Cloud Infrastructure | Critical | Confidential, Restricted | SOC 2, ISO 27001, HIPAA | [TBD] | Dec 2025 |
| Microsoft 365 | Identity, Email, Collaboration | Critical | Confidential | SOC 2, ISO 27001 | [TBD] | Dec 2025 |

### Development and Operations

| Vendor | Service | Classification | Data Access | Certifications | Contract Expiry | Last Review |
|--------|---------|---------------|-------------|----------------|-----------------|-------------|
| GitHub | Source Code Management | Critical | Confidential | SOC 2 | [TBD] | Dec 2025 |
| Atlassian (Confluence) | Documentation, Knowledge Base | Standard | Internal | SOC 2, ISO 27001 | [TBD] | Dec 2025 |

### Communication

| Vendor | Service | Classification | Data Access | Certifications | Contract Expiry | Last Review |
|--------|---------|---------------|-------------|----------------|-----------------|-------------|
| Slack | Team Communication | Standard | Internal | SOC 2, ISO 27001 | [TBD] | Dec 2025 |

## AWS Services Detail

Per AWS_Security_Configuration_Documentation.md:

| Service | Purpose | Data Classification | SOC 2 Criteria |
|---------|---------|--------------------|--------------------|
| EC2 | Compute (11 instances) | Internal/Confidential | CC6.7, CC7.1 |
| S3 | Storage (54 buckets) | All levels | CC6.6, CC7.3 |
| RDS | Databases (4 instances) | Confidential | CC6.6, CC7.3 |
| CloudTrail | API Logging | Internal | CC7.1 |
| CloudWatch | Monitoring | Internal | CC7.1 |
| IAM | Access Control | Restricted | CC6.2 |
| Lambda | Serverless Compute | Internal | CC6.7 |
| Glue | ETL Services | Confidential | CC6.6 |
| SageMaker | Machine Learning | Confidential | CC6.6 |
| Amplify | Application Hosting | Internal | CC6.7 |

## Standard Vendors

| Vendor | Service | Classification | Data Access | Certifications | Contract Expiry | Last Review |
|--------|---------|---------------|-------------|----------------|-----------------|-------------|
| [Add vendors as identified] | | | | | | |

## Low-Risk Vendors

| Vendor | Service | Classification | Data Access | Certifications | Contract Expiry | Last Review |
|--------|---------|---------------|-------------|----------------|-----------------|-------------|
| [Add vendors as identified] | | | | | | |

## Vendor Assessment Status

| Vendor | Assessment Status | Last Assessment | Next Assessment | Notes |
|--------|-------------------|-----------------|-----------------|-------|
| AWS | Complete | Dec 2025 | Dec 2026 | SOC 2 report reviewed |
| Microsoft 365 | Complete | Oct 2025 | Oct 2026 | BIA completed |
| GitHub | Pending | - | Q1 2026 | Request SOC 2 report |
| Atlassian | Pending | - | Q1 2026 | Request SOC 2 report |
| Slack | Pending | - | Q1 2026 | Request SOC 2 report |

## Contract Status

| Vendor | DPA/BAA Status | NDA Status | SLA Status | Notes |
|--------|----------------|------------|------------|-------|
| AWS | [Check] | N/A | Active | Standard AWS agreement |
| Microsoft 365 | [Check] | N/A | Active | Enterprise agreement |
| GitHub | [TBD] | [TBD] | [TBD] | Enterprise |
| Atlassian | [TBD] | [TBD] | [TBD] | Cloud |
| Slack | [TBD] | [TBD] | [TBD] | Enterprise |

## Data Flow Summary

Based on Domain Services BIA and AWS configuration:

```
Microsoft 365 (Identity)
    ↓
┌───────────────────────────────────────────────────┐
│                  EcoMetricx AWS                    │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐        │
│  │   EC2   │ → │   RDS   │ → │   S3    │        │
│  │(11 inst)│   │(4 DBs)  │   │(54 bkt) │        │
│  └─────────┘   └─────────┘   └─────────┘        │
│       ↓             ↓             ↓               │
│  ┌─────────────────────────────────────┐         │
│  │  Glue / SageMaker / Lambda / Amplify │         │
│  └─────────────────────────────────────┘         │
└───────────────────────────────────────────────────┘
    ↓
GitHub (Code) ← → Slack (Communication) ← → Confluence (Docs)
```

## Risk Assessment Summary

| Vendor | Risk Level | Key Risks | Mitigations |
|--------|------------|-----------|-------------|
| Microsoft 365 | High | Single point of failure for authentication | Workaround procedures documented (BIA) |
| AWS | Medium | Data residency, access control | Encryption, IAM policies, monitoring |
| GitHub | Medium | Source code access | Access controls, branch protection |
| Slack | Low | Communication data | SSO integration, encryption |
| Atlassian | Low | Documentation access | SSO integration |

## Action Items

1. [ ] Request SOC 2 reports from GitHub, Atlassian, Slack
2. [ ] Verify DPA/BAA status for all critical vendors
3. [ ] Complete vendor security assessments per schedule
4. [ ] Update contract expiry dates
5. [ ] Schedule next quarterly review

---

**Document Control:** Version 1.0 | December 2025 | Next Review: Q1 2026

