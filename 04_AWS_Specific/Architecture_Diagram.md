# EcoMetricx AWS Architecture Diagram

**Version:** 1.0  
**Date:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              EXTERNAL USERS                                  │
│         Customers  │  Employees  │  Partners  │  Public Website             │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  │ HTTPS (TLS 1.2+)
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           IDENTITY LAYER                                     │
│                                                                              │
│  ┌──────────────────────┐     ┌──────────────────────┐                     │
│  │   Microsoft Entra ID │────▶│    AWS IAM           │                     │
│  │   (Azure AD / M365)  │     │    Identity Center   │                     │
│  │   - SSO              │     │    - 34 users        │                     │
│  │   - MFA              │     │    - Role-based      │                     │
│  └──────────────────────┘     └──────────────────────┘                     │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                      AWS ACCOUNT: 302146782327                               │
│                           Region: us-east-1                                  │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                        VPC / NETWORKING                             │    │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐         │    │
│  │  │ Public       │    │ Private      │    │ Private      │         │    │
│  │  │ Subnet       │    │ Subnet       │    │ Subnet       │         │    │
│  │  │ (ALB, NAT)   │    │ (EC2, ECS)   │    │ (RDS)        │         │    │
│  │  └──────────────┘    └──────────────┘    └──────────────┘         │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      COMPUTE LAYER                                   │   │
│  │                                                                       │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────────────┐ │   │
│  │  │    EC2      │  │    ECS      │  │   Lambda    │  │  Amplify   │ │   │
│  │  │ 11 instances│  │  Clusters   │  │  Functions  │  │  Apps      │ │   │
│  │  │ - t2/t3     │  │             │  │             │  │            │ │   │
│  │  │ - r6a/r6g   │  │             │  │             │  │            │ │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └────────────┘ │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      DATA LAYER                                       │   │
│  │                                                                       │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────────────┐ │   │
│  │  │     RDS     │  │     S3      │  │   Glue      │  │  Redshift  │ │   │
│  │  │  4 DBs      │  │  54 buckets │  │   ETL       │  │ (optional) │ │   │
│  │  │ - MySQL     │  │ - Raw       │  │             │  │            │ │   │
│  │  │ - Aurora    │  │ - Curated   │  │             │  │            │ │   │
│  │  │ Encrypted   │  │ Encrypted   │  │             │  │            │ │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └────────────┘ │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    ML / ANALYTICS LAYER                              │   │
│  │                                                                       │   │
│  │  ┌─────────────────────────┐  ┌─────────────────────────┐          │   │
│  │  │       SageMaker         │  │      Data Pipeline      │          │   │
│  │  │   - Model Training      │  │   - Step Functions      │          │   │
│  │  │   - Model Deployment    │  │   - EventBridge         │          │   │
│  │  │   - Inference           │  │                         │          │   │
│  │  └─────────────────────────┘  └─────────────────────────┘          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                  SECURITY / MONITORING                               │   │
│  │                                                                       │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌───────────────┐ │   │
│  │  │ CloudTrail │  │ CloudWatch │  │ Security   │  │ AWS Config    │ │   │
│  │  │  API Logs  │  │  Metrics   │  │ Groups     │  │ (Pending)     │ │   │
│  │  │  Validated │  │  Alarms    │  │  + IAM     │  │               │ │   │
│  │  └────────────┘  └────────────┘  └────────────┘  └───────────────┘ │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SUPPORTING SERVICES                                  │
│                                                                              │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────────────────┐   │
│  │    GitHub      │  │   Confluence   │  │         Slack              │   │
│  │  Source Code   │  │  Documentation │  │     Communication          │   │
│  │  CI/CD         │  │  Policies      │  │                            │   │
│  └────────────────┘  └────────────────┘  └────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## RDS Database Architecture

```
                        ┌─────────────────────────────────────┐
                        │           RDS INSTANCES              │
                        │                                      │
    ┌───────────────────┼───────────────────┼─────────────────┤
    │                   │                   │                 │
    ▼                   ▼                   ▼                 ▼
┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│ database-   │   │ ecmx-       │   │ ecometricx- │   │ vox-        │
│ neurova     │   │ tenore      │   │ serverless- │   │ instance-1  │
│             │   │             │   │ rds-inst-1  │   │             │
│ MySQL       │   │ MySQL       │   │ Aurora MySQL│   │ Aurora MySQL│
│ Encrypted   │   │ Encrypted   │   │ Encrypted   │   │ Encrypted   │
│ Backup: 1d  │   │ Backup: 1d  │   │ Backup: 7d  │   │ Backup: 1d  │
│ Single-AZ   │   │ Single-AZ   │   │ Single-AZ   │   │ Single-AZ   │
└─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘
       │                 │                 │                 │
       └─────────────────┴────────┬────────┴─────────────────┘
                                  │
                                  ▼
                        ┌─────────────────────┐
                        │  Security Groups    │
                        │  (Access Control)   │
                        └─────────────────────┘
```

