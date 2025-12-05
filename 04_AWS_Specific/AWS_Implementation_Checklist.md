# AWS Implementation Checklist for SOC 2 Compliance

**Document Version:** 1.0  
**Date:** December 2, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Purpose:** Actionable checklist of AWS configurations needed for SOC 2 Type II compliance

## Critical - Implement Immediately

### 1. IAM Password Policy ⚠️ NOT CONFIGURED

**Status:** ❌ Missing  
**Priority:** HIGH  
**SOC 2 Criteria:** CC6.2 (Access Control)  
**ISO 27001:** A.8.5 (Secure Authentication)

**Implementation:**
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

**Verification:**
```bash
aws iam get-account-password-policy
```

**Documentation:** Update `AWS_Security_Configuration_Documentation.md` after implementation

---

### 2. CloudTrail Log File Validation ⚠️ DISABLED

**Status:** ⚠️ Disabled  
**Priority:** HIGH  
**SOC 2 Criteria:** CC7.1 (System Monitoring), CC8.1 (System Operations)  
**ISO 27001:** A.8.15 (Logging), A.8.16 (Monitoring)

**Current State:**
- Trail: `ecometricx_trail`
- Log file validation: Disabled

**Implementation:**
```bash
# Enable log file validation
aws cloudtrail update-trail \
  --name ecometricx_trail \
  --enable-log-file-validation

# Verify
aws cloudtrail get-trail-status --name ecometricx_trail
```

**Why Important:**
- Detects tampering with CloudTrail log files
- Required for audit evidence integrity
- Supports compliance requirements

**Documentation:** Update `AWS_Security_Configuration_Documentation.md` and `AWS_CLI_Findings_December2025.md`

---

### 3. MFA Enforcement Verification ⚠️ CANNOT VERIFY

**Status:** ⚠️ Cannot verify (permission issue)  
**Priority:** MEDIUM-HIGH  
**SOC 2 Criteria:** CC6.2 (Access Control)  
**ISO 27001:** A.8.5 (Secure Authentication)

**Current State:**
- MFA required per Access Control Policy
- Cannot verify MFA status via CLI (permission denied)

**Implementation Steps:**

1. **Grant IAM Permissions:**
   - Add `iam:ListVirtualMFADevices` permission to user/role
   - Add `iam:ListMFADevices` permission
   - Add `iam:GetUser` permission

2. **Verify MFA Status:**
```bash
# List all MFA devices
aws iam list-virtual-mfa-devices

# Check MFA for specific user
aws iam list-mfa-devices --user-name <username>

# List all users and check MFA
for user in $(aws iam list-users --query 'Users[].UserName' --output text); do
  echo "Checking MFA for $user:"
  aws iam list-mfa-devices --user-name $user
done
```

3. **Enforce MFA via IAM Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

**Documentation:** Update `AWS_Access_Control_Matrix.md` with MFA status

---

## High Priority - Implement This Month

### 4. AWS Config Setup ⚠️ NOT CONFIGURED

**Status:** ❌ Not configured  
**Priority:** MEDIUM-HIGH  
**SOC 2 Criteria:** CC6.7 (Change Management), CC7.1 (System Monitoring)  
**ISO 27001:** A.8.9 (Configuration Management)

**Why Important:**
- Configuration compliance monitoring
- Configuration change tracking
- Resource inventory
- Configuration history

**Implementation Steps:**

1. **Create S3 Bucket for Config:**
```bash
aws s3 mb s3://ecometricx-config-bucket-302146782327
aws s3api put-bucket-versioning \
  --bucket ecometricx-config-bucket-302146782327 \
  --versioning-configuration Status=Enabled
aws s3api put-bucket-encryption \
  --bucket ecometricx-config-bucket-302146782327 \
  --server-side-encryption-configuration '{
    "Rules": [{
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "AES256"
      }
    }]
  }'
```

2. **Create IAM Role for Config:**
```bash
# Create trust policy
cat > config-trust-policy.json <<EOF
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Service": "config.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}
EOF

# Create role
aws iam create-role \
  --role-name ecometricx-config-role \
  --assume-role-policy-document file://config-trust-policy.json

# Attach managed policy
aws iam attach-role-policy \
  --role-name ecometricx-config-role \
  --policy-arn arn:aws:iam::aws:policy/service-role/ConfigRole

# Attach S3 policy
aws iam attach-role-policy \
  --role-name ecometricx-config-role \
  --policy-arn arn:aws:iam::aws:policy/service-role/ConfigRole
```

