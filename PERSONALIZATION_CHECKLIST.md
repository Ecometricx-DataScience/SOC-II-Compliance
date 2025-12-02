# Personalization Checklist for SOC 2 Documents

**Created:** December 2025  
**Purpose:** Identify items that need to be personalized based on EcoMetricx-specific information not captured from AWS, Confluence, or historical documents.

## Key Information Captured from Existing Sources

### From Existing Documents:
- **InfoSec Team Lead:** Andrei Ionete
- **Contact Email:** security@ecometricx.com
- **Company Name:** EcoMetricx (EcoMetricx LLC)
- **Policy Owner Pattern:** "EcoMetricx InfoSec Team"

### From AWS:
- **AWS Account ID:** 302146782327
- **Primary Region:** us-east-1
- **IAM Users:** 30+ users (names captured)
- **Infrastructure:** EC2, S3, RDS, CloudFormation, etc.

### From Confluence:
- **AWS Tagging Policy Owner:** Conrad Jahn
- **Data Classification Framework:** Public, Internal, Confidential, Restricted (from DSDP Section 8)
- **Compliance Requirements:** GDPR, HIPAA, PCI-DSS, SOX, CCPA (from DSDP Section 12)

## Items Requiring Personalization

### 1. Approval and Signatures

**Location:** All policy documents (footer section)
- [ ] **Approved By:** [To be completed] → Replace with actual approver name/title
- **Suggested:** CEO, CISO, or designated executive
- **Action:** Add signature line or approval block

**Files Affected:**
- All 7 new policy files
- Business Continuity Plan
- Training Plan
- SOP for Documentation Review

### 2. Organizational Structure and Roles

**Information Security Policy:**
- [ ] **Chief Information Security Officer (CISO) / InfoSec Team Lead:** Currently says "InfoSec Team Lead" - confirm if there's a CISO or if Andrei Ionete is the lead
- [ ] **Management Structure:** Define reporting structure for InfoSec team
- [ ] **Data Owners:** Identify specific data owners by data type/system
- [ ] **Change Advisory Board (CAB) Members:** Define who serves on CAB

**Access Control Policy:**
- [ ] **Administrative Users:** Currently lists "Alex.Ledbetter" as primary administrator - confirm all administrative users
- [ ] **Access Approval Authority:** Define who approves access at different levels
- [ ] **Data Owners:** List specific data owners by system/data type

**Change Management Policy:**
- [ ] **Change Manager:** Identify who serves as Change Manager
- [ ] **CAB Members:** List specific CAB members and their roles
- [ ] **Change Approval Authority:** Define approval levels and authorities

**Incident Response Policy:**
- [ ] **Incident Response Lead:** Identify who serves as IR lead
- [ ] **IR Team Members:** List specific team members and their roles
- [ ] **Legal/Compliance Contact:** Identify legal counsel and compliance officer
- [ ] **Communication Lead:** Identify PR/communication contact

**Vendor Management Policy:**
- [ ] **Procurement Team:** Identify who manages vendor relationships
- [ ] **Vendor Assessment Team:** Define who conducts security assessments

### 3. Contact Information

**All Documents:**
- [ ] **Physical Address:** Add company physical address (if required for policies)
- [ ] **Phone Numbers:** Add main phone number and security hotline (if applicable)
- [ ] **Additional Contacts:** 
  - Legal department contact
  - HR department contact
  - Compliance officer contact
  - IT help desk contact

**Incident Response Policy:**
- [ ] **24/7 Security Contact:** Define after-hours security contact method
- [ ] **Escalation Contacts:** Define escalation chain

**Business Continuity Plan:**
- [ ] **Emergency Contacts:** List all emergency contact numbers
- [ ] **Alternate Site Information:** If applicable, add alternate site details
- [ ] **Crisis Management Team:** List team members and contact info

### 4. Business-Specific Information

**Business Continuity Plan:**
- [ ] **Critical Business Processes:** Review and customize Tier 1/2/3 classifications
- [ ] **RTO/RPO Values:** Confirm if the stated RTO/RPO values (4 hours, 24 hours, 72 hours) are accurate
- [ ] **Critical Systems List:** Create specific list of critical systems/applications
- [ ] **Alternate Work Locations:** Define specific alternate sites or remote work capabilities
- [ ] **Key Vendors:** List critical vendors that support business operations

**Training Plan:**
- [ ] **Training Delivery Methods:** Confirm which methods are actually used (LMS system name, etc.)
- [ ] **Training Schedule:** Customize quarterly schedule based on actual training calendar
- [ ] **Training Budget:** Add actual training budget allocation
- [ ] **LMS System:** If using an LMS, specify the system name

**Data Classification Policy:**
- [ ] **Data Examples:** Review examples to ensure they match EcoMetricx data types
- [ ] **Industry-Specific Data:** Add any industry-specific data types (energy data, utility data, etc.)
- [ ] **Customer Data Types:** Define specific types of customer data handled

### 5. Technical Details

**Access Control Policy:**
- [ ] **Authentication Systems:** Specify actual systems used (Active Directory, Okta, etc.)
- [ ] **SSO Provider:** If using SSO, specify provider name
- [ ] **MFA Methods:** Specify which MFA methods are approved/used
- [ ] **Password Management System:** Specify if using a password manager

