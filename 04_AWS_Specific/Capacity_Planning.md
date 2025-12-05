# AWS Capacity Planning Document

**Version:** 1.0  
**Date:** December 5, 2025  
**Owner:** EcoMetricx InfoSec Team (Andrei Ionete - andrei@ecometricx.com)  
**Review Cycle:** Quarterly

---

## 1. Current Resource Inventory

### 1.1 EC2 Instances (11 Total)

| Instance Type | Count | vCPU | Memory | Purpose | Utilization |
|---------------|-------|------|--------|---------|-------------|
| t2.micro | 1 | 1 | 1 GiB | Dev/Test | Low |
| t3.micro | 3 | 2 | 1 GiB | Dev/Test | Low |
| t3.small | 2 | 2 | 2 GiB | Application | Medium |
| t3a.xlarge | 1 | 4 | 16 GiB | Application | Medium |
| r6i.xlarge | 1 | 4 | 32 GiB | Memory-intensive | High |
| r6a.4xlarge | 1 | 16 | 128 GiB | ML/Analytics | High |
| r6g.4xlarge | 2 | 16 | 128 GiB | ML/Analytics | High |

**Alerts:**
- ⚠️ r6a.4xlarge (i-0e5e87039a350664d): Disk space alarm ACTIVE

### 1.2 RDS Databases (4 Total)

| Database | Engine | Instance Class | Storage | Multi-AZ | Backup Retention |
|----------|--------|----------------|---------|----------|------------------|
| database-neurova | MySQL | [TBD] | [TBD] | No | 1 day |
| ecmx-tenore | MySQL | [TBD] | [TBD] | No | 1 day |
| ecometricx-serverless-rds | Aurora MySQL | Serverless | Auto | No | 7 days |
| vox-instance-1 | Aurora MySQL | [TBD] | [TBD] | No | 1 day |

**Recommendations:**
- Enable Multi-AZ for production databases
- Increase backup retention to 7+ days for all
- Monitor Aurora serverless scaling

### 1.3 S3 Storage (54 Buckets)

| Category | Bucket Count | Estimated Size | Growth Rate |
|----------|--------------|----------------|-------------|
| Data Lake (Raw) | ~15 | [TBD] | [TBD] |
| Data Lake (Curated) | ~10 | [TBD] | [TBD] |
| Application | ~15 | [TBD] | [TBD] |
| Amplify/CloudFront | ~8 | [TBD] | Low |
| Logs/Audit | ~6 | [TBD] | Steady |

**Recommendations:**
- Implement lifecycle policies for cost optimization
- Move infrequently accessed data to S3 Glacier
- Enable intelligent tiering for variable access patterns

---

## 2. Capacity Thresholds

### 2.1 Alerting Thresholds

| Resource | Warning | Critical | Current Status |
|----------|---------|----------|----------------|
| EC2 CPU | 70% | 90% | ✅ Normal |
| EC2 Memory | 75% | 90% | ✅ Normal |
| EC2 Disk | 75% | 85% | ⚠️ Alert Active |
| RDS CPU | 70% | 85% | ✅ Normal |
| RDS Storage | 75% | 90% | ✅ Normal |
| RDS Connections | 80% max | 95% max | ✅ Normal |

### 2.2 Scaling Triggers

| Resource | Scale Up Trigger | Scale Down Trigger |
|----------|-----------------|-------------------|
| EC2 (Auto Scaling) | CPU > 70% for 5 min | CPU < 30% for 15 min |
| Aurora Serverless | Automatic | Automatic |
| Lambda | Concurrent > 80% | Automatic |

---

## 3. Growth Projections

### 3.1 12-Month Forecast

| Resource | Current | 6 Month | 12 Month | Growth Driver |
|----------|---------|---------|----------|---------------|
| EC2 Instances | 11 | 13-15 | 15-20 | New projects |
| RDS Storage | [TBD] GB | +20% | +40% | Data growth |
| S3 Storage | [TBD] TB | +30% | +60% | Data lake expansion |
| Data Transfer | [TBD] | +25% | +50% | API usage |

### 3.2 Capacity Planning Assumptions

- Customer growth: 20-30% annually
- Data volume: 40-50% increase per year
- Compute demand: Scales with data processing
- ML workloads: Increasing demand for GPU/high-memory

---

## 4. Cost Optimization

### 4.1 Current Recommendations

