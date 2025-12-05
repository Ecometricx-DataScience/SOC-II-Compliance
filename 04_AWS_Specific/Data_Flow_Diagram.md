# EcoMetricx Data Flow Diagrams

**Version:** 1.0  
**Date:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Source:** AWS Tagging Standards Policy (Data Lineage), AWS Configuration

---

## 1. Customer Data Flow

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                        CUSTOMER DATA FLOW                                     │
│                                                                               │
│  ┌─────────────┐                                                             │
│  │  Customer   │                                                             │
│  │  Portal     │                                                             │
│  └──────┬──────┘                                                             │
│         │ HTTPS (TLS 1.2+)                                                   │
│         │ Classification: Confidential                                        │
│         ▼                                                                     │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐               │
│  │   Amplify   │──────▶│   Lambda    │──────▶│     RDS     │               │
│  │  Frontend   │       │    API      │       │  Database   │               │
│  │             │       │             │       │  (MySQL/    │               │
│  │             │       │ Validation  │       │   Aurora)   │               │
│  └─────────────┘       │ AuthZ/AuthN │       │             │               │
│                        └──────┬──────┘       │ Encrypted   │               │
│                               │              └──────┬──────┘               │
│                               │                     │                        │
│                               │                     │ Scheduled ETL          │
│                               │                     ▼                        │
│                               │              ┌─────────────┐               │
│                               │              │    Glue     │               │
│                               │              │    ETL      │               │
│                               │              └──────┬──────┘               │
│                               │                     │                        │
│                               ▼                     ▼                        │
│                        ┌─────────────┐       ┌─────────────┐               │
│                        │     S3      │       │     S3      │               │
│                        │  Raw Data   │──────▶│  Curated    │               │
│                        │             │       │   Data      │               │
│                        └─────────────┘       └──────┬──────┘               │
│                                                     │                        │
│                                                     ▼                        │
│                                              ┌─────────────┐               │
│                                              │  SageMaker  │               │
│                                              │  Analytics  │               │
│                                              └─────────────┘               │
└──────────────────────────────────────────────────────────────────────────────┘

