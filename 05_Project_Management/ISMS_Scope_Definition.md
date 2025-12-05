# Information Security Management System (ISMS) Scope Definition

**Version:** 1.0  
**Date:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Alignment:** ISO 27001:2022, SOC 2

---

## 1. Organization Context

### 1.1 Organization Profile

**Organization:** EcoMetricx LLC  
**Industry:** Energy Analytics and Data Science  
**Business Model:** Cloud-based SaaS services  
**Workforce:** Distributed remote workforce  
**Headquarters:** No physical headquarters (remote-first)

### 1.2 Business Activities

- Energy consumption data analysis
- Predictive analytics for energy optimization
- Machine learning model development and deployment
- Data processing and transformation services
- Custom data science solutions

### 1.3 Key Stakeholders

| Stakeholder | Interest |
|-------------|----------|
| Customers | Data security, service availability, privacy |
| Employees | Job security, clear policies, tools |
| Management | Compliance, risk management, reputation |
| Regulators | Legal compliance, data protection |
| Partners | Trust, integration security |

---

## 2. ISMS Scope

### 2.1 Scope Statement

The EcoMetricx Information Security Management System covers all aspects of information security for the delivery of energy analytics and data science services, including:

- Cloud infrastructure management (AWS)
- Application development and deployment
- Customer data processing and storage
- Internal business operations
- Remote workforce security

### 2.2 In-Scope

#### 2.2.1 Business Processes

| Process | Description |
|---------|-------------|
| Service Delivery | Energy analytics services to customers |
| Application Development | Development, testing, deployment |
| Data Processing | ETL, analytics, machine learning |
| Customer Support | Technical support, incident handling |
| Infrastructure Management | AWS resource management |
| Access Management | User provisioning, authentication |
| Security Operations | Monitoring, incident response |

#### 2.2.2 Information Assets

| Asset Category | Examples | Classification |
|----------------|----------|----------------|
| Customer Data | Energy consumption, account info | Confidential |
| Business Data | Contracts, pricing, strategies | Confidential |
| Employee Data | HR records, credentials | Restricted |
| Source Code | Applications, algorithms | Confidential |
| Infrastructure | AWS resources, configurations | Internal |
| Documentation | Policies, procedures | Internal |

#### 2.2.3 Technology Infrastructure

**Cloud Platform (AWS Account 302146782327):**

| Service | Quantity | Purpose |
|---------|----------|---------|
| EC2 | 11 instances | Compute |
| S3 | 54 buckets | Storage |
| RDS | 4 databases | Data persistence |
| Lambda | Multiple | Serverless compute |
| CloudFormation | Multiple stacks | IaC |
| CloudWatch | - | Monitoring |
| CloudTrail | - | Audit logging |
| IAM | 34 users | Access control |

**Supporting Services:**
- Microsoft 365 (Identity, Email, Collaboration)
- GitHub (Source Control)
- Atlassian Confluence (Documentation)
- Slack (Communication)

#### 2.2.4 Physical Locations

| Location | Type | Scope |
|----------|------|-------|
| AWS us-east-1 | Data Center | Primary production |
| AWS other regions | Data Center | Secondary services |
| Microsoft Cloud | Data Center | Identity, email |
| Employee Home Offices | Workspace | Remote work endpoints |

#### 2.2.5 Personnel

| Role Category | Count | Scope |
|---------------|-------|-------|
| Individual Users | 22 | System access |
| Service Accounts | 8 | Automated access |
| Application Accounts | 4 | System integration |
| Contractors | Varies | Project-based |

### 2.3 Out of Scope

| Item | Reason |
|------|--------|
| Customer-owned systems | Customer responsibility |
| Third-party SaaS internal operations | Covered by their certifications |
| Physical data center security | AWS responsibility (shared model) |
| Customer endpoint devices | Customer responsibility |

### 2.4 Interfaces and Dependencies

