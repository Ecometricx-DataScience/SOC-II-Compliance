# AWS Backup and Recovery Procedures

**Document Version:** 1.0  
**Last Updated:** December 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Quarterly

## Overview

This document outlines backup and recovery procedures for AWS resources to ensure business continuity and meet SOC 2 Trust Service Criteria for Availability and System Backup (CC7.3).

## Backup Strategy

### Backup Objectives

- **Recovery Time Objective (RTO):** Varies by service (see service-specific sections)
- **Recovery Point Objective (RPO):** Varies by service (see service-specific sections)
- **Retention Period:** Per data classification and regulatory requirements

## S3 Backup Procedures

### Backup Configuration

**Automated Backups:**
- Versioning enabled on all critical buckets
- Cross-region replication for production data buckets
- Lifecycle policies for automated archival

**Backup Schedule:**
- Real-time versioning (all object versions retained)
- Daily snapshots for critical buckets
- Weekly full backups for standard buckets

**Retention:**
- Production data: 7 years (compliance requirement)
- Development data: 30 days
- Log data: Per logging policy (typically 1-7 years)

### Recovery Procedures

**Point-in-Time Recovery:**
1. Identify the S3 bucket and object key
2. Determine the version ID or timestamp for recovery
3. Use AWS Console or CLI to restore specific version:
   ```bash
   aws s3api restore-object --bucket <bucket-name> --key <object-key> --version-id <version-id>
   ```
4. Verify restored object integrity
5. Document recovery in incident log

**Cross-Region Recovery:**
1. Identify source region and destination region
2. Verify replication status
3. Restore from replicated bucket if source unavailable
4. Verify data integrity
5. Document recovery process

### Backup Testing

- **Frequency:** Quarterly
- **Procedure:**
  1. Select test object from production bucket
  2. Delete or corrupt test object
  3. Restore from backup
  4. Verify data integrity
  5. Document test results

## RDS Backup Procedures

### Backup Configuration

**Automated Backups:**
- Automated daily backups enabled for all RDS instances
- Backup retention: 7 days (production), 3 days (development)
- Multi-AZ deployments for production databases

**Backup Schedule:**
- Daily automated backups during maintenance window
- Transaction log backups every 5 minutes (for point-in-time recovery)

**Retention:**
- Production databases: 7 days automated backups
- Development databases: 3 days automated backups
- Manual snapshots: Retained per data classification requirements

### Recovery Procedures

**Point-in-Time Recovery:**
1. Identify target database and recovery point
2. Determine RPO (maximum data loss acceptable)
3. Use AWS Console or CLI to restore:
   ```bash
   aws rds restore-db-instance-to-point-in-time \
     --source-db-instance-identifier <source-instance> \
     --target-db-instance-identifier <target-instance> \
     --restore-time <timestamp>
   ```
4. Verify database integrity
5. Update application connection strings if needed
6. Document recovery in incident log

**Snapshot Recovery:**
1. Identify snapshot to restore
2. Create new RDS instance from snapshot:
   ```bash
   aws rds restore-db-instance-from-db-snapshot \
     --db-instance-identifier <new-instance> \
     --db-snapshot-identifier <snapshot-id>
   ```
3. Verify database integrity
4. Update application connection strings
5. Document recovery process

### Backup Testing

- **Frequency:** Quarterly
- **Procedure:**
  1. Create test database from production snapshot
  2. Verify data integrity
  3. Test application connectivity
  4. Document test results
  5. Delete test database

## EC2 Backup Procedures

### Backup Configuration

**AMI Snapshots:**
- Weekly AMI snapshots for critical instances
- Automated snapshots via AWS Backup service
- EBS volume snapshots for data volumes

**Backup Schedule:**
- Critical instances: Daily AMI snapshots
- Standard instances: Weekly AMI snapshots
- EBS volumes: Daily snapshots for production data volumes

**Retention:**
- Production AMIs: 30 days
- Production EBS snapshots: 30 days
- Development: 7 days

### Recovery Procedures

**AMI Recovery:**
1. Identify AMI to restore
2. Launch new EC2 instance from AMI:
   ```bash
   aws ec2 run-instances --image-id <ami-id> --instance-type <type>
   ```
3. Verify instance configuration
4. Update security groups and network settings
5. Verify application functionality
6. Document recovery process

**EBS Volume Recovery:**
1. Identify snapshot to restore
2. Create new EBS volume from snapshot:
   ```bash
   aws ec2 create-volume --snapshot-id <snapshot-id> --availability-zone <az>
   ```
3. Attach volume to instance
4. Mount volume and verify data
5. Document recovery process

### Backup Testing

- **Frequency:** Quarterly
- **Procedure:**
  1. Launch test instance from production AMI
  2. Verify instance functionality
  3. Restore test EBS volume
  4. Verify data integrity
  5. Document test results
  6. Terminate test resources

## DynamoDB Backup Procedures

### Backup Configuration

**Point-in-Time Recovery:**
- Enabled for production tables
- Continuous backups with 35-day recovery window

**On-Demand Backups:**
- Manual backups before major changes
- Retention: Per data classification requirements

### Recovery Procedures

**Point-in-Time Recovery:**
1. Identify table and recovery point
2. Use AWS Console or CLI to restore:
   ```bash
   aws dynamodb restore-table-to-point-in-time \
     --source-table-name <source-table> \
     --target-table-name <target-table> \
     --restore-date-time <timestamp>
   ```
3. Verify table data integrity
4. Update application table references if needed
5. Document recovery process

## Backup Monitoring

### CloudWatch Alarms

- Backup failure alarms configured
- Snapshot completion monitoring
- Backup storage usage monitoring

### Backup Verification

- **Daily:** Automated backup completion checks
- **Weekly:** Backup integrity spot checks
- **Quarterly:** Full backup and recovery testing

## Disaster Recovery

### Recovery Sites

- **Primary Region:** us-east-1
- **Secondary Region:** us-west-2 (for critical data)
- **Backup Storage:** S3 cross-region replication

### Recovery Procedures

1. **Assessment:** Determine scope of disaster
2. **Activation:** Activate disaster recovery plan
3. **Recovery:** Restore from backups in secondary region if needed
4. **Validation:** Verify all systems operational
5. **Documentation:** Document recovery process and lessons learned

## Compliance Mapping

### SOC 2 CC7.3 (System Backup)

- **Requirement:** System backup processes create retrievable copies of information
- **Implementation:**
  - Automated backups for all critical systems
  - Regular backup testing
  - Documented recovery procedures
  - Backup retention per regulatory requirements

### Regulatory Requirements

- **GDPR:** Data backup and recovery procedures documented
- **HIPAA:** PHI backup and recovery with encryption
- **PCI-DSS:** Cardholder data backup with encryption and access controls

## Related Documents

- `../03_Plans/Backup_Recovery_Template.doc` - Backup and recovery plan template
- `../03_Plans/Disaster_Recovery_Plan_2025.docx` - Disaster recovery plan
- `AWS_Security_Configuration_Documentation.md` - Overall AWS security configuration
- `../01_Policies/Access_Control_Policy.docx` - Access control for backup systems

## Review History

| Date | Reviewer | Changes | Next Review |
|------|----------|---------|-------------|
| December 2025 | InfoSec Team | Initial documentation | Q1 2026 |

