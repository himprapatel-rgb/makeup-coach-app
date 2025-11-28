# Infrastructure Research for Use-What-You-Have Makeup Coach

## Executive Summary

The Use-What-You-Have Makeup Coach requires robust cloud infrastructure to handle AI model inference, user data, video streaming, and real-time processing. This document outlines backend architecture, database selections, cloud deployment strategy, scalability considerations, and deployment pipeline.

Key infrastructure areas: cloud platform selection, API architecture, database design, AI model serving, video CDN, monitoring, and security infrastructure.

---

## 1. Cloud Platform Selection

### 1.1 Platform Comparison

| Provider | Cost | ML Support | Scalability | Global | Best For |
|----------|------|-----------|-------------|--------|----------|
| AWS | $$$ | Excellent (SageMaker) | Excellent | Yes | Enterprise, complex ML |
| Google Cloud | $$$ | Excellent (Vertex AI) | Excellent | Yes | ML/Data, BigQuery |
| Microsoft Azure | $$$ | Good (Azure ML) | Excellent | Yes | Enterprise, Microsoft stack |
| Firebase | $ | Limited | Good | Yes | Startups, rapid development |

### 1.2 Recommended: AWS for Production

**Reasons**:
- Mature SageMaker for ML model hosting
- Excellent global CDN (CloudFront)
- Strong security and compliance (HIPAA, GDPR)
- Large developer community
- Cost-effective scaling

**Alternative for MVP**: Firebase/Google Cloud for rapid development

---

## 2. Backend Architecture

### 2.1 Microservices Architecture

```
                        [API Gateway]
                             |
         ┌───────────────────┼───────────────────┐
         |                   |                   |
    [Auth Service]    [Makeup Service]   [User Service]
         |                   |                   |
    [JWT/OAuth]      [AI Model Inference] [Profile DB]
                      [Makeup DB]
```

### 2.2 Core Services

| Service | Technology | Purpose |
|---------|-----------|---------|
| API Gateway | AWS API Gateway | Route requests, rate limiting, authentication |
| Auth Service | JWT + AWS Cognito | User authentication, authorization |
| Makeup Recommendation | Python FastAPI | Core AI inference engine |
| User Service | Node.js/Python | User profiles, preferences, saved looks |
| Admin Service | React + Python | Content management, analytics |

### 2.3 Deployment Units

**Container-based (Recommended)**:
- Docker containers for each microservice
- AWS ECS or EKS for orchestration
- Auto-scaling based on CPU/memory

**Advantages**:
- Easy to scale horizontally
- Environment consistency (dev to prod)
- Simple CI/CD integration

---

## 3. Database Architecture

### 3.1 Database Selection

| Data Type | Database | Rationale |
|-----------|----------|-----------|
| User profiles | PostgreSQL | Relational, ACID compliance |
| Makeup looks (saved) | MongoDB | Flexible schema, fast queries |
| Analytics | BigQuery | Large-scale analysis, SQL |
| Cache | Redis | Fast access, session storage |
| Biometric templates | Encrypted PostgreSQL | High security, relational |

### 3.2 PostgreSQL Schema (Primary)

**Tables**:
- `users`: id, email, encrypted_password, created_at
- `user_profiles`: user_id, skin_tone, face_shape, preferences
- `saved_looks`: id, user_id, makeup_products, lighting, date_saved
- `makeup_products`: id, brand, name, shade, category
- `feedback`: id, user_id, look_id, rating, comment

### 3.3 Scaling Strategy

**Read Replicas**:
- 1 primary (write), 3 read replicas
- Automatic failover

**Sharding** (if > 10M users):
- Shard by user_id
- 4-8 shards initially

**Backup**:
- Continuous replication (AWS RDS)
- Daily snapshots to S3
- 30-day retention

---

## 4. AI Model Infrastructure

### 4.1 Model Hosting

**AWS SageMaker Endpoint**:
- Deployment: 2-4 GPU instances (ml.p3.2xlarge)
- Auto-scaling: 2-10 instances based on load
- Request timeout: 30 seconds max
- Inference latency target: < 2 seconds

