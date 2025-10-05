---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, you need to summarize the contents of the workshop that you **plan** to conduct.

# Fashion Selling: FFF

### 1. Executive Summary  
The proposed solution is a web platform for fashion sales developed on AWS, utilizing serverless technology and AI to manage a secure, stable, cost-optimized, and easily scalable e-commerce system.

### 2. Problem Statement  
*Current Problem*  
Small businesses or startups often lack the budget to build complex infrastructure.  
Operating traditional servers is expensive (hardware, maintenance, operations personnel).  
When customer numbers surge, traditional systems are prone to overload, causing service disruptions.  
Lack of AI for product recommendations, consulting chatbots, or data systems to support decision-making.  
Difficult to track customer behavior, optimize sales, and marketing.

*Solution*  
Serverless architecture → automatic scaling, cost optimization, no need to manage complex infrastructure.  
Ensure safety, stability, high availability through secure and monitored AWS services.  
Suitable for small and medium enterprises needing to test the market before large investments.  
Only $50–100/month, helping reduce financial risk.  
AI Chatbot supports customers 24/7, reduces consulting staff load, increases satisfaction, product recommendations help increase conversion rates and order value.

### 3. Solution Architecture  

### AWS Services Used  
#### 1. Frontend (Web + CDN)
- **Amazon S3** → Store and host static web (Next.js build). Helps reduce costs, no need for traditional web servers.
- **Amazon CloudFront** → Global CDN content distribution, SSL support, reduce page load latency.
- **Amazon Route 53** → Manage domain, DNS, route traffic to CloudFront/S3.

#### 2. Backend & API
- **Amazon API Gateway** → API gateway receives requests from frontend, routes to Lambda/Fargate.
- **AWS Lambda** → Run serverless code (Node.js, Python...) for APIs without servers.
- **Amazon ECS Fargate** → Run container backend (if there are complex services, need longer runtime than Lambda).
- **Amazon ECR** → Store Docker images for ECS Fargate to pull and run.

#### 3. Database & Storage
- **Amazon RDS (PostgreSQL)** → Main relational database (store users, products, orders...).
- **Amazon DynamoDB** → NoSQL database to store sessions, metadata, light cache.
- **Amazon S3** → Store product images, invoices, analytics data (data lake).
- **Amazon ElastiCache (Redis)** → In-memory cache to speed up data reads, hold temporary sessions.

#### 4. Search & Events
- **Amazon OpenSearch** → Search engine for products, log analytics.
- **Amazon SQS** → Queue to process orders or async tasks (reduce load on main API).
- **Amazon SNS** → Send notifications (email, SMS, push) when events occur.
- **Amazon EventBridge** → Event bus to connect services (e.g., trigger AI pipeline when new order).

#### 5. AI & Machine Learning
- **Amazon Personalize** → Create personalized product recommendations for each customer.
- **Amazon Lex** → AI chatbot to support customers 24/7.
- **Amazon Comprehend** → Analyze emotions and keywords in customer reviews.
- **Amazon Fraud Detector** → Detect fraudulent transactions.
- **Amazon Forecast** → Forecast inventory and sales demand (deploy in later phase).
- **Amazon SageMaker** → Train/deploy custom ML models if default AI solutions are insufficient.

#### 6. Data Lake & ETL
- **Amazon S3 (Data Lake)** → Store raw + processed data for analytics.
- **AWS Glue** → ETL jobs to clean and standardize data.
- **Amazon Athena** → Query data directly on S3 with SQL (serverless).
- **AWS Lake Formation** → Manage data access permissions in the data lake.
- **Amazon Kinesis** → Collect real-time events, clickstreams from users.

#### 7. Authentication & Security
- **Amazon Cognito** → Registration, login, user pool management (SSO, OAuth2).
- **AWS IAM** → Manage roles and access permissions between services.
- **AWS KMS** → Encrypt data in RDS, S3, DynamoDB.
- **AWS Secrets Manager** → Store API keys, DB passwords, automatic secret rotation.
- **AWS WAF** → Filter bad requests, protect against OWASP Top 10 attacks.
- **AWS Shield** → Protect against DDoS attacks.

#### 8. Observability & Monitoring
- **Amazon CloudWatch** → Collect logs, metrics, set up alerts.
- **AWS X-Ray** → Trace requests end-to-end, find bottlenecks.
- **Amazon Managed Grafana** → Visualize metrics, logs dashboards.
- **OpenSearch Dashboards** → Interface to search & analyze logs from OpenSearch.
- **Amazon SNS** → Send alerts on errors or threshold exceedances.

#### 9. CI/CD & IaC
- **Terraform / AWS CDK** → IaC (Infrastructure as Code) for automatic infrastructure deployment.
- **GitHub Actions** → CI/CD pipeline: lint → test → build → deploy to AWS.
- **Amazon CodeArtifact** (optional) → Manage private package repository.

#### 10. Backup & DR
- **Amazon RDS Snapshot** → Automatic database backup.
- **Amazon S3 Versioning + Cross-Region Replication** → Backup files and data lake to another region (DR).

### 4. Technical Implementation  
*Implementation Phases*  

#### 1. Tech Lead / Architect

