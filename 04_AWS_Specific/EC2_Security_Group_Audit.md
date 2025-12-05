# EC2 Security Group Audit Report

**Audit Date:** December 5, 2025  
**Auditor:** EcoMetricx InfoSec Team

## Executive Summary

**Finding:** 20 security groups have inbound rules allowing traffic from 0.0.0.0/0 (any IP).

**Risk Level:** Medium - Requires review to ensure only necessary public access is allowed.

## Security Groups with 0.0.0.0/0 Rules

| Security Group ID | Name | Review Priority |
|-------------------|------|-----------------|
| sg-013ab182ec70beb22 | default | HIGH - Review immediately |
| sg-06be2db925be0878f | launch-wizard-1 | HIGH - Likely temporary |
| sg-07c61050341f16035 | launch-wizard-2 | HIGH - Likely temporary |
| sg-0474f41c318df08da | launch-wizard-3 | HIGH - Likely temporary |
| sg-02138f8f15bb0bbc7 | launch-wizard-4 | HIGH - Likely temporary |
| sg-08d1c602d5c3d8a08 | launch-wizard-5 | HIGH - Likely temporary |
| sg-06f920b5359c1f22c | SQL Database - IP Whitelist | MEDIUM - Verify whitelist |
| sg-0a40c23f7db506196 | ecx-dev_lb_SecurityGroup_dev | LOW - Load balancer |
| sg-0eec4217a1bbf2a03 | ecometricx_data_engineering_development | MEDIUM - Dev environment |
| sg-026986a97d1b622cb | Odoo-SG | MEDIUM - Application |
| sg-07f6e3299c6ca067e | neurova-backend-sg | MEDIUM - Backend |
| sg-06b26509e571eea7b | InfluxDB-SG | MEDIUM - Database |
| sg-07de0b30be4fd16e7 | vox_frontend-security-group | LOW - Frontend expected |
| sg-04e7c6b47d47f1115 | ecx-dev_EC2_Instance_SecurityGroup_dev | MEDIUM - Dev |
| sg-0bb738a7910d18b89 | ecx-load-balancer-security-group | LOW - Load balancer |
| sg-0b40d56ca0f2d52d0 | neurova-backend-us-east-1 | MEDIUM - Backend |
| sg-014e971efa78e78bb | Development Server Security Group Access | MEDIUM - Dev |
| sg-03fd07b4c13c7eb84 | community-service-security-group | MEDIUM - Service |
| sg-0c22a4a980e1b44fc | monthly-vm-sg | MEDIUM - VM |
| sg-010ee2e4d75b53a88 | ecx-ecs-tasks-security-group | LOW - ECS tasks |

## Recommendations

### Immediate Actions

1. **Review launch-wizard-* groups** - These are auto-created and often overly permissive
2. **Review default security group** - Should not allow 0.0.0.0/0

### Short-Term Actions

3. **Restrict database security groups** - SQL, InfluxDB should not be public
4. **Document legitimate public access** - Load balancers, frontends

### Best Practices

- Use specific IP ranges instead of 0.0.0.0/0 where possible
- Implement Security Group rules based on least privilege
- Regular quarterly reviews of security group rules

## Compliance Mapping

- **SOC 2 CC6.2:** Network access controls
- **ISO 27001 A.8.20-22:** Network security controls

---
**Next Audit:** Q1 2026