**Load Testing Numbers**:
- Peak concurrent users: 100,000+
- Requests per second: 1,000+
- Average model inference: 0.5-1 second

### 4.2 Model Updates

**Retraining Schedule**:
- Monthly: Retrain with new user feedback
- Quarterly: Major updates with new data
- A/B testing: 10% traffic to new model

**Deployment**:
- Blue-green deployment (zero downtime)
- Canary releases (5% → 25% → 100%)
- Automatic rollback if error rate > 1%

### 4.3 Model Versioning

```
/models/
  └── makeup-recommendation/
       ├── v1.0/
       │   ├── model.pkl
       │   └── metadata.json
       └── v2.0/
           ├── model.pkl
           └── metadata.json
```

---

## 5. API Design

### 5.1 Core Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/v1/auth/signup` | POST | User registration |
| `/api/v1/auth/login` | POST | User login |
| `/api/v1/makeup/recommend` | POST | Get makeup recommendations |
| `/api/v1/looks/save` | POST | Save makeup look |
| `/api/v1/looks/{id}` | GET | Retrieve saved look |
| `/api/v1/products/search` | GET | Search makeup products |

### 5.2 Request/Response Format

**Request**:
```json
{
  "image": "base64_encoded_image",
  "skin_tone": "medium",
  "preferences": ["minimal", "natural"],
  "available_products": ["MAC", "NYX"]
}
```

**Response**:
```json
{
  "recommendations": [
    {
      "look_name": "Everyday Natural",
      "products": [...],
      "steps": [...],
      "confidence": 0.95
    }
  ],
  "processing_time_ms": 450
}
```

### 5.3 Rate Limiting

- Free tier: 50 requests/day
- Pro tier: 500 requests/day
- Enterprise: Unlimited

---

## 6. Security Infrastructure

### 6.1 Data Protection

**Encryption**:
- In transit: TLS 1.3 for all APIs
- At rest: AES-256 for sensitive data
- Database: AWS RDS encryption

**Authentication**:
- JWT tokens (2-hour expiry)
- AWS Cognito for OAuth/SAML
- MFA for admin accounts

### 6.2 Network Security

- VPC with private subnets for databases
- Security groups limiting access
- WAF (Web Application Firewall) for DDoS
- Regular penetration testing

### 6.3 Compliance

- GDPR: Data encryption, deletion rights
- CCPA: User data access & export
- HIPAA: Not required (cosmetics app), but best practices applied
- SOC 2 Type II compliance target (Year 2)

---

## 7. Monitoring & Logging

### 7.1 Key Metrics

| Metric | Target | Alert Threshold |
|--------|--------|-----------------|
| API latency (p95) | < 500ms | > 1000ms |
| Error rate | < 0.1% | > 0.5% |
| Model inference time | < 1s | > 2s |
| GPU utilization | 60-80% | > 90% or < 20% |
| Database connection pool | 80 | > 100 |

### 7.2 Logging Stack

- **CloudWatch**: Application logs, error tracking
- **Datadog**: APM, infrastructure monitoring
- **ELK Stack**: Centralized log analysis (alternative)
- **Sentry**: Error tracking and alerting

### 7.3 Dashboards

- Real-time performance dashboard
- AI model health dashboard
- User analytics dashboard
- Infrastructure cost dashboard

---

## 8. Scalability Strategy

### 8.1 Horizontal Scaling

**API Servers**:
- Min: 2 instances
- Max: 50 instances
- Target: 70% CPU utilization
- Scaling action: +3 instances when threshold exceeded

**Database**:
- Auto-scaling storage (10GB → 1TB range)
- Read replicas added based on query latency
- Automatic backup scaling

### 8.2 Load Balancing

- AWS ALB (Application Load Balancer)
- Sticky sessions for user continuity
- Health checks every 15 seconds
- Graceful shutdown timeout: 30 seconds

### 8.3 Caching Strategy

- **CDN**: CloudFront for static assets (tutorials, images)
- **API cache**: Redis (1-hour TTL)
- **Database**: Query result caching (5-minute TTL)

