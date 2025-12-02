# Access Control Policy

**Version:** 1.0  
**Effective Date:** December 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Annually or following major organizational or regulatory change

## 1. Purpose

This policy establishes requirements for controlling access to EcoMetricx information systems and data to protect against unauthorized access, ensure data confidentiality and integrity, and comply with SOC 2 Trust Service Criteria CC6.2 (Access Control). This policy aligns with the Data Solution Design Policy Section 8: Security and Access Control.

## 2. Scope

This policy applies to:
- All employees, contractors, vendors, and third parties
- All information systems, applications, databases, and cloud services
- All network resources and infrastructure
- Physical and logical access controls

## 3. Access Control Principles

### 3.1 Least Privilege

- Users granted minimum access necessary for job function
- Access reviewed and adjusted based on role changes
- Unnecessary elevated privileges prohibited
- Regular access reviews (quarterly minimum)

### 3.2 Need-to-Know

- Access granted only when necessary for business purposes
- Access limited to specific data and systems required
- Access revoked when no longer needed
- Justification required for access requests

### 3.3 Separation of Duties

- Administrative and operational access separated
- No single individual has complete control over critical processes
- Dual approval required for sensitive operations
- Audit trails for all administrative actions

## 4. Authentication Requirements

### 4.1 User Authentication

**Individual Users:**
- Unique user accounts (no shared accounts)
- Strong passwords (minimum 14 characters, complexity requirements)
- Multi-factor authentication (MFA) required for:
  - Administrative access
  - Remote access
  - Production system access
  - Access to Confidential or Restricted data
- Password rotation every 90 days
- Cannot reuse last 12 passwords

**Service Accounts:**
- Strong passwords (minimum 20 characters)
- Regular rotation (every 90 days)
- No interactive login capability
- Limited scope of permissions
- Regular review and deactivation of unused accounts

**Third-Party Access:**
- Vendor-specific accounts with limited scope
- MFA required
- Time-limited access when possible
- Regular review and revocation

### 4.2 Authentication Methods

- Corporate Active Directory/LDAP integration preferred
- Single Sign-On (SSO) where available
- MFA using approved methods (TOTP, SMS, hardware tokens)
- Certificate-based authentication for service accounts

### 4.3 Session Management

- Automatic session timeout (15 minutes inactivity for sensitive systems)
- Concurrent session limits
- Session monitoring and logging
- Secure session termination

## 5. Authorization Requirements

### 5.1 Role-Based Access Control (RBAC)

**Standard Roles:**
- **Reader:** Read-only access to assigned resources
- **Writer:** Read and write access to assigned resources
- **Administrator:** Full access to assigned systems (requires additional approval)
- **Auditor:** Read-only access for audit purposes

**Application-Specific Roles:**
- Defined per application requirements
- Minimum necessary permissions
- Regular review and adjustment

### 5.2 Access Request Process

1. Employee/manager submits access request
2. Business justification required
3. Data owner/manager approval
4. IT/Security team implements access
5. Access granted and documented
6. User acknowledges access and responsibilities

### 5.3 Access Modification

- Changes require manager approval
- Access reviews identify necessary changes
- Immediate revocation upon role change or termination
- Documentation of all access changes

### 5.4 Access Revocation

**Immediate Revocation:**
- Employee termination
- Contractor/vendor contract termination
- Security incident
- Policy violation

**Scheduled Revocation:**
- Role changes
- Project completion
- Access no longer needed
- Quarterly access review findings

## 6. System Access Controls

### 6.1 Network Access

- Firewall rules restrict network access
- VPN required for remote access
- Network segmentation for sensitive systems
- IP whitelisting for administrative access
- Intrusion detection and prevention

### 6.2 Application Access

- Application-level authentication required
- Role-based permissions within applications
- Session management and timeout
- Audit logging of access and actions
- Secure API access with authentication

### 6.3 Database Access

- Database authentication required
- Role-based database permissions
- Read-only access by default
- Write access requires change ticket
- MFA for production database access
- Connection encryption (TLS 1.2+)
- Audit logging of all database access

### 6.4 Cloud Service Access

- IAM policies enforce access controls
- MFA required for all cloud console access
- Least privilege IAM policies
- Regular IAM access reviews
- See AWS_Access_Control_Matrix.md for AWS-specific controls

## 7. Physical Access Controls

