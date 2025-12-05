# Risk Register

**Version:** 1.0  
**Last Updated:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Quarterly

---

## Risk Assessment Methodology

### Likelihood Scale
| Level | Rating | Description |
|-------|--------|-------------|
| 1 | Rare | Less than 1% chance |
| 2 | Unlikely | 1-10% chance |
| 3 | Possible | 10-50% chance |
| 4 | Likely | 50-90% chance |
| 5 | Almost Certain | Greater than 90% chance |

### Impact Scale
| Level | Rating | Description |
|-------|--------|-------------|
| 1 | Negligible | Minor inconvenience, no data loss |
| 2 | Minor | Some disruption, minimal data exposure |
| 3 | Moderate | Significant disruption, limited data exposure |
| 4 | Major | Major disruption, significant data exposure |
| 5 | Severe | Business-critical failure, widespread data breach |

### Risk Score
**Risk Score = Likelihood × Impact**

| Score | Risk Level | Action Required |
|-------|------------|-----------------|
| 1-4 | Low | Accept or monitor |
| 5-9 | Medium | Mitigate within 90 days |
| 10-15 | High | Mitigate within 30 days |
| 16-25 | Critical | Immediate action required |

---

## Active Risks

### RISK-001: Microsoft 365 Authentication Failure

**Risk ID:** RISK-001  
**Category:** Availability  
**Source:** Domain Services BIA (October 2025)

| Attribute | Value |
|-----------|-------|
| Description | Microsoft 365 serves as single identity provider; failure causes complete authentication lockout |
| Likelihood | 2 (Unlikely) |
| Impact | 5 (Severe) |
| **Risk Score** | **10 (High)** |
| Owner | IT Infrastructure |
| Status | Open |

**Current Controls:**
- Microsoft 365 SLA (99.9% uptime)
- Workaround procedures documented
- Emergency contact list

**Gaps Identified:**
- Google Workspace workaround never tested
- Personal device communication channels not tested

**Treatment Plan:**
- [ ] Test Google Workspace workaround (Q1 2026)
- [ ] Create and distribute emergency contact list
- [ ] Consider backup identity provider

---

### RISK-002: IAM Password Policy Not Configured

**Risk ID:** RISK-002  
**Category:** Security (Access Control)  
**Source:** AWS CLI Findings (December 2025)

| Attribute | Value |
|-----------|-------|
| Description | AWS account has no password policy, allowing weak passwords |
| Likelihood | 3 (Possible) |
| Impact | 4 (Major) |
| **Risk Score** | **12 (High)** |
| Owner | InfoSec Team |
| Status | Open - Pending Admin Action |

**Current Controls:**
- MFA required (documented policy)
- Access reviews quarterly

**Gaps Identified:**
- No technical enforcement of password complexity
- No password rotation requirement

**Treatment Plan:**
- [ ] Configure IAM password policy (requires admin - see AWS_Admin_Requests.md)
- [ ] Verify implementation

---

### RISK-003: Security Groups with Open Access

**Risk ID:** RISK-003  
**Category:** Security (Network)  
**Source:** EC2 Security Group Audit (December 2025)

| Attribute | Value |
|-----------|-------|
| Description | 20 security groups allow inbound from 0.0.0.0/0 |
| Likelihood | 3 (Possible) |
| Impact | 3 (Moderate) |
| **Risk Score** | **9 (Medium)** |
| Owner | IT Infrastructure |
| Status | Open |

**Current Controls:**
- Security groups in use
- CloudTrail logging of changes

**Gaps Identified:**
- Launch-wizard groups likely over-permissive
- Database security groups may be unnecessarily exposed

**Treatment Plan:**
- [ ] Review and remediate launch-wizard groups
- [ ] Restrict database security groups
- [ ] Document legitimate public access needs

---

### RISK-004: RDS Multi-AZ Not Enabled

**Risk ID:** RISK-004  
**Category:** Availability  
**Source:** RDS Security Audit (December 2025)

