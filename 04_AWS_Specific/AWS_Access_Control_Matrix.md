# AWS Access Control Matrix

**Document Version:** 1.0  
**Last Updated:** December 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Quarterly (per Access Control Policy)

## Overview

This document maps IAM users and roles to SOC 2 Trust Service Criteria requirements and provides an access control matrix for AWS resources.

## IAM Users Inventory

As of December 2025, EcoMetricx maintains the following IAM users:

### Individual User Accounts

1. **Alex.Ledbetter** - Primary administrator
2. **andrei** (Andrei Ionete) - InfoSec Team Lead (andrei@ecometricx.com)
3. **Benjamin-Westrich** - Team member
4. **catalin** - Team member
5. **conrad** - Team member (AWS Tagging Standards Policy owner)
6. **emilian_varga** - Team member
7. **evan** - Team member
8. **franklin_files** - Team member
9. **Garrett-Rouser** - Team member
10. **jervin** - Team member
11. **kyle_domingo** - Team member
12. **matt.harding** - Team member
13. **michelle_buenaflor** - Team member
14. **mihaim** - Team member
15. **mik_iris_samson** - Team member
16. **nedim.hadzi** - Team member
17. **nedim_test** - Test account
18. **nedim_testing** - Test account
19. **nick** - Team member
20. **rowe_tampus** - Team member
21. **tudor** - Team member
22. **vincent_tucal** - Team member

### Service Accounts

23. **ecr_github_push_community_user** - ECR push access for community projects
24. **ecr_github_push_llm_testing_user** - ECR push access for LLM testing
25. **ecr_github_push_nilm_pecformer_user** - ECR push access for NILM PecFormer
26. **ecr_github_push_ocpa_user** - ECR push access for OCPA
27. **ecr_github_push_shopify_ai_user** - ECR push access for Shopify AI
28. **ecr_github_push_tenore_user** - ECR push access for Tenore
29. **ecr_github_push_user** - General ECR push access
30. **ecr_github_push_xcel_user** - ECR push access for Xcel Energy

### Application/System Accounts

31. **gitlab.cicd** - GitLab CI/CD integration
32. **tenore_dev_s3_user** - S3 access for Tenore development
33. **ses-smtp-user.20250829-094338** - SES SMTP user
34. **ses-smtp-user.20250829-100528** - SES SMTP user

## Access Control Requirements (SOC 2 CC6.2)

### Authentication Requirements

- **MFA Required:** All individual user accounts must have MFA enabled
- **Password Policy:** 
  - Minimum 14 characters
  - Complexity requirements (uppercase, lowercase, numbers, special characters)
  - Rotation every 90 days
  - Cannot reuse last 12 passwords

### Authorization Requirements

- **Least Privilege:** Users granted minimum permissions necessary for job function
- **Role-Based Access:** Access based on job role and responsibilities
- **Regular Reviews:** Quarterly access reviews to ensure appropriate access levels

### Access Review Schedule

| User Type | Review Frequency | Next Review |
|-----------|-----------------|-------------|
| Individual Users | Quarterly | Q1 2026 |
| Service Accounts | Quarterly | Q1 2026 |
| Application Accounts | Quarterly | Q1 2026 |
| Test Accounts | Monthly | January 2026 |

## IAM User Classification

### Administrative Access

Users with administrative privileges (require additional scrutiny):
- **Alex.Ledbetter** - Primary administrator
- Additional administrative users (to be documented per access review)

**Requirements:**
- MFA mandatory
- All actions logged via CloudTrail
- Peer review for structural changes
- Time-limited elevated access sessions when possible

### Standard User Access

Standard team members with read/write access to assigned resources:
- All individual user accounts (except administrators)
- Access limited to resources required for job function

### Service Account Access

Service accounts with programmatic access:
- ECR push users (GitHub integration)
- SES SMTP users
- Application-specific service accounts

**Requirements:**
- Strong access keys (rotated every 90 days)
- Limited scope of permissions
- No console access
- Regular key rotation

### Test Account Access

Test accounts for development and testing:
- **nedim_test**
- **nedim_testing**

**Requirements:**
- Limited to non-production resources
- Regular review and cleanup
- Deactivation when not in use

## Resource Access Mapping

### EC2 Instances

| User/Role | Access Level | Instances | Justification |
|-----------|--------------|-----------|---------------|
| Alex.Ledbetter | Full | All | Primary administrator |
| Team Members | Read/Write | Assigned projects | Job function |
| Service Accounts | None | N/A | No EC2 access required |

### S3 Buckets

| User/Role | Access Level | Buckets | Justification |
|-----------|--------------|---------|---------------|
| Alex.Ledbetter | Full | All | Primary administrator |
| Team Members | Read/Write | Project-specific | Job function |
| tenore_dev_s3_user | Read/Write | tenore-dev-* | Development access |
| Service Accounts | Read/Write | Application-specific | Application requirements |

### RDS Databases

| User/Role | Access Level | Databases | Justification |
|-----------|--------------|-----------|---------------|
| Alex.Ledbetter | Full | All | Primary administrator |
| Team Members | Read-only (default) | Assigned databases | Job function |
| Application Roles | Read/Write | Application databases | Application requirements |

### CloudFormation

| User/Role | Access Level | Stacks | Justification |
|-----------|--------------|--------|---------------|
| Alex.Ledbetter | Full | All | Primary administrator |
| Team Members | Read/Write | Project-specific | Infrastructure management |

## Access Control Procedures

### New User Onboarding

1. Request access through manager approval
2. Create IAM user account
3. Assign appropriate IAM policies
4. Enable MFA
5. Provide access credentials securely
6. Document in this matrix
7. Schedule quarterly access review

### Access Modification

1. Manager approval required
2. Update IAM policies
3. Document changes in this matrix
4. Notify user of changes
5. Verify access works as intended

### User Offboarding

1. Disable IAM user account immediately upon termination
2. Revoke all access keys
3. Remove from all groups and policies
4. Document removal in this matrix
5. Review and revoke any shared resources

### Quarterly Access Review

1. Review all IAM users
2. Verify MFA status
3. Check for unused accounts
4. Verify access levels are appropriate
5. Update this matrix
6. Document findings and actions taken

## Compliance Mapping

### SOC 2 CC6.2 (Access Control)

- **Requirement:** Logical and physical access controls restrict access to information assets
- **Implementation:**
  - IAM policies enforce access controls
  - MFA required for all users
  - Least privilege principle applied
  - Regular access reviews conducted

### SOC 2 CC6.3 (Access Credentials)

- **Requirement:** System credentials are assigned, authenticated, and monitored
- **Implementation:**
  - IAM user accounts for all access
  - MFA for authentication
  - Access keys rotated regularly
  - CloudTrail logs all access

## Security Best Practices

1. **Principle of Least Privilege:** Users granted minimum permissions necessary
2. **Separation of Duties:** Administrative and operational access separated
3. **Regular Reviews:** Quarterly access reviews to ensure appropriate access
4. **MFA Enforcement:** MFA required for all individual user accounts
5. **Key Rotation:** Access keys rotated every 90 days
6. **Audit Logging:** All access logged via CloudTrail

## Related Documents

- `../01_Policies/Access_Control_Policy.docx` - Access control policy
- `AWS_Security_Configuration_Documentation.md` - Overall AWS security configuration
- `../07_Confluence_References/Confluence_Page_Mapping.md` - Confluence references

## Review History

| Date | Reviewer | Changes | Next Review |
|------|----------|---------|-------------|
| December 2025 | InfoSec Team | Initial documentation | Q1 2026 |