Data Classification at Each Stage:
- Input (Customer Portal): Confidential
- Processing (Lambda/Glue): Confidential
- Storage (RDS/S3): Confidential, Encrypted
- Analytics (SageMaker): Confidential
```

---

## 2. Authentication Data Flow

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                      AUTHENTICATION DATA FLOW                                 │
│                                                                               │
│  ┌─────────────┐       ┌─────────────────────┐       ┌─────────────┐       │
│  │    User     │──────▶│   Microsoft 365     │──────▶│   AWS IAM   │       │
│  │             │       │   (Entra ID)        │       │             │       │
│  └─────────────┘       │                     │       │             │       │
│        │               │  ┌───────────────┐  │       │  ┌───────┐  │       │
│        │               │  │ SSO Redirect  │  │       │  │ Role  │  │       │
│        ▼               │  └───────┬───────┘  │       │  │Assume │  │       │
│  ┌─────────────┐       │          │          │       │  └───┬───┘  │       │
│  │   MFA       │       │          ▼          │       │      │      │       │
│  │ Challenge   │◀──────│  ┌───────────────┐  │       │      │      │       │
│  │             │       │  │ SAML Response │──┼──────▶│      │      │       │
│  │ Microsoft   │       │  └───────────────┘  │       │      ▼      │       │
│  │Authenticator│       │                     │       │ ┌────────┐  │       │
│  └─────────────┘       └─────────────────────┘       │ │Session │  │       │
│                                                       │ │ Token  │  │       │
│                                                       │ └────┬───┘  │       │
│                                                       └──────┼──────┘       │
│                                                              │               │
│                                                              ▼               │
│                                                       ┌─────────────┐       │
│                                                       │ AWS Console │       │
│                                                       │ or CLI      │       │
│                                                       └─────────────┘       │
│                                                                               │
│  Data Classification:                                                         │
│  - Credentials: Restricted (never stored in plain text)                      │
│  - Session Tokens: Restricted (time-limited)                                 │
│  - Audit Logs: Internal (CloudTrail)                                         │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Data Lake ETL Flow

Based on AWS Tagging Standards Policy data lineage requirements:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                        DATA LAKE ETL FLOW                                     │
│                                                                               │
│  SOURCE SYSTEMS                                                               │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐                │
│  │ External  │  │ Customer  │  │ Internal  │  │ Partner   │                │
│  │   APIs    │  │ Uploads   │  │ Systems   │  │   APIs    │                │
│  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘                │
│        │              │              │              │                        │
│        └──────────────┼──────────────┼──────────────┘                        │
│                       │              │                                        │
│                       ▼              ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    RAW LAYER (Bronze)                                │   │
│  │  S3: {project}-{env}-s3-raw-{region}                                │   │
│  │                                                                       │   │
│  │  Tags:                                                                │   │
│  │  - SourceSystems: ExternalAPI,CustomerUpload                         │   │
│  │  - DataDomain: [Domain]                                              │   │
│  │  - DataClassification: Confidential                                  │   │
│  │  - ProcessOwner: DataEngineering                                     │   │
│  └──────────────────────────────┬──────────────────────────────────────┘   │
│                                  │                                          │
│                                  │ Glue Crawler + ETL Jobs                  │
│                                  ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                   CURATED LAYER (Silver)                             │   │
│  │  S3: {project}-{env}-s3-curated-{region}                            │   │
│  │                                                                       │   │
│  │  Tags:                                                                │   │
│  │  - SourceSystems: S3-Raw-Layer                                       │   │
│  │  - TargetSystems: S3-Refined,Redshift                               │   │
│  │  - DataDomain: [Domain]                                              │   │
│  │  - DataClassification: Confidential                                  │   │
│  │  - ProcessOwner: DataEngineering                                     │   │
│  └──────────────────────────────┬──────────────────────────────────────┘   │
│                                  │                                          │
│                                  │ Glue ETL + Aggregation                   │
│                                  ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                   REFINED LAYER (Gold)                               │   │
│  │  S3: {project}-{env}-s3-refined-{region}                            │   │
│  │                                                                       │   │
│  │  Tags:                                                                │   │
│  │  - SourceSystems: S3-Curated-Layer                                   │   │
│  │  - TargetSystems: Analytics,Dashboard,ML                            │   │
│  │  - DataDomain: [Domain]                                              │   │
│  │  - DataClassification: Confidential                                  │   │
│  │  - ProcessOwner: AnalyticsTeam                                       │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                  │                                          │
│        ┌─────────────────────────┼─────────────────────────┐                │
│        │                         │                         │                │
│        ▼                         ▼                         ▼                │
│  ┌───────────┐           ┌───────────┐           ┌───────────┐            │
│  │ SageMaker │           │ Dashboard │           │  Reports  │            │
│  │    ML     │           │  (BI)     │           │           │            │
│  └───────────┘           └───────────┘           └───────────┘            │
│                                                                               │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Audit Log Data Flow

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                        AUDIT LOG DATA FLOW                                    │
│                                                                               │
│  AWS API Activity                                                             │
│       │                                                                       │
│       ▼                                                                       │
│  ┌─────────────┐                                                             │
│  │ CloudTrail  │                                                             │
│  │             │                                                             │
│  │ - API calls │                                                             │
│  │ - Console   │                                                             │
│  │   logins    │                                                             │
│  │ - SDK calls │                                                             │
│  └──────┬──────┘                                                             │
│         │                                                                     │
│         ▼                                                                     │
│  ┌─────────────┐       ┌─────────────┐                                      │
│  │     S3      │──────▶│ Athena      │─────▶ Security Analysis              │
│  │ CloudTrail  │       │ Queries     │                                      │
│  │   Bucket    │       └─────────────┘                                      │
│  │             │                                                             │
│  │ Validated ✓ │       ┌─────────────┐                                      │
│  │ Encrypted   │──────▶│ CloudWatch  │─────▶ Alerts / Dashboards            │
│  └─────────────┘       │   Logs      │                                      │
│                        └─────────────┘                                      │
│                                                                               │
│  Application Logs                                                             │
│       │                                                                       │
│       ▼                                                                       │
│  ┌─────────────┐       ┌─────────────┐                                      │
│  │ CloudWatch  │──────▶│ CloudWatch  │─────▶ Operational Monitoring         │
│  │   Logs      │       │   Alarms    │                                      │
│  │             │       └─────────────┘                                      │
│  │ EC2/Lambda/ │                                                             │
│  │ ECS logs    │                                                             │
│  └─────────────┘                                                             │
│                                                                               │
│  Retention:                                                                   │
│  - CloudTrail: 1 year (minimum), 7 years (compliance)                        │
│  - CloudWatch Logs: 90 days (default), extended as needed                    │
│                                                                               │
│  Data Classification: Internal                                                │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Backup Data Flow

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                         BACKUP DATA FLOW                                      │
│                                                                               │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐               │
│  │     RDS     │──────▶│  Automated  │──────▶│   S3       │               │
│  │  Databases  │       │  Snapshots  │       │  Backup    │               │
│  │             │       │  (Daily)    │       │  Storage   │               │
│  │ 4 instances │       │             │       │  (Same     │               │
│  └─────────────┘       │ Retention:  │       │   Region)  │               │
│                        │ 1-7 days    │       │            │               │
│                        └─────────────┘       │ Encrypted  │               │
│                                              └─────────────┘               │
│                                                                               │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐               │
│  │     S3      │──────▶│ Versioning  │──────▶│ Cross-     │               │
│  │   Buckets   │       │  Enabled    │       │ Region     │               │
│  │             │       │             │       │ Replication│               │
│  │ 54 buckets  │       │ 30-day      │       │ (Critical  │               │
│  └─────────────┘       │ retention   │       │  buckets)  │               │
│                        └─────────────┘       └─────────────┘               │
│                                                                               │
│  ┌─────────────┐       ┌─────────────┐                                      │
│  │     EC2     │──────▶│    AMI      │                                      │
│  │  Instances  │       │  Snapshots  │                                      │
│  │             │       │  (Manual/   │                                      │
│  │ 11 instances│       │  Scheduled) │                                      │
│  └─────────────┘       └─────────────┘                                      │
│                                                                               │
│  Recovery Time Objectives (RTO):                                              │
│  - RDS: 1 hour (point-in-time recovery)                                      │
│  - S3: Near-instant (versioning)                                             │
│  - EC2: 2-4 hours (AMI restoration)                                          │
│                                                                               │
│  Recovery Point Objectives (RPO):                                             │
│  - RDS: 5 minutes (transaction logs)                                         │
│  - S3: Near-zero (versioning)                                                │
│  - EC2: Time since last AMI                                                  │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Data Classification Mapping

| Data Type | Classification | Encryption | Access Control |
|-----------|---------------|------------|----------------|
| Customer PII | Confidential | AES-256 | Need-to-know |
| Authentication | Restricted | AES-256 | Strict limit |
| Business Data | Confidential | AES-256 | Role-based |
| Audit Logs | Internal | AES-256 | InfoSec only |
| Public Content | Public | Optional | Unrestricted |

---

**Document Control:** Version 1.0 | December 2025