| Attribute | Value |
|-----------|-------|
| Description | All 4 RDS databases lack Multi-AZ, risking availability during AZ outages |
| Likelihood | 2 (Unlikely) |
| Impact | 4 (Major) |
| **Risk Score** | **8 (Medium)** |
| Owner | IT Infrastructure |
| Status | Open - Pending Admin Action |

**Current Controls:**
- Automated backups enabled
- Encryption at rest enabled

**Gaps Identified:**
- Single point of failure per database
- Manual failover required during outage

**Treatment Plan:**
- [ ] Enable Multi-AZ for production databases (see AWS_Admin_Requests.md)
- [ ] Prioritize: database-neurova, ecmx-tenore

---

### RISK-005: CloudWatch Disk Space Alarm Active

**Risk ID:** RISK-005  
**Category:** Availability  
**Source:** AWS CLI Findings (December 2025)

| Attribute | Value |
|-----------|-------|
| Description | Low disk space alarm active for r6a.4xlarge instance |
| Likelihood | 5 (Almost Certain) |
| Impact | 2 (Minor) |
| **Risk Score** | **10 (High)** |
| Owner | IT Infrastructure |
| Status | Open - Investigation Needed |

**Current Controls:**
- CloudWatch monitoring
- Alarm notification configured

**Treatment Plan:**
- [ ] Investigate disk usage on i-0e5e87039a350664d
- [ ] Clean up or expand disk
- [ ] Resolve alarm

---

### RISK-006: Backup Restoration Not Tested

**Risk ID:** RISK-006  
**Category:** Availability  
**Source:** PROJECT_STATUS_REPORT

| Attribute | Value |
|-----------|-------|
| Description | RDS and S3 backup restoration procedures not tested |
| Likelihood | 3 (Possible) |
| Impact | 4 (Major) |
| **Risk Score** | **12 (High)** |
| Owner | IT Infrastructure |
| Status | Open |

**Current Controls:**
- Automated RDS backups (1-7 days)
- S3 versioning

**Gaps Identified:**
- No documented test results
- Restoration procedures not validated

**Treatment Plan:**
- [ ] Schedule quarterly backup restoration tests
- [ ] Document test results
- [ ] Update AWS_Backup_Recovery_Procedures.md

---

### RISK-007: MFA Status Unverified

**Risk ID:** RISK-007  
**Category:** Security (Access Control)  
**Source:** AWS CLI Findings (December 2025)

| Attribute | Value |
|-----------|-------|
| Description | Cannot verify MFA status for IAM users due to permission restrictions |
| Likelihood | 2 (Unlikely) |
| Impact | 4 (Major) |
| **Risk Score** | **8 (Medium)** |
| Owner | InfoSec Team |
| Status | Open - Pending Admin Action |

**Current Controls:**
- MFA required per policy
- Microsoft 365 MFA enforced

**Treatment Plan:**
- [ ] Request iam:ListVirtualMFADevices permission (see AWS_Admin_Requests.md)
- [ ] Verify and document MFA status for all users
- [ ] Update AWS_Access_Control_Matrix.md

---

## Risk Summary

| Risk Level | Count | Risks |
|------------|-------|-------|
| Critical | 0 | - |
| High | 4 | RISK-001, RISK-002, RISK-005, RISK-006 |
| Medium | 3 | RISK-003, RISK-004, RISK-007 |
| Low | 0 | - |

---

## Risk Treatment Summary

| Treatment | Risks |
|-----------|-------|
| Pending Admin Action | RISK-002, RISK-004, RISK-007 |
| Investigation Needed | RISK-005 |
| Testing Required | RISK-001, RISK-006 |
| Remediation Required | RISK-003 |

---

## Review History

| Date | Reviewer | Changes |
|------|----------|---------|
| Dec 5, 2025 | InfoSec Team | Initial risk register creation |

---

**Next Review:** Q1 2026

**Document Control:** Version 1.0 | December 2025

