# System Description Document

**Document Version:** 1.0  
**Date:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Purpose:** SOC 2 Type II Audit Readiness

---

## 1. Company Overview

### 1.1 Organization

**Company Name:** EcoMetricx LLC  
**Industry:** Energy Analytics and Data Science  
**Headquarters:** Remote (distributed workforce)  
**Primary Contact:** security@ecometricx.com

### 1.2 Services Provided

EcoMetricx provides energy analytics and data science services including:
- Energy consumption analysis
- Predictive analytics
- Machine learning models for energy optimization
- Data processing and transformation

### 1.3 Organizational Structure

EcoMetricx operates as a fully remote company with:
- No physical headquarters
- Distributed workforce
- Cloud-based infrastructure (AWS)
- Microsoft 365 as primary identity provider

---

## 2. Infrastructure Description

### 2.1 Cloud Platform

**Primary Cloud Provider:** Amazon Web Services (AWS)
- **Account ID:** 302146782327
- **Primary Region:** us-east-1
- **Multi-Region:** Yes

### 2.2 AWS Services in Scope

| Service | Purpose | Data Classification |
|---------|---------|---------------------|
| EC2 | Compute instances (11) | Internal/Confidential |
| S3 | Object storage (54 buckets) | All levels |
| RDS | Managed databases (4) | Confidential |
| Lambda | Serverless compute | Internal |
| CloudFormation | Infrastructure as code | Internal |
| CloudWatch | Monitoring and logging | Internal |
| CloudTrail | API audit logging | Internal |
| IAM | Identity and access management | Restricted |
| Amplify | Application hosting | Internal |
| SageMaker | Machine learning | Confidential |
| ECS | Container orchestration | Internal |
| Glue | ETL services | Confidential |

### 2.3 Supporting Infrastructure

| Provider | Service | Purpose |
|----------|---------|---------|
| Microsoft 365 | Identity, Email, Collaboration | Primary identity provider, MFA |
| GitHub | Source Code Management | Version control, CI/CD |
| Atlassian Confluence | Documentation | Knowledge base, policies |
| Slack | Communication | Team collaboration |

---

## 3. Data Flow Overview

### 3.1 Authentication Flow

```
User → Microsoft 365 (MFA) → AWS Console/Applications
         ↓
    Microsoft Authenticator (2FA)
```

### 3.2 Data Processing Flow

```
Data Sources (APIs, Files)
         ↓
    S3 (Raw Data Layer)
         ↓
    Glue ETL / Lambda (Processing)
         ↓
    RDS/S3 (Curated Data)
         ↓
    SageMaker (ML Models)
         ↓
    Applications (Amplify, EC2)
```

### 3.3 Critical Dependencies

Per Domain Services BIA (October 2025):
- Microsoft 365 is the single point of failure for authentication
- All business systems depend on Microsoft 365 SSO
- RTO: 1 hour (minimum communications), 8 hours (baseline operations)

---

## 4. People and Roles

### 4.1 Key Personnel

| Role | Name | Email |
|------|------|-------|
| InfoSec Team Lead | Andrei Ionete | andrei@ecometricx.com |
| AWS Tagging Standards Owner | Conrad Jahn | - |
| Primary AWS Administrator | Alex Ledbetter | - |

### 4.2 IAM User Summary

Per AWS Access Control Matrix:
- **Individual Users:** 22 accounts
- **Service Accounts:** 8 accounts
- **Application Accounts:** 4 accounts
- **Total:** 34 IAM users

### 4.3 Access Control

- Multi-factor authentication required (Microsoft Authenticator)
- Role-based access control (RBAC)
- Quarterly access reviews
- Least privilege principle applied

---

## 5. Policies and Procedures

### 5.1 Policies in Place