3. **Create Configuration Recorder:**
```bash
aws configservice put-configuration-recorder \
  --configuration-recorder name=ecometricx-recorder,roleARN=arn:aws:iam::302146782327:role/ecometricx-config-role
```

4. **Create Delivery Channel:**
```bash
aws configservice put-delivery-channel \
  --delivery-channel name=ecometricx-delivery-channel,s3BucketName=ecometricx-config-bucket-302146782327
```

5. **Start Configuration Recorder:**
```bash
aws configservice start-configuration-recorder \
  --configuration-recorder-name ecometricx-recorder
```

**Documentation:** Create new section in `AWS_Security_Configuration_Documentation.md`

---

### 5. S3 Bucket Security Audit

**Status:** ⚠️ Partial (encryption verified on sample)  
**Priority:** MEDIUM  
**SOC 2 Criteria:** CC6.6 (System Boundaries), CC7.3 (System Backup)  
**ISO 27001:** A.8.24 (Use of Cryptography)

**Current State:**
- 54 S3 buckets total
- Encryption verified on sample bucket (AES256)
- Full audit needed

**Implementation:**

1. **Audit All Buckets:**
```bash
#!/bin/bash
# Audit all S3 buckets for security settings

for bucket in $(aws s3api list-buckets --query 'Buckets[].Name' --output text); do
  echo "=== Auditing bucket: $bucket ==="
  
  # Check encryption
  echo "Encryption:"
  aws s3api get-bucket-encryption --bucket $bucket 2>&1 || echo "  No encryption configured"
  
  # Check versioning
  echo "Versioning:"
  aws s3api get-bucket-versioning --bucket $bucket
  
  # Check public access
  echo "Public Access:"
  aws s3api get-public-access-block --bucket $bucket 2>&1 || echo "  Public access not blocked"
  
  # Check lifecycle
  echo "Lifecycle:"
  aws s3api get-bucket-lifecycle-configuration --bucket $bucket 2>&1 || echo "  No lifecycle policy"
  
  # Check logging
  echo "Logging:"
  aws s3api get-bucket-logging --bucket $bucket 2>&1 || echo "  No logging configured"
  
  echo ""
done
```

2. **Remediate Issues:**
   - Enable encryption on buckets without it
   - Enable versioning on critical buckets
   - Block public access on all buckets
   - Configure lifecycle policies
   - Enable access logging

**Documentation:** Create `S3_Bucket_Security_Audit_Report.md`

---

### 6. CloudWatch Alarm Remediation

**Status:** ⚠️ 1 alarm in ALARM state  
**Priority:** MEDIUM  
**SOC 2 Criteria:** CC7.1 (System Monitoring)  
**ISO 27001:** A.8.16 (Monitoring)

**Current State:**
- `Low_Disk_Space_For_root_drive_r6a_4xlarge` - ALARM state
- Multiple alarms in INSUFFICIENT_DATA state

**Implementation:**

1. **Investigate ALARM:**
```bash
# Get alarm details
aws cloudwatch describe-alarms \
  --alarm-names Low_Disk_Space_For_root_drive_r6a_4xlarge

# Get metric data
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name disk_used_percent \
  --dimensions Name=InstanceId,Value=i-0e5e87039a350664d \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 300 \
  --statistics Average
```

2. **Remediate:**
   - Investigate disk space issue on instance
   - Clean up unnecessary files
   - Increase disk size if needed
   - Resolve alarm

3. **Fix INSUFFICIENT_DATA Alarms:**
   - Verify CloudWatch agent installed on instances
   - Check metric collection configuration
   - Update alarm configuration if needed

**Documentation:** Update `AWS_Security_Configuration_Documentation.md`

---

## Medium Priority - Implement Next Quarter

### 7. Enhanced CloudWatch Monitoring

**Status:** ⚠️ Basic monitoring configured  
**Priority:** MEDIUM  
**SOC 2 Criteria:** CC7.1 (System Monitoring), PI1 (Processing Integrity)

**Implementation:**

1. **Add Processing Integrity Alarms:**
   - Data processing error rates
   - Data quality metric thresholds
   - Processing latency thresholds
   - Reconciliation failure alerts

2. **Add Security Alarms:**
   - Unauthorized API calls
   - Root account usage
   - Failed login attempts
   - Security group changes
   - IAM policy changes

3. **Create Custom Dashboards:**
   - Security dashboard
   - Processing integrity dashboard
   - Availability dashboard

