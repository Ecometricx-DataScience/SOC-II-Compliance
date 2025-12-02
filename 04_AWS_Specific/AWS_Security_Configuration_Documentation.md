# AWS Security Configuration Documentation

**Document Version:** 1.0  
**Last Updated:** December 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Quarterly

## Overview

This document provides a comprehensive overview of EcoMetricx's AWS security configuration, infrastructure, and compliance posture for SOC 2 Type II audit purposes.

## AWS Account Information

- **Account ID:** 302146782327
- **Primary Region:** us-east-1
- **Multi-Region Deployment:** Yes (resources across multiple regions)

## Infrastructure Summary

### EC2 Instances

EcoMetricx maintains multiple EC2 instances across various instance types:

- **Total Instances:** 11 instances
- **Instance Types:** Mix of t2.micro, t3.micro, t3.small, t3a.xlarge, r6i.xlarge, r6a.4xlarge, r6g.4xlarge
- **States:** Mix of running and stopped instances

**Security Considerations:**
- All instances should be tagged according to AWS Tagging Standards Policy (see `AWS_Tagging_Standards_Reference.md`)
- Security groups configured for network access control
- Regular security patching required
- Instance access should follow Access Control Policy requirements

### S3 Buckets

EcoMetricx maintains 50+ S3 buckets for various purposes including:

- Data storage and archival
- Application deployments (Amplify)
- Backup storage
- Analytics data (e.g., britbox-prod-* buckets)
- Webhook processing (axentra-webhook-*)
- CloudTrail logs (ecometricx-cloudtrail-bucket)

**Security Considerations:**
- Bucket encryption at rest (AES-256) required
- Bucket versioning enabled for critical data
- Access logging enabled
- Lifecycle policies configured per data retention requirements
- Public access blocked by default
- Bucket policies aligned with least privilege principle

### RDS Databases

EcoMetricx maintains 4 RDS database instances:

1. **database-neurova** - MySQL - Available
2. **ecmx-tenore** - MySQL - Available
3. **ecometricx-serverless-rds-instance-1** - Aurora MySQL - Available
4. **vox-instance-1** - Aurora MySQL - Available

**Security Considerations:**
- Encryption at rest enabled
- Encryption in transit (TLS) required
- Automated backups configured
- Multi-AZ deployment for production databases
- Database access restricted via security groups
- Database credentials managed through AWS Secrets Manager or IAM database authentication

### CloudFormation Stacks

Multiple CloudFormation stacks deployed for infrastructure as code:

- Amplify application stacks
- CDK toolkit stacks
- Application-specific infrastructure stacks

**Security Considerations:**
- Stack templates stored in version control
- Stack changes require approval workflow
- Stack drift detection enabled

## Access Control

### IAM Users

EcoMetricx maintains 30+ IAM users for various purposes:

- Individual user accounts
- Service accounts (e.g., ECR GitHub push users)
- Application-specific users
- SES SMTP users

**Access Control Requirements:**
- All users must have MFA enabled
- Access reviews conducted quarterly
- Least privilege principle applied
- Unused accounts deactivated
- See `AWS_Access_Control_Matrix.md` for detailed user mapping

### Security Groups

Multiple security groups configured for network access control:

- Application load balancer security groups
- EC2 instance security groups
- Database security groups
- SageMaker domain security groups
- IP whitelist security groups for specific resources

**Security Considerations:**
- Security groups follow least privilege principle
- Inbound rules restricted to necessary ports and sources
- Outbound rules reviewed regularly
- Security group changes logged and audited

## Monitoring and Logging

### CloudWatch

- CloudWatch alarms configured for:
  - EC2 instance health checks
  - Disk space monitoring
  - Auto Scaling group metrics
  - DynamoDB capacity tracking
  - Application performance metrics

**Monitoring Requirements:**
- Critical alarms configured with SNS notifications
- Alarm thresholds reviewed quarterly
- Log retention per Logging and Monitoring Policy

### CloudTrail

- CloudTrail enabled for API logging
- Logs stored in dedicated S3 bucket: `ecometricx-cloudtrail-bucket`
- Log file integrity validation enabled

## AWS Services in Use

- **EC2:** Compute instances
- **S3:** Object storage
- **RDS:** Managed databases
- **CloudFormation:** Infrastructure as code
- **CloudWatch:** Monitoring and logging
- **CloudTrail:** API logging
- **IAM:** Identity and access management
- **Amplify:** Application hosting and deployment
- **SageMaker:** Machine learning services
- **ECS:** Container orchestration
- **Lambda:** Serverless compute
- **Glue:** ETL services
- **DataZone:** Data governance

## Tagging Standards

All AWS resources must be tagged according to the **ECMX AWS Tagging Standards Policy** (v1.1):

- See `AWS_Tagging_Standards_Reference.md` for complete tagging requirements
- Tags include: Environment, Project, Owner, DataClassification, Compliance
- Tagging enables resource organization, cost allocation, and compliance tracking

## Backup and Recovery

- **S3:** Versioning and cross-region replication for critical buckets
- **RDS:** Automated daily backups with configurable retention
- **EC2:** AMI snapshots for critical instances
- See `AWS_Backup_Recovery_Procedures.md` for detailed procedures

## Incident Response

- AWS-specific incident response procedures documented
- CloudWatch alarms trigger incident response workflows
- See `AWS_Incident_Response_Procedures.md` for detailed procedures

## Compliance Mapping

### SOC 2 Trust Service Criteria

- **CC6.2 (Access Control):** IAM policies, security groups, MFA
- **CC6.7 (Change Management):** CloudFormation, version control
- **CC7.1 (System Monitoring):** CloudWatch, CloudTrail
- **CC7.2 (Incident Response):** Incident response procedures
- **CC7.3 (System Backup):** S3 versioning, RDS backups, AMI snapshots

### Regulatory Compliance

- **GDPR:** Data residency, encryption, access controls
- **HIPAA:** PHI protection, encryption, audit logging
- **PCI-DSS:** Cardholder data protection, encryption, access restrictions

## Security Best Practices

1. **Encryption:**
   - All data encrypted at rest (S3, RDS, EBS)
   - All data encrypted in transit (TLS 1.2+)

2. **Access Control:**
   - MFA required for all IAM users
   - Least privilege access policies
   - Regular access reviews

3. **Monitoring:**
   - CloudWatch alarms for critical metrics
   - CloudTrail for API audit logging
   - Regular security assessments

4. **Backup:**
   - Automated backups for all critical data
   - Backup testing and restoration procedures
   - Off-site backup storage

5. **Change Management:**
   - Infrastructure as code (CloudFormation)
   - Version control for all changes
   - Change approval workflows

## Review and Maintenance

- **Quarterly Reviews:** Security configuration, access controls, monitoring
- **Annual Reviews:** Complete security posture assessment
- **Continuous:** Monitoring, alerting, and incident response

## Related Documents

- `AWS_Access_Control_Matrix.md` - Detailed IAM user and role mapping
- `AWS_Backup_Recovery_Procedures.md` - Backup and recovery procedures
- `AWS_Incident_Response_Procedures.md` - Cloud-specific incident response
- `AWS_Tagging_Standards_Reference.md` - AWS resource tagging standards
- `../01_Policies/Access_Control_Policy.docx` - Access control policy
- `../01_Policies/Logging_Monitoring_Policy.docx` - Logging and monitoring policy