---

## 9. Deployment Pipeline

### 9.1 CI/CD Workflow

```
Code Push → GitHub
    ↓
GitHub Actions Tests (Unit, Integration)
    ↓
Build Docker Images
    ↓
Push to ECR
    ↓
Deploy to Staging
    ↓
Smoke Tests
    ↓
Manual Approval
    ↓
Deploy to Production
    ↓
Health Checks
```

### 9.2 Release Process

**Testing**:
- Unit tests: >80% coverage required
- Integration tests: All services tested
- Staging deployment: 24 hours before production
- Smoke tests: 30-minute validation period

**Deployment**:
- Blue-green deployment (zero downtime)
- Canary release: 5% traffic for 1 hour
- Gradual rollout: 25%, 50%, 100%
- Automatic rollback if error rate spikes

### 9.3 Deployment Frequency

- MVP: Daily deployments
- Growth: 2-3 times/week
- Scale: Continuous deployment (multiple per day)

---

## 10. Cost Optimization

### 10.1 Infrastructure Costs (Monthly Estimate)

| Component | Cost | Notes |
|-----------|------|-------|
| API servers (ECS) | $500-1,000 | 5-20 instances |
| AI model (SageMaker) | $2,000-5,000 | GPU instances |
| Database (RDS) | $500-1,000 | Multi-AZ, 1TB storage |
| CDN (CloudFront) | $200-500 | Video delivery |
| Data transfer | $200-500 | Inter-region transfer |
| **Total** | **$3,400-8,000** | **Scales with users** |

### 10.2 Cost Reduction Strategies

1. **Reserved Instances**: 30% discount for 1-3 year commitment
2. **Spot Instances**: 70% discount for non-critical workloads
3. **Auto-scaling**: Right-size instances based on demand
4. **Regional optimization**: Place data closer to users
5. **Database optimization**: Index optimization, query caching

---

## 11. Disaster Recovery

### 11.1 RTO & RPO Targets

| Metric | Target |
|--------|--------|
| RTO (Recovery Time Objective) | 1 hour |
| RPO (Recovery Point Objective) | 15 minutes |
| Backup frequency | Hourly incremental |
| Retention | 30 days |

### 11.2 Backup Strategy

- Continuous replication to secondary region
- Daily snapshots to S3 (multi-region)
- Weekly full backups to Glacier
- Monthly archive retention

### 11.3 Failover Plan

1. Automatic detection of primary region failure
2. DNS cutover to secondary region (< 5 minutes)
3. Read replicas promoted to primary
4. Manual validation before full restoration

---

## 12. Infrastructure as Code

### 12.1 Terraform/CloudFormation

```
infrastructure/
├── main.tf
├── variables.tf
├── vpc.tf
├── rds.tf
├── sagemaker.tf
├── networking.tf
└── monitoring.tf
```

**Benefits**:
- Version-controlled infrastructure
- Reproducible deployments
- Environment parity (dev/staging/prod)
- Disaster recovery testing

---

## 13. Roadmap

### Phase 1: MVP (Months 1-3)
- [ ] Single AWS region (us-east-1)
- [ ] Basic RDS setup
- [ ] SageMaker endpoint
- [ ] CloudFront for CDN
- [ ] CloudWatch monitoring

### Phase 2: Scale (Months 4-6)
- [ ] Multi-region deployment
- [ ] Database read replicas
- [ ] ElastiCache (Redis)
- [ ] Enhanced monitoring (Datadog)
- [ ] Load testing to 10,000 concurrent users

### Phase 3: Global (Months 7-12)
- [ ] 3+ global regions
- [ ] Edge computing (Lambda@Edge)
- [ ] Advanced caching strategies
- [ ] Cost optimization (Reserved Instances)
- [ ] Load testing to 100,000+ concurrent users

---

## 14. References

- AWS Well-Architected Framework
- Cloud Native Computing Foundation
- The Twelve-Factor App methodology
- Kubernetes best practices

---

**Document Version**: 1.0
**Last Updated**: 2025
**Status**: Ready for infrastructure provisioning
