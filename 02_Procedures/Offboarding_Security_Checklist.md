# Offboarding Security Checklist

**Version:** 1.0  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Based on:** Access Control Policy, AWS Access Control Matrix

---

## Employee/Contractor Offboarding

**Employee Name:** ______________________  
**Department:** ______________________  
**Manager:** ______________________  
**Last Day:** ______________________  
**Offboarding Date:** ______________________  
**HR Contact:** ______________________  
**IT Contact:** ______________________

---

## Pre-Departure (Before Last Day)

### Knowledge Transfer
- [ ] Critical knowledge documented
- [ ] Ongoing projects transitioned
- [ ] Passwords/credentials for shared accounts documented securely
- [ ] Key contacts and responsibilities transferred

### Manager Tasks
- [ ] Exit interview scheduled
- [ ] Final work assignments completed
- [ ] Acknowledgment of confidentiality obligations obtained

---

## Day of Departure

### Identity and Access (IT Team)

#### Microsoft 365 / Entra ID
- [ ] Disable Microsoft 365 account
- [ ] Revoke MFA tokens
- [ ] Remove from all Microsoft 365 groups
- [ ] Disable mailbox (or convert to shared if needed)
- [ ] Revoke Microsoft Authenticator registration
- [ ] Remove from Teams channels

#### AWS Access (Per AWS Access Control Matrix)
- [ ] Disable IAM user account
- [ ] Revoke all access keys
- [ ] Remove from all IAM groups
- [ ] Remove IAM policies
- [ ] Update AWS Access Control Matrix
- [ ] Verify no console access possible

**IAM Users to Disable:** ______________________

#### Third-Party Applications
- [ ] Revoke Slack access
- [ ] Revoke Atlassian (Confluence) access
- [ ] Revoke GitHub access
- [ ] Remove from GitHub organization and repositories
- [ ] Revoke any other SaaS application access

**Applications revoked (list):** ______________________

#### VPN and Network Access
- [ ] Revoke VPN access
- [ ] Remove from network access lists
- [ ] Revoke any remote access tools

### Equipment Return

- [ ] Laptop returned
- [ ] Serial number: ______________________
- [ ] Mobile phone returned (if company-issued)
- [ ] Serial number: ______________________
- [ ] Access badges/keys returned
- [ ] Any other company equipment returned

**Equipment condition notes:** ______________________

### Data and Files

- [ ] Company data removed from personal devices (if BYOD)
- [ ] Employee acknowledgment of data removal obtained
- [ ] Company files backed up or transferred
- [ ] Personal files returned to employee
- [ ] Email auto-reply or forwarding configured (if needed)

---

## Post-Departure (Within 24 Hours)

### Access Verification

- [ ] Attempt login to all systems to verify access revoked
- [ ] Verify no active sessions remain
- [ ] Check CloudTrail for any post-termination access attempts
- [ ] Verify shared accounts passwords changed (if any)

### Documentation

- [ ] Access revocation documented in this checklist
- [ ] AWS Access Control Matrix updated
- [ ] Vendor Inventory updated (if applicable)
- [ ] Offboarding ticket closed
- [ ] This checklist filed in HR records

### Security Review

- [ ] Any anomalous activity during notice period?
- [ ] Any security concerns noted?
- [ ] If yes, escalate to InfoSec Team

---

## Contractor-Specific Items

- [ ] Contract termination confirmed
- [ ] NDA obligations reminded
- [ ] Equipment returned (if provided)
- [ ] Third-party system access revoked
- [ ] Vendor record updated

---

## Sign-Off

### IT/Security Confirmation

All access has been revoked and equipment collected.

**IT Representative:**
- Name: ______________________
- Date: ______________________
- Signature: ______________________

### Manager Confirmation

Knowledge transfer completed and no outstanding issues.

**Manager:**
- Name: ______________________
- Date: ______________________
- Signature: ______________________

### HR Confirmation

Offboarding process completed per policy.

**HR Representative:**
- Name: ______________________
- Date: ______________________
- Signature: ______________________

---

## Emergency Contact

For security concerns post-offboarding:
- **Email:** security@ecometricx.com
- **InfoSec Lead:** Andrei Ionete (andrei@ecometricx.com)

---

**Document Control:** Version 1.0 | December 2025