| Policy | Version | Last Updated |
|--------|---------|--------------|
| Information Security Policy | 1.0 | December 2025 |
| Access Control Policy | 1.0 | December 2025 |
| Data Classification & Handling Policy | 1.0 | December 2025 |
| Acceptable Use Policy | 1.0 | October 2025 |
| Acceptable Encryption Standard Policy | 1.0 | April 2025 |
| Privacy Management Policy | 1.0 | April 2025 |
| Vendor Management Policy | 1.0 | December 2025 |
| Change Management Policy | 1.0 | December 2025 |
| Incident Response Policy | 1.0 | December 2025 |
| Logging and Monitoring Policy | 1.0 | December 2025 |
| Processing Integrity Policy | 1.0 | December 2025 |
| Vulnerability Management Policy | 1.0 | December 2025 |
| Clean Desk/Clear Screen Policy | 1.0 | December 2025 |
| Public Privacy Policy | 1.0 | December 2025 |

### 5.2 Procedures in Place

| Procedure | Version | Last Updated |
|-----------|---------|--------------|
| Standard Operating Procedure - Documentation Review | 1.0 | December 2025 |
| Data Quality Assurance Procedures | 1.0 | December 2025 |
| Vendor Assessment Questionnaire | 1.0 | December 2025 |
| Offboarding Security Checklist | 1.0 | December 2025 |
| Background Check Procedures | 1.0 | December 2025 |

### 5.3 Plans in Place

| Plan | Version | Last Updated |
|------|---------|--------------|
| Business Continuity Plan | 1.0 | December 2025 |
| Training Plan | 1.0 | December 2025 |
| Business Impact Analysis (Microsoft 365) | 1.0 | October 2025 |

---

## 6. Security Controls Summary

### 6.1 Access Control (CC6.2)

- IAM policies enforce access controls
- MFA required for all users
- Quarterly access reviews
- Immediate revocation upon termination

### 6.2 Change Management (CC6.7)

- Infrastructure as code (CloudFormation)
- Version control (GitHub)
- Change approval workflows
- CloudTrail logging of all changes

### 6.3 System Monitoring (CC7.1)

- CloudWatch alarms configured
- CloudTrail for API logging
- Log file validation enabled (December 2025)
- CloudWatch Logs for application logging

### 6.4 Incident Response (CC7.2)

- Incident Response Policy in place
- AWS-specific incident procedures documented
- 24-72 hour breach notification requirements

### 6.5 System Backup (CC7.3)

- S3 versioning enabled
- RDS automated backups (1-7 days retention)
- AMI snapshots for EC2 instances

### 6.6 Encryption (CC6.6)

- S3: AES-256 encryption at rest
- RDS: Encryption at rest enabled
- Transit: TLS 1.2+ required

---

## 7. Boundaries and Exclusions

### 7.1 In Scope

- EcoMetricx AWS account (302146782327)
- All production and development environments
- All services listed in Section 2.2
- All personnel with system access

### 7.2 Out of Scope

- Client systems and infrastructure
- Third-party SaaS applications (managed by providers)
- Physical security (remote workforce, no data centers)

---

## 8. Subservice Organizations

| Organization | Services | Controls Reliance |
|--------------|----------|-------------------|
| Amazon Web Services | Cloud infrastructure | SOC 2 Type II, ISO 27001 |
| Microsoft | Identity, email | SOC 2 Type II, ISO 27001 |

**Note:** EcoMetricx relies on AWS and Microsoft for physical security controls and certain logical security controls as documented in their SOC 2 reports.

---

## 9. Recent Changes

| Date | Change | Impact |
|------|--------|--------|
| Dec 2025 | CloudTrail log file validation enabled | Improved audit evidence integrity |
| Dec 2025 | Processing Integrity Policy created | SOC 2 PI criteria coverage |
| Dec 2025 | Security audits completed (S3, EC2, RDS) | Baseline documentation |
| Oct 2025 | Microsoft 365 BIA completed | DR planning improved |

---

## 10. Contact Information

**Security Inquiries:** security@ecometricx.com  
**InfoSec Lead:** Andrei Ionete (andrei@ecometricx.com)  
**Audit Contact:** [TBD]

---

**Document Control:** Version 1.0 | December 2025 | Next Review: Q1 2026