**Commands:**
```bash
# Create alarm for high error rate
aws cloudwatch put-metric-alarm \
  --alarm-name Processing-High-Error-Rate \
  --alarm-description "Alert when processing error rate exceeds threshold" \
  --metric-name ErrorRate \
  --namespace Custom/Processing \
  --statistic Average \
  --period 300 \
  --threshold 5.0 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2

# Create dashboard
aws cloudwatch put-dashboard \
  --dashboard-name SOC2-Security-Dashboard \
  --dashboard-body file://dashboard.json
```

---

### 8. S3 Bucket Key Enablement

**Status:** ⚠️ Not enabled  
**Priority:** LOW-MEDIUM  
**Impact:** Cost optimization, performance improvement

**Implementation:**
```bash
# Enable bucket key for a bucket
aws s3api put-bucket-encryption \
  --bucket <bucket-name> \
  --server-side-encryption-configuration '{
    "Rules": [{
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "aws:kms",
        "KMSMasterKeyID": "alias/aws/s3"
      },
      "BucketKeyEnabled": true
    }]
  }'
```

**Note:** Requires KMS encryption (not SSE-S3)

---

### 9. RDS Backup Verification

**Status:** ⚠️ Needs verification  
**Priority:** MEDIUM  
**SOC 2 Criteria:** CC7.3 (System Backup)

**Implementation:**

1. **Verify Backup Configuration:**
```bash
# List all RDS instances
aws rds describe-db-instances \
  --query 'DBInstances[].[DBInstanceIdentifier,BackupRetentionPeriod,MultiAZ,StorageEncrypted]' \
  --output table

# Verify automated backups
for db in $(aws rds describe-db-instances --query 'DBInstances[].DBInstanceIdentifier' --output text); do
  echo "Checking backups for $db:"
  aws rds describe-db-instances \
    --db-instance-identifier $db \
    --query 'DBInstances[0].[BackupRetentionPeriod,AutomatedBackups]' \
    --output table
done
```

2. **Verify Backup Restoration:**
   - Test backup restoration procedures
   - Document restoration times
   - Update `AWS_Backup_Recovery_Procedures.md`

---

### 10. EC2 Security Group Review

**Status:** ⚠️ Needs comprehensive review  
**Priority:** MEDIUM  
**SOC 2 Criteria:** CC6.2 (Access Control)

**Implementation:**

1. **Audit Security Groups:**
```bash
# List all security groups
aws ec2 describe-security-groups \
  --query 'SecurityGroups[].[GroupId,GroupName,Description]' \
  --output table

# Check for overly permissive rules
aws ec2 describe-security-groups \
  --filters "Name=ip-permission.cidr,Values=0.0.0.0/0" \
  --query 'SecurityGroups[].[GroupId,GroupName,IpPermissions]' \
  --output json
```

2. **Remediate:**
   - Remove overly permissive rules
   - Implement least privilege
   - Document security group purposes
   - Review regularly

---

## Implementation Priority Summary

### This Week (Critical)
1. ✅ IAM Password Policy
2. ✅ CloudTrail Log File Validation
3. ⚠️ MFA Verification (requires permission grant)

### This Month (High Priority)
4. AWS Config Setup
5. S3 Bucket Security Audit
6. CloudWatch Alarm Remediation

### Next Quarter (Medium Priority)
7. Enhanced CloudWatch Monitoring
8. S3 Bucket Key Enablement
9. RDS Backup Verification
10. EC2 Security Group Review

---

## Verification Commands

After implementing each item, verify with:

```bash
# Password policy
aws iam get-account-password-policy

# CloudTrail
aws cloudtrail get-trail-status --name ecometricx_trail

# AWS Config
aws configservice describe-configuration-recorders
aws configservice describe-delivery-channels

# MFA
aws iam list-virtual-mfa-devices

# S3 encryption
aws s3api get-bucket-encryption --bucket <bucket-name>

# CloudWatch alarms
aws cloudwatch describe-alarms --state-value ALARM
```

---

## Related Documents

- `AWS_CLI_Findings_December2025.md` - Detailed findings
- `AWS_Security_Configuration_Documentation.md` - Current configuration
- `AWS_Processing_Integrity_Controls.md` - Processing integrity controls
- `AWS_Access_Control_Matrix.md` - IAM user mapping

---

**Document Control:**
- **Version:** 1.0
- **Date:** December 2, 2025
- **Next Review:** After each implementation
- **Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)