### 7.1 Office Access

- Badge access required
- Visitor registration and escort
- Secure areas with additional controls
- Access logs maintained

### 7.2 Data Center/Server Access

- Restricted access to authorized personnel only
- Badge and biometric authentication
- Access logs and monitoring
- Visitor escort required

### 7.3 Equipment Access

- Laptops and mobile devices encrypted
- Screen locks enabled
- Secure storage when unattended
- Remote wipe capability

## 8. Privileged Access Management

### 8.1 Administrative Access

**Requirements:**
- Named accounts only (no shared administrative accounts)
- MFA mandatory
- All actions logged and audited
- Peer review for structural changes
- Time-limited elevated access when possible
- Break-glass account for emergencies (heavily audited)

**Approval:**
- Senior management approval required
- Business justification required
- Regular review of administrative access

### 8.2 Emergency Access

- Break-glass procedures documented
- Emergency access heavily audited
- Immediate review after use
- Post-incident analysis required

## 9. Access Reviews

### 9.1 Review Schedule

- **Individual Users:** Quarterly
- **Service Accounts:** Quarterly
- **Administrative Access:** Quarterly
- **Third-Party Access:** Quarterly
- **Privileged Access:** Monthly

### 9.2 Review Process

1. Generate access reports
2. Data owners review access lists
3. Identify unnecessary access
4. Revoke access no longer needed
5. Document review findings
6. Update access control matrix

### 9.3 Review Documentation

- Access review reports
- Actions taken
- Justification for continued access
- Management approval

## 10. Access Monitoring and Auditing

### 10.1 Logging Requirements

- All authentication attempts logged
- All access granted/revoked logged
- All privileged actions logged
- Failed access attempts logged
- Unusual access patterns logged

### 10.2 Monitoring

- Real-time monitoring of access attempts
- Automated alerts for suspicious activity
- Regular review of access logs
- Investigation of anomalies

### 10.3 Audit Trail

- Immutable audit logs
- Log retention per regulatory requirements (minimum 1 year, 7 years for compliance)
- Regular audit log reviews
- Protection of audit logs from tampering

## 11. Third-Party and Vendor Access

### 11.1 Vendor Access Requirements

- Vendor-specific accounts with limited scope
- MFA required
- Time-limited access when possible
- Regular review and revocation
- Vendor access agreement required

### 11.2 Contractor Access

- Contractor-specific accounts
- Access limited to project requirements
- Immediate revocation upon project completion
- Regular review during engagement

## 12. Compliance

### 12.1 SOC 2 CC6.2 (Access Control)

- Logical and physical access controls restrict access
- System credentials assigned, authenticated, and monitored
- Access removed or disabled in timely manner

### 12.2 Regulatory Requirements

- **GDPR:** Access controls for personal data
- **HIPAA:** Access controls for PHI
- **PCI-DSS:** Access controls for cardholder data
- **SOX:** Access controls for financial systems

## 13. Responsibilities

### 13.1 Information Security Team

- Maintain access control systems
- Implement access controls
- Monitor access and investigate anomalies
- Conduct access reviews
- Provide training

### 13.2 Data Owners

- Approve access requests
- Review access regularly
- Revoke unnecessary access
- Classify data appropriately

### 13.3 Managers

- Approve access requests for team members
- Ensure access is appropriate for role
- Notify IT/Security of role changes
- Ensure compliance with policy

### 13.4 Users

- Use access only for authorized purposes
- Protect credentials
- Report suspicious activity
- Complete required training

## 14. Training and Awareness

- Access control training upon hire
- Annual refresher training
- Role-specific training for administrators
- Awareness of policy updates

## 15. Policy Violations

Non-compliance may result in:
- Mandatory refresher training
- Written warnings
- Suspension of access privileges
- Termination of employment
- Legal consequences for illegal activities

## 16. Related Policies

- Acceptable Use Policy
- Data Classification and Handling Policy
- Information Security Policy
- Logging and Monitoring Policy
- Incident Response Policy
- Vendor Management Policy

## 17. Review Cycle

This policy is reviewed annually or following:
- Major organizational changes
- Regulatory changes
- Security incidents
- Technology changes

---

**Document Control:**
- **Version:** 1.0
- **Last Updated:** December 2025
- **Next Review:** December 2026
- **Approved By:** [To be completed]
- **Distribution:** All employees and contractors