**Logging and Monitoring Policy:**
- [ ] **SIEM System:** Specify which SIEM is used (if any)
- [ ] **Monitoring Tools:** List specific monitoring tools in use
- [ ] **Log Aggregation System:** Specify log aggregation platform
- [ ] **Alerting System:** Specify alerting/notification system (PagerDuty, etc.)

**Change Management Policy:**
- [ ] **Change Management Tool:** Specify tool used (ServiceNow, Jira, etc.)
- [ ] **Version Control System:** Specify Git provider and repositories
- [ ] **CI/CD Tools:** List CI/CD tools in use

### 6. Compliance-Specific Information

**All Policies:**
- [ ] **Regulatory Applicability:** Confirm which regulations actually apply:
  - GDPR: Does EcoMetricx process EU data?
  - HIPAA: Does EcoMetricx handle PHI?
  - PCI-DSS: Does EcoMetricx process payment cards?
  - CCPA: Does EcoMetricx have California customers?
  - SOX: Is EcoMetricx publicly traded or subject to SOX?

**Vendor Management Policy:**
- [ ] **Critical Vendors List:** Create list of current critical vendors
- [ ] **Vendor Assessment Criteria:** Customize assessment criteria based on actual process
- [ ] **Contract Template:** Reference actual vendor contract template

### 7. Procedures and Processes

**Incident Response Policy:**
- [ ] **Incident Response Playbooks:** Reference specific playbooks if they exist
- [ ] **Forensics Capabilities:** Define if internal forensics or external vendor
- [ ] **Communication Templates:** Reference actual communication templates

**Change Management Policy:**
- [ ] **Change Windows:** Define actual maintenance windows
- [ ] **Change Types:** Customize change classification based on actual process
- [ ] **Approval Workflow:** Document actual approval workflow

**Backup and Recovery Procedures (AWS):**
- [ ] **Backup Schedules:** Verify actual backup schedules match documented
- [ ] **Recovery Testing:** Document actual recovery testing schedule and results
- [ ] **Backup Tools:** Specify actual backup tools used

### 8. Metrics and Reporting

**All Policies:**
- [ ] **Reporting Schedule:** Confirm actual reporting frequencies
- [ ] **Metrics Dashboard:** Reference actual metrics dashboards if they exist
- [ ] **Management Reporting:** Define who receives reports and how often

**Training Plan:**
- [ ] **Training Metrics:** Define specific metrics tracked
- [ ] **Completion Targets:** Set actual completion rate targets
- [ ] **Assessment Passing Scores:** Define passing scores for assessments

### 9. Document-Specific Customizations

**Information Security Policy:**
- [ ] **Security Program Maturity:** Describe current security program maturity level
- [ ] **Security Framework:** Specify if following a specific framework (NIST, ISO 27001, etc.)
- [ ] **Risk Management Process:** Detail actual risk management methodology

**Business Continuity Plan:**
- [ ] **BCP Team Members:** List actual BCP team members
- [ ] **Alternate Site Details:** If applicable, provide alternate site information
- [ ] **Recovery Procedures:** Detail specific recovery procedures for critical systems

**Training Plan:**
- [ ] **Training Topics:** Review and customize based on actual training curriculum
- [ ] **Role-Specific Training:** Define actual role-specific training requirements
- [ ] **Training Vendors:** List external training vendors if used

### 10. Cross-References

**All Documents:**
- [ ] **Related Policies:** Verify all cross-references to other policies are correct
- [ ] **Document Links:** Ensure all internal document links work
- [ ] **Confluence Links:** Verify Confluence page links are accessible

## Quick Reference: Key Contacts to Add

- [x] **CISO/InfoSec Lead:** Andrei Ionete (andrei@ecometricx.com) ✓
- [ ] **Security Email:** security@ecometricx.com ✓ (already captured)
- [ ] **Legal Contact:** [To be added]
- [ ] **Compliance Officer:** [To be added]
- [ ] **HR Contact:** [To be added]
- [ ] **IT Help Desk:** [To be added]
- [ ] **Change Manager:** [To be added]
- [ ] **Incident Response Lead:** [To be added]

## Quick Reference: Systems to Specify

- [ ] **SSO Provider:** [e.g., Okta, Azure AD, etc.]
- [ ] **SIEM System:** [e.g., Splunk, QRadar, etc.]
- [ ] **LMS System:** [e.g., Workday, Cornerstone, etc.]
- [ ] **Change Management Tool:** [e.g., ServiceNow, Jira, etc.]
- [ ] **Password Manager:** [e.g., 1Password, LastPass, etc.]
- [ ] **Monitoring Tools:** [List actual tools]

## Priority Items (Must Complete Before Audit)

1. **Approval Signatures** - All documents need actual approval
2. **Contact Information** - Legal, compliance, and emergency contacts
3. **Organizational Roles** - Define actual team members and responsibilities
4. **Regulatory Applicability** - Confirm which regulations actually apply
5. **Critical Systems List** - Document actual critical systems
6. **Incident Response Team** - Define actual IR team members

## Nice-to-Have Items (Can Complete Over Time)

1. Detailed metrics and dashboards
2. Specific tool names and versions
3. Detailed procedures and playbooks
4. Vendor-specific information
5. Training curriculum details

---

**Note:** This checklist should be reviewed and updated as documents are personalized. Mark items as complete as they are addressed.