---

## S3 Data Lake Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            S3 DATA LAKE (54 Buckets)                         │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         RAW LAYER                                    │   │
│  │  - Source data ingestion                                             │   │
│  │  - Original format preservation                                       │   │
│  │  - Data classification: Confidential                                  │   │
│  │  Buckets: *-raw-*, axentra-webhook-*, external-data-*                │   │
│  └──────────────────────────────┬──────────────────────────────────────┘   │
│                                  │ Glue ETL                                 │
│                                  ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                       CURATED LAYER                                  │   │
│  │  - Cleaned and transformed data                                       │   │
│  │  - Standardized formats (Parquet)                                     │   │
│  │  - Data classification: Confidential                                  │   │
│  │  Buckets: *-curated-*, *-processed-*                                 │   │
│  └──────────────────────────────┬──────────────────────────────────────┘   │
│                                  │ Glue ETL / Lambda                       │
│                                  ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                       REFINED LAYER                                  │   │
│  │  - Analytics-ready datasets                                           │   │
│  │  - Aggregated views                                                   │   │
│  │  - Data classification: Confidential                                  │   │
│  │  Buckets: britbox-prod-*, *-analytics-*                              │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      OPERATIONAL BUCKETS                             │   │
│  │  - ecometricx-cloudtrail-bucket (Audit Logs)                         │   │
│  │  - Amplify deployment buckets                                         │   │
│  │  - Backup buckets                                                     │   │
│  │  - SageMaker model artifacts                                          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  Security Controls:                                                          │
│  ✓ Encryption at rest (AES-256 / SSE-S3)                                   │
│  ✓ Public access blocked (default)                                          │
│  ✓ Bucket policies (least privilege)                                        │
│  ✓ Versioning enabled (critical buckets)                                    │
│  ✓ Lifecycle policies (retention)                                           │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Security Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SECURITY CONTROLS                                    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      IDENTITY & ACCESS                               │   │
│  │                                                                       │   │
│  │  Microsoft Entra ID ──SSO──▶ AWS IAM                                │   │
│  │         │                        │                                    │   │
│  │         │ MFA                    │ IAM Policies                       │   │
│  │         │                        │                                    │   │
│  │         ▼                        ▼                                    │   │
│  │  ┌─────────────┐          ┌─────────────────┐                       │   │
│  │  │ Authenticator│          │ 34 IAM Users    │                       │   │
│  │  │ (TOTP/Push) │          │ Role-Based      │                       │   │
│  │  └─────────────┘          │ Least Privilege │                       │   │
│  │                           └─────────────────┘                       │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      NETWORK SECURITY                                │   │
│  │                                                                       │   │
│  │  Internet ──▶ WAF ──▶ ALB ──▶ Security Groups ──▶ EC2/ECS          │   │
│  │                                      │                                │   │
│  │                                      │ Restrict                       │   │
│  │                                      ▼                                │   │
│  │                               ┌─────────────┐                        │   │
│  │                               │     RDS     │                        │   │
│  │                               │ (No public) │                        │   │
│  │                               └─────────────┘                        │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                       DATA PROTECTION                                │   │
│  │                                                                       │   │
│  │  At Rest:                        In Transit:                         │   │
│  │  ├─ S3: SSE-S3 (AES-256)        ├─ TLS 1.2+ required                │   │
│  │  ├─ RDS: KMS encryption         ├─ HTTPS for all APIs               │   │
│  │  ├─ EBS: KMS encryption         └─ Database SSL connections         │   │
│  │  └─ Secrets Manager                                                   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                   LOGGING & MONITORING                               │   │
│  │                                                                       │   │
│  │  CloudTrail ──▶ S3 (ecometricx-cloudtrail-bucket)                   │   │
│  │       │              │                                                │   │
│  │       │              ▼                                                │   │
│  │       │        Log Validation ✓                                      │   │
│  │       │                                                               │   │
│  │  CloudWatch ──▶ Alarms ──▶ SNS ──▶ Notification                     │   │
│  │       │                                                               │   │
│  │       ▼                                                               │   │
│  │  Metrics Dashboard                                                    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## SOC 2 Trust Service Criteria Mapping

| Architecture Component | SOC 2 Criteria | Controls |
|------------------------|----------------|----------|
| IAM / Microsoft Entra | CC6.2 | Access Control |
| Security Groups / VPC | CC6.1, CC6.6 | Network Security |
| CloudTrail | CC7.1 | Audit Logging |
| CloudWatch Alarms | CC7.2 | Incident Detection |
| S3/RDS Encryption | CC6.6, CC6.7 | Data Protection |
| RDS Backups | A1.2 | Availability |
| Multi-Region | A1.1 | Availability |

---

**Document Control:** Version 1.0 | December 2025

