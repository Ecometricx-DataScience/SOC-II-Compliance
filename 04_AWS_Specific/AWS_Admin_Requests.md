# AWS Admin Requests for SOC 2 Compliance

**Requested By:** Alex Ledbetter  
**Date:** December 5, 2025  
**Priority:** High - Required for SOC 2 Type II

## Request 1: IAM Password Policy Configuration

**Status:** Pending  
**Priority:** Critical

**Current State:** No password policy configured

**Requested Configuration:**
```bash
aws iam update-account-password-policy \
  --minimum-password-length 14 \
  --require-uppercase-characters \
  --require-lowercase-characters \
  --require-numbers \
  --require-symbols \
  --max-password-age 90 \
  --password-reuse-prevention 12 \
  --hard-expiry
```

**Justification:** SOC 2 CC6.2 requires strong authentication controls.

---

## Request 2: MFA Verification

**Status:** Pending  
**Priority:** High

**Current State:** Cannot verify MFA status (permission denied)

**Options:**
1. Grant `iam:ListVirtualMFADevices` permission to Alex.Ledbetter
2. OR provide MFA audit report showing all users have MFA enabled

**Verification Command (if permission granted):**
```bash
aws iam list-virtual-mfa-devices
```

**Justification:** SOC 2 CC6.2 requires MFA for all users.

---

## Request 3: AWS Config Setup

**Status:** Pending  
**Priority:** Medium

**Current State:** AWS Config not configured

**Required Setup:**
1. Create S3 bucket for Config snapshots
2. Create IAM role for Config service
3. Configure configuration recorder
4. Configure delivery channel

**Commands:**
```bash
# Create IAM role (requires admin)
aws iam create-role \
  --role-name ecometricx-config-role \
  --assume-role-policy-document file://config-trust-policy.json

# Create configuration recorder
aws configservice put-configuration-recorder \
  --configuration-recorder name=ecometricx-recorder,roleARN=arn:aws:iam::302146782327:role/ecometricx-config-role

# Start recording
aws configservice start-configuration-recorder \
  --configuration-recorder-name ecometricx-recorder
```

**Justification:** SOC 2 CC6.7 requires configuration change monitoring.

---

## Request 4: RDS Multi-AZ Enablement

**Status:** Pending  
**Priority:** Medium

**Current State:** No databases configured for Multi-AZ

**Affected Databases:**
- database-neurova (production MySQL)
- ecmx-tenore (production MySQL)

**Command:**
```bash
aws rds modify-db-instance \
  --db-instance-identifier database-neurova \
  --multi-az \
  --apply-immediately
```

**Justification:** SOC 2 A1.2 requires system availability controls.

---

## Completed Actions (No Admin Required)

| Item | Status | Date |
|------|--------|------|
| CloudTrail Log File Validation | COMPLETE | Dec 5, 2025 |
| S3 Bucket Security Audit | COMPLETE | Dec 5, 2025 |
| EC2 Security Group Audit | COMPLETE | Dec 5, 2025 |
| RDS Configuration Audit | COMPLETE | Dec 5, 2025 |

---

**Contact:** Alex Ledbetter  
**Follow-up Date:** December 9, 2025

