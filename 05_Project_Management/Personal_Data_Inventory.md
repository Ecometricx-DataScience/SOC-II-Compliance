# Personal Data Inventory

**Version:** 1.0  
**Last Updated:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Annually

---

## Overview

This inventory documents personal data processed by EcoMetricx to support privacy compliance (GDPR, CCPA) and SOC 2 Privacy criteria.

---

## Data Inventory Summary

| Category | Data Elements | Classification | Primary System | Legal Basis |
|----------|---------------|----------------|----------------|-------------|
| Customer Contact | Name, Email, Phone | Confidential | AWS RDS, Microsoft 365 | Contract |
| Employee Data | Name, SSN, Address | Restricted | Microsoft 365, HR Systems | Legal/Contract |
| User Credentials | Usernames, Passwords (hashed) | Restricted | AWS IAM, Microsoft Entra | Contract |
| Usage Analytics | IP Address, Browser, Activity | Internal | AWS CloudWatch | Legitimate Interest |
| Business Data | Energy consumption data | Confidential | AWS RDS, S3 | Contract |

---

## Detailed Inventory

### 1. Customer Data

| Data Element | Classification | Collection Method | Storage Location | Retention | Legal Basis |
|--------------|----------------|-------------------|------------------|-----------|-------------|
| Full Name | Confidential | Direct input | RDS (ecmx-tenore) | Contract + 7 years | Contract |
| Email Address | Confidential | Direct input | RDS, Microsoft 365 | Contract + 7 years | Contract |
| Phone Number | Confidential | Direct input | RDS | Contract + 7 years | Contract |
| Company Name | Internal | Direct input | RDS | Contract + 7 years | Contract |
| Billing Address | Confidential | Direct input | RDS | Contract + 7 years | Contract |

**Data Flow:** Customer Portal → AWS Amplify → RDS → Analytics (S3/Glue)

### 2. Employee Data

| Data Element | Classification | Collection Method | Storage Location | Retention | Legal Basis |
|--------------|----------------|-------------------|------------------|-----------|-------------|
| Full Name | Confidential | HR onboarding | Microsoft 365 | Employment + 7 years | Legal |
| Social Security Number | Restricted | HR onboarding | HR System | Employment + 7 years | Legal |
| Home Address | Confidential | HR onboarding | HR System | Employment + 7 years | Legal |
| Bank Account (payroll) | Restricted | HR onboarding | HR System | Employment + 7 years | Legal |
| Emergency Contact | Confidential | HR onboarding | HR System | Employment + 7 years | Consent |
| Performance Data | Confidential | Manager input | Microsoft 365 | Employment + 7 years | Legitimate Interest |

**Data Flow:** HR System → Microsoft 365 → Payroll Processor

### 3. System/Technical Data

| Data Element | Classification | Collection Method | Storage Location | Retention | Legal Basis |
|--------------|----------------|-------------------|------------------|-----------|-------------|
| IP Address | Internal | Automatic | CloudWatch Logs | 90 days | Legitimate Interest |
| Browser/Device Info | Internal | Automatic | CloudWatch Logs | 90 days | Legitimate Interest |
| Login Timestamps | Internal | Automatic | CloudTrail | 1 year | Legitimate Interest |
| API Access Logs | Internal | Automatic | CloudTrail | 1 year | Legitimate Interest |

**Data Flow:** User Activity → CloudWatch/CloudTrail → S3 (archived)

### 4. Marketing Data

| Data Element | Classification | Collection Method | Storage Location | Retention | Legal Basis |
|--------------|----------------|-------------------|------------------|-----------|-------------|
| Email Address | Internal | Opt-in form | Marketing Platform | Until unsubscribe | Consent |
| Name | Internal | Opt-in form | Marketing Platform | Until unsubscribe | Consent |
| Preferences | Internal | User selection | Marketing Platform | Until unsubscribe | Consent |

---

## Data Processing Activities

### Activity 1: Customer Service Delivery

| Attribute | Value |
|-----------|-------|
| Purpose | Provide energy analytics services |
| Data Categories | Customer contact, business data |
| Data Subjects | Customers, end users |
| Recipients | Internal teams, authorized vendors |
| Transfers | US (AWS us-east-1) |
| Retention | Contract duration + 7 years |
| Security | Encryption, access controls |

### Activity 2: HR Administration

| Attribute | Value |
|-----------|-------|
| Purpose | Employee management, payroll |
| Data Categories | Employee personal data |
| Data Subjects | Employees, contractors |
| Recipients | HR team, payroll processor |
| Transfers | US |
| Retention | Employment + 7 years |
| Security | Encryption, restricted access |

### Activity 3: Security Monitoring

| Attribute | Value |
|-----------|-------|
| Purpose | System security, compliance |
| Data Categories | Technical data, access logs |
| Data Subjects | All system users |
| Recipients | InfoSec team |
| Transfers | US (AWS) |
| Retention | 90 days - 1 year |
| Security | Encryption, limited access |

---

## Third-Party Data Sharing

| Recipient | Purpose | Data Shared | Agreement |
|-----------|---------|-------------|-----------|
| AWS | Infrastructure | All data (encrypted) | DPA |
| Microsoft | Identity, email | Employee data | DPA |
| Payroll Provider | Compensation | Employee financial data | DPA + BAA |
| [Other vendors] | [Purpose] | [Data] | [Agreement] |

---

## Data Subject Rights

### Request Handling Process

1. Request received at privacy@ecometricx.com
2. Identity verified within 3 days
3. Request processed within 30 days
4. Response sent to data subject
5. Request logged and documented

### Request Statistics (Template)

| Quarter | Access | Rectification | Erasure | Total |
|---------|--------|---------------|---------|-------|
| Q1 2026 | | | | |
| Q2 2026 | | | | |

---

## AWS Data Storage Locations

Based on AWS_Security_Configuration_Documentation.md:

### RDS Databases

| Database | Personal Data | Classification | Encryption |
|----------|---------------|----------------|------------|
| database-neurova | Customer data | Confidential | AES-256 |
| ecmx-tenore | Customer data | Confidential | AES-256 |
| ecometricx-serverless-rds-instance-1 | Customer data | Confidential | AES-256 |
| vox-instance-1 | Analytics | Confidential | AES-256 |

### S3 Buckets (Data-Related)

| Bucket Pattern | Personal Data | Classification | Encryption |
|----------------|---------------|----------------|------------|
| *-prod-* buckets | May contain | Confidential | AES-256 |
| CloudTrail bucket | IP addresses, user IDs | Internal | AES-256 |
| Backup buckets | All data types | Per source | AES-256 |

---

## Compliance Mapping

| Regulation | Requirement | Status |
|------------|-------------|--------|
| GDPR Art. 30 | Records of processing | This document |
| GDPR Art. 15 | Right to access | Procedures in place |
| GDPR Art. 17 | Right to erasure | Procedures in place |
| CCPA 1798.100 | Notice at collection | Privacy notices |
| SOC 2 P1-P8 | Privacy criteria | Policies in place |

---

## Action Items

- [ ] Complete vendor data processing agreements
- [ ] Implement automated data subject request tracking
- [ ] Map all S3 buckets containing personal data
- [ ] Define data retention automation procedures
- [ ] Annual inventory review and update

---

**Document Control:** Version 1.0 | December 2025 | Next Review: December 2026