| Opportunity | Savings Estimate | Effort | Priority |
|-------------|------------------|--------|----------|
| Reserved Instances (EC2) | 30-40% | Medium | High |
| Reserved Capacity (RDS) | 30-40% | Medium | High |
| S3 Lifecycle Policies | 20-30% storage | Low | Medium |
| Right-sizing underutilized | 10-20% | Medium | Medium |
| Spot instances (dev/test) | 60-70% | Low | Low |

### 4.2 Instance Right-Sizing Review

| Instance | Current Type | Recommendation | Justification |
|----------|--------------|----------------|---------------|
| t2.micro | t2.micro | Upgrade to t3.micro | Better performance/$ |
| Stopped instances | Various | Evaluate termination | Cost reduction |
| r6a.4xlarge | r6a.4xlarge | Review disk usage | Alarm active |

---

## 5. High Availability Considerations

### 5.1 Current State

| Component | HA Status | Recommendation |
|-----------|-----------|----------------|
| EC2 (Prod) | Single-AZ | Consider Multi-AZ ALB |
| RDS | Single-AZ | Enable Multi-AZ ⚠️ |
| S3 | Multi-AZ (built-in) | ✅ Adequate |
| Lambda | Multi-AZ (built-in) | ✅ Adequate |
| Aurora | Single-AZ | Enable Multi-AZ ⚠️ |

### 5.2 Recovery Metrics

| Component | Current RTO | Target RTO | Current RPO | Target RPO |
|-----------|-------------|------------|-------------|------------|
| RDS | 1-2 hours | 15 minutes | 5 minutes | 5 minutes |
| EC2 | 2-4 hours | 30 minutes | Last snapshot | 1 hour |
| S3 | Instant | Instant | Near-zero | Near-zero |

---

## 6. Performance Monitoring

### 6.1 Key Metrics Tracked

| Metric | Source | Alert Threshold |
|--------|--------|-----------------|
| CPU Utilization | CloudWatch | 70% warning, 90% critical |
| Memory Utilization | CloudWatch Agent | 75% warning, 90% critical |
| Disk Utilization | CloudWatch | 75% warning, 85% critical |
| Network I/O | CloudWatch | Baseline + 50% |
| Database Connections | CloudWatch | 80% max_connections |
| API Latency | CloudWatch | P95 > 1s |

### 6.2 Dashboard Links

- CloudWatch Dashboard: [Link to be added]
- Cost Explorer: AWS Console → Billing → Cost Explorer
- Trusted Advisor: AWS Console → Trusted Advisor

---

## 7. Capacity Planning Process

### 7.1 Review Cycle

| Activity | Frequency | Owner |
|----------|-----------|-------|
| Utilization review | Weekly | DevOps |
| Cost analysis | Monthly | Finance/DevOps |
| Capacity forecast | Quarterly | Infrastructure |
| Architecture review | Annually | InfoSec/Architecture |

### 7.2 Scaling Decision Matrix

| Utilization Level | Trend | Action |
|-------------------|-------|--------|
| <50% | Stable | Consider downsizing |
| 50-70% | Stable | No action |
| 50-70% | Increasing | Plan upgrade |
| 70-85% | Any | Schedule upgrade |
| >85% | Any | Immediate action |

---

## 8. Action Items

### Immediate (0-30 days)

- [ ] Investigate r6a.4xlarge disk space alarm
- [ ] Review and clean up stopped EC2 instances
- [ ] Implement S3 lifecycle policies

### Short-term (30-90 days)

- [ ] Enable Multi-AZ for production RDS instances
- [ ] Evaluate Reserved Instance purchases
- [ ] Implement enhanced monitoring

### Long-term (90+ days)

- [ ] Develop auto-scaling strategies
- [ ] Plan for 12-month growth
- [ ] Evaluate cost optimization opportunities

---

## 9. Budget Considerations

### 9.1 Current Monthly Estimate

| Service | Estimated Cost | % of Total |
|---------|---------------|------------|
| EC2 | $[TBD] | [TBD]% |
| RDS | $[TBD] | [TBD]% |
| S3 | $[TBD] | [TBD]% |
| Data Transfer | $[TBD] | [TBD]% |
| Other | $[TBD] | [TBD]% |
| **Total** | **$[TBD]** | 100% |

### 9.2 Budget Planning

- Reserved Instance commitment: $[TBD]/month savings
- Growth buffer: 20% headroom recommended
- Emergency capacity: Maintain ability to scale 2x short-term

---

**Document Control:** Version 1.0 | December 2025 | Next Review: Q1 2026

