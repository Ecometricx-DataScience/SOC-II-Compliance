# ECMX AWS Tagging Standards Policy

**Source:** [Confluence](https://ecometricx.atlassian.net/wiki/spaces/ATS/pages/579993604/ECMX+AWS+Tagging+Standards+Policy)

**Last Modified:** 2025-11-12T19:15:10.488Z
**Modified By:** Conrad Jahn

---

Version: 1.1 (with Data Lineage)   Effective Date: November 2025   Owner: Conrad   Review Cycle: Quarterly

1. Executive Summary

This policy establishes mandatory tagging standards for all AWS resources at ECMX to enable:

Cost allocation and chargeback by project and team

Resource inventory and lifecycle management

Security and compliance tracking

Automated governance and operations

Quick identification of resource ownership and purpose

Data lineage tracking and impact analysis for analytics workloads

All AWS resources must comply with this policy. Non-compliant resources may be subject to automated remediation or termination after a grace period.

2. Scope

Applies to: All AWS accounts, resources, and services within the ECMX AWS Organization

Key Resources (but not limited to):

Compute: EC2, ECS, Lambda, Fargate

Storage: S3, EBS, EFS

Database: RDS, Redshift, DynamoDB

Analytics: Glue, Athena, EMR, Data Catalog

Networking: VPC, ELB/ALB, Route 53

Containers: ECR, EKS

IAM: Roles, Policies

Other: KMS keys, Step Functions, SNS/SQS

3. Naming Convention Standard

3.1 General Format
wide1800
3.2 Component Definitions

Component

Description

Format

Examples

project

Customer or project name

3-15 chars, lowercase, alphanumeric + hyphens

shopify, franklin, sjce, internal-tools

environment

Deployment environment

dev, test, stage, prod, sandbox

dev, prod

service

AWS service abbreviation

Standard abbreviations (see 3.3)

ec2, rds, s3

purpose

Specific function or component

3-20 chars, descriptive

web, api, worker, logs

region

AWS region code

Standard AWS codes

us-east-1, us-west-2

Project Name Guidelines:

Use customer name or abbreviation when project is customer-specific (e.g., shopify, franklin, sjce)

For multi-word customers, use hyphens (e.g., san-jose-ce or abbreviate as sjce)

For internal/shared projects, use descriptive names (e.g., internal-tools, shared-analytics)

Keep customer names recognizable but consider abbreviations for very long names

Document the mapping in the Project Registry (see Section 16)

3.3 Service Abbreviations

Service

Abbreviation

Service

Abbreviation

EC2

ec2

RDS

rds

S3

s3

Redshift

redshift

Lambda

lambda

DynamoDB

dynamodb

ECS

ecs

Glue

glue

ELB/ALB

alb or elb

EMR

emr

VPC

vpc

Athena

athena

EBS

ebs

KMS

kms

ECR

ecr

IAM

iam

3.4 Examples

Customer Projects:
wide1800
Internal/Shared Projects:
wide1800
Analytics Projects:
wide1800
3.5 Special Cases

S3 Buckets:

Must be globally unique, so add account ID or timestamp suffix if needed

Format: <project>-<environment>-<purpose>-<account-id>

Example: shopify-prod-logs-123456789012, sjce-dev-raw-data-123456789012

IAM Roles/Policies:

Format: <project>-<environment>-<service>-<purpose>-role

Example: shopify-prod-ec2-api-role, franklin-dev-lambda-processor-role

Analytics/Data Resources:

Include data classification hint in purpose

Example: franklin-prod-s3-sensitive-data-us-east-1, sjce-prod-s3-billing-data-us-east-1

Customer Confidentiality:

If customer name must remain confidential, use an internal project code and maintain mapping in secure Project Registry

Example: Use proj-001 instead of customerX, document mapping separately

4. Required Tags

4.1 Mandatory Tags (All Resources)

Tag Key

Description

Allowed Values

Example

Project

Customer or project identifier

Any valid customer/project name

Shopify, Franklin, SJCE, InternalTools

Environment

Deployment environment

Dev, Test, Stage, Prod, Sandbox

Prod

Owner

Email of primary technical contact

Valid email address

john.doe@ecmx.com

CostCenter

Department or team for billing

Finance-approved cost center code

ENG-001, DATA-002, CLIENT-SERVICES

ManagedBy

Provisioning method

Terraform, CloudFormation, ServiceCatalog, Manual, ControlTower

Terraform

4.2 Conditional Tags (Required Based on Resource Type)

Tag Key

Description

Required For

Allowed Values

Example

DataClassification

Data sensitivity level

All data storage (S3, RDS, Redshift, EBS, Glue)

Public, Internal, Confidential, Restricted

Confidential

Compliance

Regulatory requirements

Resources with compliance needs

HIPAA, SOC2, PCI, None

SOC2

BackupPolicy

Backup retention requirement

Production data resources

Daily, Weekly, Monthly, None

Daily

ApplicationRole

Application tier/function

Compute resources (EC2, ECS, Lambda)

Web, API, Database, Worker, Analytics

API

4.3 Data Flow Tags (Required for Data Resources)

Tag Key

Description

Required For

Format

Example

SourceSystems

Upstream data sources

All data processing resources (S3 data layers, Glue jobs, Lambda ETL)

Comma-separated list

Salesforce,SAP,ShopifyAPI

TargetSystems

Downstream consumers

All data storage/output resources (S3 refined, Redshift, data marts)

Comma-separated list

Redshift,Tableau,PowerBI

DataDomain

Business data domain

All data resources (S3 buckets, Glue databases, Redshift schemas)

Domain name

Customer, Product, Order, Finance

ProcessOwner

Team managing data flow

All data processing resources

Team name

DataEngineering, AnalyticsTeam

4.4 Optional Tags (Recommended)

Tag Key

Description

Use Case

Example

TTL

Time-to-live for ephemeral resources

Dev/test resources

2024-12-31, 7d, 30d

Version

Application or infrastructure version

Version tracking

v1.2.3, 2024.11

Contact

Secondary contact or team

Additional ownership info

data-engineering-team

BusinessUnit

Higher-level org structure

Multi-department orgs

Engineering, DataScience

Criticality

Business impact

DR planning

Critical, High, Medium, Low

ChangeWindow

Approved maintenance window

Production resources

Tue-0200-0600-UTC, Weekend

AutoShutdown

Auto-stop schedule

Cost optimization

Weeknights, Weekends, Never

5. Tag Value Standards

5.1 General Rules

Case sensitivity: Tag keys are case-insensitive in AWS but enforce PascalCase for consistency

Values: Use consistent capitalization (TitleCase for words, lowercase for codes)

No spaces: Use hyphens or underscores in multi-word values

Length limits:

Keys: Max 128 characters

Values: Max 256 characters

Special characters: Avoid except hyphens, underscores, periods, and forward slashes

5.2 Environment Values (Standardized)

Dev - Development/feature branches

Test - QA and testing environments

Stage - Pre-production/staging

Prod - Production

Sandbox - Experimental/learning environments (auto-cleanup)

5.3 DataClassification Values

Level

Description

Examples

Public

Publicly available data

Marketing content, public documentation

Internal

Internal use only

Employee directories, internal docs

Confidential

Sensitive business data

Financial records, customer PII

Restricted

Highly sensitive/regulated

PHI, PCI data, credentials

6. Service-Specific Tagging Requirements

6.1 EC2 Instances

Required: All mandatory tags + ApplicationRole, BackupPolicy (if prod)   Naming: <project>-<env>-ec2-<purpose>-<region>   Example: shopify-prod-ec2-web-us-east-1   Best Practice: Tag both instance and all attached EBS volumes

6.2 S3 Buckets

Required: All mandatory tags + DataClassification + Data lineage tags (SourceSystems, TargetSystems, DataDomain, ProcessOwner) for data lake buckets   Naming: <project>-<env>-<purpose>-<account-id>   Example: franklin-prod-analytics-123456789012   Best Practice: Apply bucket policies to enforce tagging on objects

6.3 RDS & Redshift

Required: All mandatory tags + DataClassification, BackupPolicy, Compliance + Data lineage tags (SourceSystems, TargetSystems, DataDomain, ProcessOwner)   Naming: RDS: <project>-<env>-rds-<purpose>-<region>   Example: sjce-prod-rds-billing-us-west-2   Best Practice: Tag snapshots with same tags as source database

6.4 Lambda Functions

Required: All mandatory tags + ApplicationRole   Naming: <project>-<env>-lambda-<purpose>-<region>   Example: shopify-prod-lambda-order-processor-us-east-1   Best Practice: Tag execution roles with same project tags

6.5 Analytics (Glue, Athena, EMR)

Required: All mandatory tags + DataClassification + Data lineage tags (SourceSystems, TargetSystems, DataDomain, ProcessOwner)   Naming:

Glue DB: <project>_<env>_<purpose>_db

Crawlers: <project>-<env>-crawler-<purpose>

EMR: <project>-<env>-emr-<purpose>   Example: franklin_prod_sales_db, sjce-prod-crawler-meter-data   Best Practice: Tag Data Catalog tables, crawlers, and ETL jobs consistently with data lineage information

6.6 IAM Roles

Required: Project, Environment, Owner, ManagedBy   Naming: <project>-<env>-<service>-<purpose>-role   Example: shopify-prod-ec2-api-role   Best Practice: Tag policies attached to roles with same project tags

7. Implementation Approach

7.1 Phase 1: Discovery & Backfill (Month 1-2)

Create Project Registry: Document all current customer projects with names, codes, owners, accounts

Inventory: Export all existing resources and current tags

Heuristic Tagging: Use automated scripts to backfill tags based on:

Resource names (match to Project Registry)

VPC/subnet association

Security group names

CloudTrail creator identity

Existing partial tags

Manual Review: Owner verification for ambiguous resources; validate customer name usage

Target: ≥80% tag coverage by end of Month 2, Project Registry complete

7.2 Phase 2: Soft Enforcement (Month 2-3)

Tag Policies: Deploy AWS Organizations Tag Policies (non-blocking)

Dashboards: Cost Explorer by Project, Resource Groups by compliance status

Notifications: Weekly reports to teams on untagged resources

Target: ≥90% tag coverage, teams aware of requirements

7.3 Phase 3: Hard Enforcement (Month 4+)

Preventive Controls:

Service Catalog/Terraform templates enforce tags at creation

SCPs deny resource creation without required tags (pilot first)

Detective Controls:

AWS Config rules flag non-compliant resources

Daily automated reports

Corrective Controls:

Auto-tagging via Lambda for low-risk resources

Auto-stop/delete for untagged sandbox resources after grace period

Target: ≥95% tag coverage, <5% weekly non-compliance rate

8. Governance & Enforcement

8.1 Tag Policies (AWS Organizations)

Deploy Organization-level Tag Policies to:

Define required tag keys per OU

Restrict tag values to allowed list (e.g., Environment must be Dev/Test/Stage/Prod)

Inheritance model: Prod OU has stricter requirements than Dev/Sandbox

8.2 AWS Config Rules

Rules to Enable:

required-tags: Flag resources missing mandatory tags

approved-tag-values: Validate environment/classification values

s3-bucket-tagging: Ensure all S3 buckets are tagged

ec2-instance-tagging: Ensure EC2 instances and volumes tagged

rds-cluster-tagging: Database tagging compliance

data-lineage-tags: Validate SourceSystems/TargetSystems/DataDomain/ProcessOwner on data resources (custom rule)

Remediation:

Month 1-3: Report only

Month 4+: Auto-remediate safe rules (add missing tags from resource name)

Month 6+: Isolate/stop non-compliant resources in Dev/Sandbox

8.3 Cost Allocation Tags

Activate in Billing:

Project

Environment

CostCenter

Owner

Enable these tags for cost tracking and create Cost Explorer views for chargeback.

8.4 Exemption Process

Rare cases may require exemption:

Submit request via ticket to Platform team

Include: Resource ARN, reason, temporary/permanent

Platform team approves with expiration date

Exempted resources tracked in exceptions registry

Review all exemptions quarterly

9. Automation & Tooling

9.1 Tag Backfill Scripts

Language: Python/Boto3 or AWS CLI   Logic:

Load valid project codes from Project Registry for validation

Parse resource names to extract project/environment

Cross-reference VPC/security group tags

Query CloudTrail for creator email (set as Owner)

Validate extracted project names against Project Registry

Apply heuristic tags with ManagedBy=AutoTagging

Flag ambiguous resources or unrecognized project names for manual review

Example Projects:

aws-tag-backfiller (open source)

AWS Resource Groups Tagging API

Custom Lambda function triggered by EventBridge

Integration with Project Registry:

Script reads Project Registry (CSV/JSON export or API)

Validates all Project tag values exist in registry

Auto-corrects common misspellings (e.g., shopfy → shopify)

Alerts Platform team when new project detected not in registry

9.2 Template Enforcement

Terraform:
wide1800
CloudFormation:
wide1800
9.3 Compliance Monitoring

Weekly Automated Reports:

List of untagged resources by account

Tag coverage % by service type

Cost allocation tag activation status

Data lineage completeness for data resources

Send to project Owners and Platform team

Dashboard Tools:

AWS Resource Groups (filtered views)

AWS Config Advanced Queries

Cost Explorer by tag

Custom CloudWatch dashboard

Data lineage visualization using Resource Groups queries

9.4 Data Lineage Visualization & Documentation

Automated Lineage Mapping: Use AWS Resource Groups Tagging API to query resources and generate lineage graphs:
wide1800
Documentation Generation:

Auto-generate data flow diagrams from tags using Graphviz or Mermaid

Create data catalogs with lineage information

Build impact analysis reports (what breaks if source X changes?)

Generate compliance audit trails for data movement

Integration with AWS Glue Data Catalog:

Tag Glue tables with lineage information

Use Glue Data Catalog APIs to enrich metadata

Build data quality dashboards showing source→target flows

Alerting on Lineage Gaps:

EventBridge rule triggers on new data resource creation

Lambda checks for presence of lineage tags

Slack/email alert to data team if tags missing

Weekly summary of non-compliant data resources

10. Special Considerations

10.1 Analytics Data Lakes

For Glue, Athena, and S3 data lake resources:

Database naming: <project>_<env>_<data-domain>_db (underscores for Glue compatibility)

Example: sjce_prod_meter_data_db, franklin_prod_sales_db

Bucket structure: <project>-<env>-datalake-<layer>-<account-id>

Layers: raw, curated, refined, sandbox

Example: sjce-prod-datalake-raw-123456789012, shopify-prod-datalake-curated-123456789012

Catalog tables: Inherit tags from database, add DataClassification based on columns

Crawlers: Tag with owning project + ApplicationRole=DataCrawler

Example: sjce-prod-crawler-meter-readings, franklin-dev-crawler-transactions

10.2 Data Lineage Tracking

Data lineage tags enable end-to-end tracking of data flow through the analytics pipeline.

Required Tags for Data Resources:

SourceSystems: Where the data comes from (comma-separated)

TargetSystems: Where the data goes to (comma-separated)

DataDomain: Business domain classification

ProcessOwner: Team responsible for the data pipeline

Data Layer Guidelines:

Raw/Landing Zone (S3):
wide1800
Curated/Transformed Layer (S3 + Glue):
wide1800
Refined/Analytics-Ready (Redshift, S3):
wide1800
Data Processing (Glue Jobs, Lambda):
wide1800
Complete Example - SJCE Meter Data Pipeline:
wide1800
Data Domain Standards:

Common data domains to use (maintain consistency):

Customer - Customer profiles, accounts, contacts

Product - Product catalog, inventory, SKUs

Order - Orders, transactions, fulfillment

Finance - Invoices, payments, revenue

MeterData - Utility meter readings (SJCE-specific)

MarketData - Financial market data (Franklin-specific)

Inventory - Stock, warehouse, supply chain

Benefits of Data Lineage Tags:

Impact Analysis: Quickly find what breaks when a source changes

Compliance: Track data flow for GDPR/SOC2 audit trails

Troubleshooting: Identify data quality issues at source

Documentation: Auto-generate data flow diagrams from tags

Cost Attribution: Understand which systems drive processing costs

Automation Opportunities:

Generate lineage graphs using AWS Resource Groups queries

Build Glue Data Catalog lineage views from tags

Create automated impact analysis reports

Alert on missing lineage tags for new data resources

10.3 Shared Services

Resources used by multiple projects:

Set Project=SharedServices

Add SharedBy=<project1,project2,project3> tag

Allocate costs via split billing logic in FinOps reports

Examples: Centralized logging, shared VPN, SSO directory

10.4 Migration Period

During the 6-9 month restructure:

Legacy resources: Tag with MigrationStatus=Pending|InProgress|Complete|Deprecated

Target accounts: Tag with MigrationPhase=<month-number>

Track migration progress via this tag in reports

10.5 Sandbox Auto-Cleanup

Sandbox/dev resources without TTL tag:

Default TTL: 30 days from creation

Auto-stop after 30 days (EC2, RDS)

Auto-delete after 45 days (with 7-day warning notification)

Override by adding TTL=Permanent tag with approval

11. Training & Documentation

11.1 Quick Start Guide

For Engineers (1-page):

All resources must have 5 mandatory tags (Project, Environment, Owner, CostCenter, ManagedBy)

Use naming convention: <project>-<env>-<service>-<purpose>-<region>

Project = customer name or code (e.g., shopify, sjce, franklin)

Check the Project Registry (wiki) to confirm your project's code and tags

Copy tags from existing resources in your customer's project

Data storage requires DataClassification tag

Data resources (S3 data lake, Glue, Redshift) require data lineage tags: SourceSystems, TargetSystems, DataDomain, ProcessOwner

Verify compliance in Resource Groups dashboard

11.2 Migration Runbook

For Project Teams:

Inventory current resources: aws resourcegroupstaggingapi get-resources

Apply tags using CLI or console

Verify with compliance dashboard

Submit migration request when 100% compliant

Validate post-migration using same tags

11.3 Support

Slack Channel: #aws-tagging-help

Documentation: Internal wiki - "AWS Standards"

Office Hours: Platform team office hours, Tuesdays 2-3pm

Ticket System: Tag requests, exemptions, questions

12. Metrics & KPIs

12.1 Tag Coverage (Target: ≥95%)
wide1800
12.2 Data Lineage Coverage (Target: ≥90%)
wide1800
Where data resources = S3 data lake buckets, Glue databases/jobs, Redshift clusters, RDS analytics databases

12.3 Cost Allocation Accuracy (Target: ≥90%)
wide1800
12.4 Compliance Rate (Target: ≥95%)
wide1800
12.5 Time to Tag (Target: <2 days)

Average time from resource creation to full tag compliance

12.6 Monthly Tracking

Report these KPIs in monthly platform review meetings and trend over time.

13. Roles & Responsibilities

Role

Responsibility

Platform Team

Policy creation, automation, Config rules, Tag Policies, support

Project Teams

Apply tags to resources, maintain accuracy, respond to non-compliance notifications

Operations

Cost allocation tags, chargeback reports, budget monitoring by tag

Security 

Compliance tag validation, data classification enforcement

Resource Owners

Keep Owner tag current, update tags when ownership changes

14. Policy Review & Updates

Review Cycle: Quarterly

Change Process:

Propose changes via RFC (Request for Comments)

2-week comment period

Platform team approval

30-day notice before enforcement

Update version number and changelog

Version History:

v1.1 (Nov 2025): Added data lineage tracking requirements (SourceSystems, TargetSystems, DataDomain, ProcessOwner tags)

v1.0 (Nov 2025): Initial policy

15. References & Resources

15.1 AWS Documentation

AWS Tagging Best Practices

AWS Organizations Tag Policies

AWS Config Rules for Tagging

15.2 Industry Standards

FinOps Foundation: Cloud Tagging Standards

AWS Well-Architected Framework: Cost Optimization Pillar

CIS AWS Foundations Benchmark

15.3 Internal Resources

ECMX AWS Restructure Roadmap (Month 1-9 plan)

Cost Center Directory (Finance)

Project Registry (Product Management) - see Section 16

Data Classification Policy (Security)

16. Project Registry

16.1 Purpose

The Project Registry is the authoritative source mapping customer/project names to their AWS resources, ownership, and metadata. This registry ensures:

Consistent project naming across all resources

Clear customer-to-infrastructure mapping

Contact information for escalations

Cost allocation accuracy

16.2 Registry Location

Primary: Internal wiki or shared document (e.g., Confluence, Notion)   Backup: Git repository with version control   Access: Read-only for all engineers, write access for Platform team

16.3 Required Fields

Field

Description

Example

Project Code

Short identifier used in resource names

shopify, franklin, sjce

Customer Name

Full legal customer name

Shopify Inc., Franklin Templeton, San Jose Clean Energy

Project Type

Category of engagement

E-commerce, Analytics, FinTech, Utility

Status

Current state

Active, Onboarding, Deprecated, Archived

Start Date

When project began

2023-06-15

Primary Owner

Main technical contact

ecommerce-team@ecmx.com

Account Lead

Business owner/PM

sarah.jones@ecmx.com

AWS Accounts

List of AWS account IDs

123456789012, 234567890123

Cost Center

Billing code

CLIENT-SERVICES

Environments

Active environments

dev, stage, prod

Compliance Reqs

Any regulatory needs

PCI, SOC2

Confidentiality

Can name be public in tags?

Public, Internal-Only, Confidential

16.4 Example Entries
wide1800
16.5 Maintenance Process

New Project: Account lead creates registry entry before any AWS resources

Updates: Platform team updates when accounts/environments change

Offboarding: Status changed to Deprecated, then Archived after full decommission

Audit: Quarterly review by Platform + Finance to verify accuracy

Access Control: Only Platform team can modify; all engineers have read access

16.6 Integration with Tagging

Automated Validation: Tag compliance scripts check Project tag against registry

Cost Reports: Finance pulls customer names from registry for billing reports

Ownership Lookup: Incident response uses registry to find escalation contacts

Compliance Audits: Security team cross-references tags with compliance requirements

17. Customer Confidentiality Guidelines

17.1 When to Use Customer Names in Tags

Public in Tags (Safe to Use):

Customer has given explicit permission

Customer name is already public knowledge (e.g., public sector, known partnerships)

No NDA or confidentiality clause prevents disclosure

Examples: SJCE (public utility), publicly announced partnerships

Internal-Only (Use with Caution):

Customer relationship is not public

Use customer name in tags but understand AWS console is visible to team

Limit AWS account access to need-to-know basis

Examples: Most customer projects, use actual customer names but control AWS access

Confidential (Use Project Codes):

Strict NDA in place

High-profile customer requiring anonymity

Pre-announcement/stealth projects

Use internal project codes (e.g., proj-001, alpha, phoenix) and maintain mapping in secure registry

Document real customer name only in secure, access-controlled Project Registry

17.2 Best Practices

Default: Use recognizable customer names/abbreviations in tags unless confidentiality is explicitly required

AWS Console Access: Control who has access to AWS accounts via SSO groups

External Docs: When sharing architecture diagrams or docs externally, replace customer names with generic labels

Cost Reports: Finance team maintains confidential version of reports with full customer names

Incident Response: Ensure on-call engineers have access to Project Registry to map codes to customers

17.3 Example Scenarios

Scenario 1: Public Sector Customer

Customer: San Jose Clean Energy (public utility)

Confidentiality: Public

Tagging: Project=SJCE ✓

Reasoning: Public sector, publicly known relationship

Scenario 2: Enterprise SaaS Customer

Customer: Large tech company with NDA

Confidentiality: Internal-Only

Tagging: Project=Shopify (recognizable to internal team) ✓

Reasoning: Not public but internal team needs to recognize it

Scenario 3: Stealth Startup

Customer: Pre-launch startup with strict confidentiality

Confidentiality: Confidential

Tagging: Project=Phoenix (code name) ✓

Registry Mapping: Phoenix → StartupX (Confidential)

Reasoning: Customer requires anonymity even within ECMX

Appendix A: Migration Checklist

For Each Resource Migration:

Current resource inventory exported with existing tags

Naming convention applied to target resources

All 5 mandatory tags applied

Service-specific conditional tags applied

DataClassification validated (for data resources)

Data lineage tags applied (SourceSystems, TargetSystems, DataDomain, ProcessOwner) for data resources

Tags verified in target account

Cost allocation tags activated in billing

Resource Groups created for project

Old resource tagged with MigrationStatus=Complete

Documentation updated with new resource names/tags

Data flow documentation updated with lineage information

Appendix B: Tag Value Examples by Project Type

Customer Project - E-commerce (Shopify)
wide1800
Customer Project - Utility Analytics (SJCE)
wide1800
Customer Project - Financial Services (Franklin)
wide1800
Internal Tools / Shared Services
wide1800
Sandbox / Experimentation
wide1800
END OF POLICY

For questions or clarifications, contact Conrad, Ben or Catalin in Slack