**Week 0–2**
- Design architecture: VPC, RDS, S3, Cognito, OpenSearch.
- Write architecture documentation + network diagrams.
- Review Terraform modules.

**Week 3–12**
- Code review, approve PRs.
- Ensure SLO: latency, error budget, cost.
- Test DR drill, review runbooks.

✅ **Deliverables:**  
- Diagram, architecture documentation  
- SLO/SLA  
- IAM model

---

#### 2. Frontend Engineer

**Week 1–2**
- Scaffold Vite project (JavaScript + Bootstrap).
- Integrate Cognito auth (login/signup).

**Week 3–6**
- Build UI: home, product list, product detail, cart, checkout.
- Upload images via S3 pre-signed URL.

**Week 7–9**
- Integrate chatbot widget (Lex).
- Integrate product recommendations from Personalize.

**Week 10–12**
- E2E tests (Playwright).
- Optimize SEO + Lighthouse (>= 90).
- Deploy build → CloudFront.

✅ **Deliverables:**  
- Web source code  
- Complete UI  
- Test report  
- Production build

---

#### 3. Backend Engineer

**Week 1–3**
- Scaffold NestJS API, define OpenAPI spec.
- Design DB schema (Postgres, Prisma migration).

**Week 4–6**
- API: products, cart, orders.
- Payment integration (stub).
- Upload media (S3).

**Week 7–9**
- Lambda consumer for SQS (order events).
- Recommendation API (Personalize).
- Fraud Detector scoring API.

**Week 10–12**
- Hardening: rate limit, validation.
- Integration tests (Jest + Supertest).
- Blue/green deployment support.

✅ **Deliverables:**  
- OpenAPI docs  
- Tested APIs  
- DB migrations  
- Docker image

---

#### 4. DevOps / SRE

**Week 0–2**
- Repo `/infra` + Terraform skeleton.
- Setup GitHub Actions (lint → test → plan → apply).
- State backend: S3 + DynamoDB lock.

**Week 3–6**
- Provision VPC, RDS, S3, Cognito, OpenSearch.
- Deploy staging environment.

**Week 7–9**
- Monitoring: Grafana dashboards.
- Alerts: SNS → Slack/email.
- Backup policies: RDS snapshots, S3 versioning.

**Week 10–12**
- DR drill (restore RDS, cross-region S3).
- Cost alerts, rightsizing.
- Prod cutover: Route53 switch.

✅ **Deliverables:**  
- Terraform repo  
- CI/CD pipelines  
- Monitoring dashboards  
- Runbooks

---

#### 5. Data / ML Engineer

**Week 1–3**
- Design event schema (click, view, purchase).
- Pipeline: Kinesis → S3 raw.
- Glue job ETL → Parquet.

**Week 4–6**
- Feed data into Personalize.
- Train + deploy campaign.

**Week 7–9**
- Chatbot Lex intents: FAQ, order tracking, product recommendations.
- Fulfillment Lambda: query RDS, call Personalize API.

**Week 10–12**
- Forecast model for inventory.
- Model Monitor (drift detection).
- Evaluation report.

✅ **Deliverables:**  
- ETL jobs + Parquet data  
- Personalize campaign + chatbot intents  
- Forecast model + monitoring  
- Evaluation report

### 5. Timeline & Milestones  
- *Pre-Internship (Month 0)*: 1 month for planning and evaluating old stations.  
- *Internship (Months 1–3)*:  
    - Month 1: Learn AWS and upgrade hardware.  
    - Month 2: Design and adjust architecture.  
    - Month 3: Implement, test, and launch.  

### 6. Budget Estimation  
You can find the budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  
Or download the [Budget Estimation File](../attachments/budget_estimation.pdf).  

*Infrastructure Costs*  
- AWS Lambda: $0.00/month (1,000 requests, 512 MB storage).  
- S3 Standard: $0.15/month (6 GB, 2,100 requests, 1 GB scanned).  
- Data Transfer: $0.02/month (1 GB inbound, 1 GB outbound).  
- AWS Amplify: $0.35/month (256 MB, 500 ms requests).  
- Amazon API Gateway: $0.01/month (2,000 requests).  
- AWS Glue ETL Jobs: $0.02/month (2 DPUs).  
- AWS Glue Crawlers: $0.07/month (1 crawler).  
- MQTT (IoT Core): $0.08/month (5 devices, 45,000 messages).  

*Total*: $0.7/month, $8.40/12 months  
- *Hardware*: $265 one-time (Raspberry Pi 5 and sensors).  

### 7. Risk Assessment  
*Risk Matrix*  
- Network Loss: Medium impact, medium probability.  
- Sensor Failure: High impact, low probability.  
- Budget Overrun: Medium impact, low probability.  

*Mitigation Strategies*  
- Network: Local storage on Raspberry Pi with Docker.  
- Sensors: Regular checks, spare parts.  
- Cost: AWS budget alerts, service optimization.  

*Contingency Plans*  
- Revert to manual collection if AWS fails.  
- Use CloudFormation to restore cost-related configurations.  

### 8. Expected Outcomes  
*Technical Improvements*: Real-time data and analytics replace manual processes. Scalable to 10–15 stations.  
*Long-term Value*: 1-year data foundation for AI research, reusable for future projects.