| Interface | Direction | Description |
|-----------|-----------|-------------|
| Customer Portals | Inbound | Customer access to services |
| API Integrations | Both | Data exchange with customer systems |
| AWS Services | Both | Cloud infrastructure |
| Microsoft 365 | Outbound | Identity, authentication |
| Third-party APIs | Outbound | Data sources |

---

## 3. Boundaries

### 3.1 Organizational Boundaries

- All EcoMetricx employees and contractors
- All departments and functions
- All subsidiaries (if applicable)

### 3.2 Physical Boundaries

- AWS data centers (via shared responsibility model)
- Employee home offices (endpoint security)
- No physical office locations in scope

### 3.3 Network Boundaries

- AWS VPCs and security groups
- VPN connections for administrative access
- Internet-facing applications
- API gateways

### 3.4 System Boundaries

- All AWS resources in account 302146782327
- Supporting SaaS services (Microsoft 365, GitHub, etc.)
- Development and production environments

---

## 4. Exclusions and Justifications

| Exclusion | Justification |
|-----------|---------------|
| Customer data processing logic | Varies by customer, covered by agreements |
| Third-party vendor internal controls | Covered by vendor certifications (SOC 2) |
| Physical security of data centers | AWS responsibility per shared model |

---

## 5. Regulatory and Legal Requirements

### 5.1 Applicable Regulations

| Regulation | Applicability | Impact |
|------------|---------------|--------|
| SOC 2 | Customer requirement | Trust Service Criteria |
| ISO 27001 | Best practice | ISMS framework |
| GDPR | EU customer data | Data protection |
| CCPA | California residents | Privacy rights |
| HIPAA | If handling PHI | Health data protection |

### 5.2 Contractual Requirements

- Customer contracts with security requirements
- SLAs for availability and incident response
- Data processing agreements

---

## 6. Risk Context

### 6.1 External Context

| Factor | Description |
|--------|-------------|
| Technology | Rapid cloud evolution, emerging threats |
| Regulatory | Increasing data protection requirements |
| Market | Customer demand for security assurance |
| Economic | Cost of breaches, cyber insurance |

### 6.2 Internal Context

| Factor | Description |
|--------|-------------|
| Remote Workforce | Distributed endpoints, home networks |
| Cloud-Native | AWS expertise, shared responsibility |
| Data Processing | Customer data sensitivity |
| Growth | Scaling infrastructure and team |

---

## 7. ISMS Objectives

### 7.1 Security Objectives

| Objective | Metric | Target |
|-----------|--------|--------|
| Confidentiality | Data breaches | 0 per year |
| Integrity | Data corruption incidents | 0 per year |
| Availability | System uptime | 99.9% |

### 7.2 Compliance Objectives

| Objective | Metric | Target |
|-----------|--------|--------|
| SOC 2 Audit | Findings | 0 critical/high |
| Policy Compliance | Exceptions | <5% |
| Training Completion | Personnel trained | 100% |

---

## 8. Documentation References

| Document | Purpose |
|----------|---------|
| Information Security Policy | Overall security policy |
| Access Control Policy | Access management |
| Data Classification Policy | Data handling |
| Risk Register | Risk tracking |
| AWS Security Configuration | Technical controls |

---

## 9. Review and Maintenance

| Activity | Frequency | Owner |
|----------|-----------|-------|
| Scope Review | Annually | InfoSec Team |
| Risk Assessment | Annually | InfoSec Team |
| Context Review | Annually | Management |
| Documentation Update | As needed | InfoSec Team |

---

## 10. Approval

This ISMS Scope Definition has been reviewed and approved.

**InfoSec Team Lead:**

Name: ______________________  
Date: ______________________  
Signature: ______________________

**Executive Sponsor:**

Name: ______________________  
Date: ______________________  
Signature: ______________________

---

**Document Control:** Version 1.0 | December 2025 | Next Review: December 2026

