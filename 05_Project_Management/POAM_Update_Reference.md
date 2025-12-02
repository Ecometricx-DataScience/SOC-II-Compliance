# POAM Update Reference Guide

## Document Location Changes

### Policies (Previously: OneDrive_3_12-2-2025)
All policies now located in: **01_Policies/**
- Acceptable_Use_Policy_SOC2_v1.0_Oct'25.docx
- Acceptable_Encryption_Standard_Policy_April2025.docx
- Privacy_Management_Policy_April2025.docx
- Risk_Communication_Management_Policy_April2025.docx

### NEW Policies Created (Location: 01_Policies/)
- Access_Control_Policy.md (to be converted to .docx)
- Data_Classification_Handling_Policy.md (to be converted to .docx)
- Information_Security_Policy.md (to be converted to .docx)
- Incident_Response_Policy.md (to be converted to .docx)
- Vendor_Management_Policy.md (to be converted to .docx)
- Change_Management_Policy.md (to be converted to .docx)
- Logging_Monitoring_Policy.md (to be converted to .docx)

### Plans (Previously: OneDrive_2_12-2-2025)
All plans now located in: **03_Plans/**
- EcoMetricx-Backup-Recovery-Template.doc
- EcoMetricx-BIA-Template.doc
- EcoMetricx-Disaster-Preparedness-Plan-Template.doc
- EcoMetricx-Disaster-Recovery-Plan-Template.docx

### NEW Plans Created (Location: 03_Plans/)
- Business_Continuity_Plan.md (to be converted to .docx)
- Training_Plan.md (to be converted to .docx)

### Procedures (NEW)
Location: **02_Procedures/**
- Standard_Operating_Procedure_Documentation_Review.md (to be converted to .docx)

### AWS-Specific Documentation (NEW)
Location: **04_AWS_Specific/**
- AWS_Security_Configuration_Documentation.md
- AWS_Access_Control_Matrix.md
- AWS_Backup_Recovery_Procedures.md
- AWS_Incident_Response_Procedures.md
- AWS_Tagging_Standards_Reference.md (from Confluence)

### Project Management (Previously: OneDrive_1_12-2-2025)
Location: **05_Project_Management/**
- SOCII_ISO27001_POAM_22_Nov_2025.xlsx
- SOCII_ISO27001_POAM_Start.xlsx

### Reference Materials (Previously: OneDrive_1_12-2-2025 and OneDrive_2_12-2-2025)
Location: **06_Reference_Materials/**
- Cybersecurity_Framework_v2.0_Concept_Crosswalk_Template_v1.0.xlsx
- 800-53-rev3-controls.csv
- NIST_SP-800-53_rev5_catalog_load.csv

### Confluence References (NEW)
Location: **07_Confluence_References/**
- Confluence_Page_Mapping.md

## Confluence Integration

The following Confluence content has been extracted and integrated:

1. **AWS Tagging Standards Policy (v1.1)** - Extracted to AWS_Tagging_Standards_Reference.md
   - Source: https://ecometricx.atlassian.net/wiki/spaces/ATS/pages/579993604
   - Last Modified: November 12, 2025 by Conrad Jahn

2. **Data Solution Design Policy - Section 8 (Security and Access Control)**
   - Used to inform Data_Classification_Handling_Policy and Access_Control_Policy
   - Source: https://ecometricx.atlassian.net/wiki/spaces/DSDP/pages/574291970

3. **Data Solution Design Policy - Section 12 (Compliance and Governance)**
   - Used to inform compliance sections in various policies
   - Source: https://ecometricx.atlassian.net/wiki/spaces/DSDP/pages/574914571

## POAM Update Recommendations

### Completed Tasks (Update Status)
- [x] Build/Configure central repository for all policies and procedures → **COMPLETED** (New directory structure created)
- [x] Build initial list of all documentation and templates needed → **COMPLETED** (All missing documents created)
- [x] Write Standard Operating Procedure for every 6 month documentation review → **COMPLETED** (See 02_Procedures/)
- [x] Policies for Security Defined → **COMPLETED** (Information_Security_Policy created)
- [x] Policies for Availability Defined → **COMPLETED** (Business_Continuity_Plan created)
- [x] Policies for Privacy Defined → **COMPLETED** (Privacy_Management_Policy exists, Data_Classification_Handling_Policy created)
- [x] Policies for Confidentiality Defined → **COMPLETED** (Data_Classification_Handling_Policy created)
- [x] Policies for Processing Integrity Defined → **COMPLETED** (Change_Management_Policy, Logging_Monitoring_Policy created)
- [x] Training Plan → **COMPLETED** (See 03_Plans/Training_Plan.md)

### In Progress / Pending
- [ ] Engage third party auditor → **PENDING**
- [ ] ISO27001 Initiation and Planning → **PENDING**
- [ ] ISO27001 Context and Risk Assessment → **PENDING**
- [ ] ISO27001 Engage 3rd Party Auditor → **PENDING**

## Next Steps for POAM Update

1. Update document paths in POAM to reflect new directory structure
2. Mark completed tasks as complete with new document locations
3. Add Confluence references where applicable
4. Update remediation plan steps with new document locations
5. Add notes about Confluence integration
