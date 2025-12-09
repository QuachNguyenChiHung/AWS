---
title: "Proposal"
date: 2025-10-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# AWS First Cloud AI Journey – Project Plan
## Online Shopping Website: Furious Five Fashion (FFF)
## AWS & AI-Powered E-commerce Website Solution
### 1. Background and Motivation
#### 1.1 Executive Summary

The client is a small-sized business specializing in fashion products for young customers. They aim to build an online clothing e-commerce website using AWS and AI, with the ability to scale flexibly, support long-term growth, and optimize operational costs.

The goal of this project is to shift from traditional manual management on physical servers to a flexible, intelligent, and cost-efficient cloud-based model. AWS enables the system to scale at any time, maintain fast access speed, and allow the business to focus on product development instead of infrastructure.

The system is designed to support end-to-end e-commerce operations: hosting and distributing web content, managing product and order databases, supporting payments, and monitoring system performance. Everything aims toward stability, security, and long-term scalability.

The Furious Five implementation team will accompany the client throughout the process—advising, designing the architecture, and configuring key AWS services such as Lambda, S3, DynamoDB, CloudFront, and Route 53. Beyond building the system, they also help optimize costs, ensure security, and train the internal team to manage the infrastructure effectively.

This project is not just a technical plan—it marks an important step in the company’s digital transformation journey.

#### 1.2 Project Success Criteria

To ensure the success of the Furious Five Fashion project, the following clear and measurable criteria must be met, representing both business goals and technical effectiveness:

**System Performance**
The website must maintain response times under 2 seconds for all user actions, even during peak hours.

**Availability**
The system must achieve 99.9% uptime, monitored and automatically reported through services like CloudWatch.

**Scalability**
AWS infrastructure must scale automatically when traffic increases by at least 2× without causing service disruption.

**Cost Optimization**
Monthly operating costs must remain under 30% of the projected budget, supported by AWS cost-monitoring tools such as Cost Explorer and Trusted Advisor.

**Security**
No data leaks or unauthorized access. All customer data must be protected by AWS security standards (IAM policies, encryption, HTTPS, etc.).

**Deployment & Operations**
Infrastructure must be fully deployed within 4 weeks, with complete documentation so the internal team can manage the environment effectively.

**Training & Knowledge Transfer**
The internal technical team must be trained to confidently maintain, monitor, and secure the system without depending entirely on external support.

#### 1.3 Assumptions

To ensure alignment and smooth execution of the FFF project, the following assumptions have been made:

The team already has access to AWS accounts with required permissions and has basic knowledge of essential AWS services such as Lambda, S3, IAM, and Route 53. Stable Internet connectivity is assumed since all infrastructure runs in the cloud. The team is also aware of basic security and compliance requirements before deployment.

The project depends on multiple external factors: stable service availability in the selected AWS region, smooth domain routing via Route 53, and effective collaboration between development teams to ensure the web application operates properly in the cloud environment.

The project is part of an internship, so the budget is limited—favoring free-tier usage and low-cost service configurations. Due to limited experience and tight timelines, the chosen architecture remains simple and practical.

Potential risks include IAM misconfigurations, accidental overspending due to unused resources, AWS regional outages, service incompatibilities, or limited expertise in troubleshooting cloud systems.

Despite these risks, the project is built on clear expectations: this is a pilot environment, with layered monitoring, backup, and cost-management strategies in place. Every challenge is considered an opportunity to learn and grow in cloud engineering.

### 2. SOLUTION ARCHITECTURE
#### 2.1 Technical Architecture Diagram

The following architecture is designed for FFF, deployed in AWS Region Singapore (ap-southeast-1). It emphasizes flexibility, security, automation, scalability, and simplicity—appropriate for an internship-level project while following AWS best practices.

The system follows a multi-layer design consisting of six key components:

Frontend & Security Layer
Users access the website through Route 53. Incoming traffic is protected with AWS WAF and optimized via CloudFront. Source code is managed and deployed through GitLab CI/CD using CloudFormation templates.

API & Compute Layer
API Gateway routes all requests to AWS Lambda, which handles application logic. Cognito manages authentication and access control.

Storage Layer
Two S3 buckets store static content (StaticData) and user uploads.

Data Layer
DynamoDB stores product metadata and unstructured data. IAM ensures secure interactions between components.

AI Layer
Amazon Rekognition and Amazon Bedrock power image processing and generative AI features.

Observability & Security Layer
CloudWatch, SNS, and SES provide monitoring, alerting, and system notifications.

<img src="/images/2-Proposal/OJT-SS5.drawio.png"/>

#### 2.2 Technical Implementation Plan

Infrastructure will be managed and deployed using Infrastructure as Code (IaC) with AWS CloudFormation to ensure repeatability, stability, and ease of maintenance.

Key AWS components—S3, Lambda, API Gateway, VPC, RDS , Cognito, and CloudWatch—will be defined entirely through CloudFormation templates stored in GitLab for version control and rollback capability.

Sensitive configurations such as IAM permissions or WAF rules require approval before deployment and follow the internal governance process with review and validation.

All critical system paths—from authentication to data processing—are covered by automated and manual test cases to ensure stability, security, and scalability.

This technical plan enables the FFF team to deploy and manage a professional cloud environment, learning real DevOps and AWS best practices.

#### 2.3 Project Plan

The project follows Agile Scrum over 3 months, divided into 4 sprints.

**Sprint Structure**

* Sprint Planning

  * Setup AWS foundational services (S3, Route 53, IAM)

  * Configure security (WAF, CloudFront)

  * Integrate backend (Lambda, API Gateway, RDS)

  * Testing, optimization, and demo preparation

* Daily Stand-up
30-minute updates to address blockers and track status.

* Sprint Review
Review deliverables, demo on real AWS environment, fix issues.

* Retrospective
Improve DevOps workflows and automation pipeline.

**Team Roles**

* Product Owner: Business alignment, backlog prioritization

* Scrum Master: Coordination, Agile process enforcement

* DevOps/Technical Team: Backend, infrastructure, CI/CD

* Mentor / AWS Partner: Architecture validation, AI testing, cost & security review

**Communication Rhythm**

* Daily Stand-ups (23:00)

* Weekly Sync

* End-of-Sprint Demo

**Knowledge Transfer**
After the final sprint, the technical team will deliver hands-on training on operations, monitoring (Budgets, CloudWatch), scaling, and recovery procedures.

#### 2.4 Security Considerations

Access Management <br>
MFA for admin users; IAM roles with least privilege; auditing through CloudTrail.

Infrastructure Security<br>
dedicated VPC, services are restricted using resource policies; all public endpoints use HTTPS.

Data Protection<br>
S3 and RDS encryption; TLS data transfer; manual periodic backups.

Detection & Monitoring<br>
CloudTrail, Config, and CloudWatch for visibility; GuardDuty for threat detection.

Incident Response<br>
Clear incident workflows with log collection, analysis, and periodic simulations.

### 3. PROJECT ACTIVITIES & DELIVERABLES
#### 3.1 Activities & Deliverables Table

|Phase  | Timeline  | Activities  |  Deliverables  | Effort(day)  |  
|---------------|:--------------------:|-----------------------------|------------------------|:---:|
| Infrastructure Setup  |Week <br> 1 – 2    | Requirements gathering, architecture design, AWS configuration (S3, CloudFront, API, Lambda, RDS, Cognito), GitLab CI/CD setup    | Completed AWS Architecture, Ready Infrastructure, Active CI/CD    |   10    |
|Frontend Development   |  Week 3–5  | UI/UX design, FE pages (Home, Catalog, Product Detail, Cart, Checkout), API integration | Completed FE (Dev), Frontend connected to API  |15 | 
| Backend & Database  | Week 6–9   | Lambda APIs, RDS setup, order/user/product logic, Cognito IAM setup | Stable API, validated data flow, full Frontend–Backend integration  |20 | 
| Testing & Validation  | Week <br> 10–11   | Functional, security, performance testing, integration testing | Test Report, Validated System  | 5| 
| Production Launch  | Week 12   |Deploy to production, domain & SSL setup, training & handover  | Live FFF Website, Documentation Package  | 5|

#### 3.2 Out of Scope

The following items were discussed during the requirements definition phase, but were determined to be out of scope for the FFF Web Clothing project at the current stage.

Items out of scope include:
* Mobile App development for the system (Android/iOS).
* Integration of real-world inventory, shipping and logistics management systems (Fast Delivery, GHN, Viettel Post, etc.).
* Advanced administrative functions such as multi-level authorization, automatic revenue reporting, advanced statistical charts.
* Integration of third-party CRM (Customer Relationship Management) or ERP (Enterprise Resource Planning).
* Use of AWS services with higher, more expensive automatic security features.
* Integration of real-world payment gateways (VNPay, Momo, ZaloPay, Stripe, PayPal, etc.)
* Multilingual and multi-currency
#### 3.3	PATH TO PRODUCTION
Phase 1 – Prototype (POC)

Activities:
Build a test version of FFF Web Sales with basic interface (Home, Category, Product Details, Cart).

* Connect backend via API Gateway – Lambda – DynamoDB.

* Deploy static website on Amazon S3 + CloudFront.
* Configure admin account and demo trial order process.


Phase 2 – Complete system and test (UAT)

Activities:
* Add user functions: login/register, authentication via AWS Cognito.
* Add trial payment feature via sandbox.
* Add monitoring with Amazon CloudWatch and error handling log.

* Perform internal user testing (User Acceptance Test).


Phase 3 - Official Operation Deployment (Production)

Activities:
* Move the entire system from the test environment to Production AWS.
* Configure Route53 for the official domain and SSL certificate via AWS Certificate Manager.
* Set up external security with AWS WAF.
* Optimize S3 capacity and CDN structure on CloudFront.



Phase 4 – Stabilization & optimization after deployment

Activities:
* Monitor actual AWS costs, optimize storage and logs.
* Adjust Lambda configuration to reduce cold start time.

* Perform periodic backups and test data recovery.
* Update operational documentation for the administration team.


Summary

The FFF Web Sales system has been successfully deployed on the AWS Serverless platform with a cost-optimized, highly secure and scalable architecture.
The stages were completed on schedule, ensuring that all functions were tested, refined and operated stably.
The project is now ready to expand real users and integrate more advanced e-commerce features.

### 4. AWS COST ESTIMATION

Estimated monthly cost: 

- Route 53 :         $1.00
- AWS WAF :          $5.00
- CloudFront:        $3.90
- Amplify: $10.00
- S3 (StaticData) :  $0.50
- S3 (Uploads):      $0.75
- S3 (Bucket):       $0.75
- AWS Lambda:        $0.25
- API Gateway:       $3.50
- Amazon Bedrock:    $3.00
- RDS:               $21.00
- IAM:               Free
- CloudWatch:        $2.00
- SNS:               $0.10
- SES:               $0.20
- CloudFormation:    Free
- GitLab CI/CD  :    $3.00
- WS Config / Setup & Test migration tools $5.00 (1 lần)
- Estimated monthly total cost: ~ $50.00 – $55.00 USD

KEY ASSUMPTIONS

Region: ap-southeast-1 (Singapore).
User access: 100–200/month.
The system is always running 24/7 but low load.
Mostly API via Lambda.
Small data (<30GB total).
CI/CD 1–2 deployments per week.
Free-tier is valid for the first 12 months.
AI is used for demo purposes, not large-scale inference.

SUGGESTED COST OPTIMISATION

Enable S3 Intelligent-Tiering to automatically move less frequently accessed data.
Limit CloudWatch Logs to 14–30 days.
Use AWS Budgets to alert if it exceeds $40/month.
If deploying long-term → consider Savings Plan for Lambda (30–40% reduction).

### 5. Team

#### **Partner Executive Sponsor** <br>

Name: Nguyen Gia Hung <br>
Title: FCJ Vietnam Training Program Director <br>
Description: Responsible for overall oversight of the FCJ internship program<br>
Email/contact information: hunggia@amazon.com|

#### **Project Stakeholders** <br>
Name: Van Hoang Kha <br>
Title: Support Teams <br>
Description: Responsible for overall supervision of the FCJ internship program as the Executive Support person.<br>
Email/Contact information: Khab9thd@gmail.com

#### **Partner Project Team** (Furious Five Internship Team)<br>
Name: Duong Minh Duc <br>
Title: Project Team Leader<br>
Description: Manage progress, coordinate work between the team and mentor, Manage AWS infrastructure deployment (S3, Lambda, IAM)<br>
Email/Contact information: ducdmse182938@fpt.edu.vn

Name: Quach Nguyen Chi Hung<br>
Title: Member<br>
Description: In charge of UI/UX and user interface<br>
Email/Contact information: bacon3632@gmail.com

Name: Nguyen Tan Xuan<br>
Title: Member<br>
Description: Responsible for Backend and server logic processing<br>
Email/Contact information: xuanntse184074@fpt.edu.vn<br>

Name: Nguyen Hai Dang<br>
Title: Member<br>
Description: Manage AWS infrastructure deployment (S3, Lambda, IAM) and AI chat bot integration<br>
Email/Contact information: dangnhse184292@fpt.edu.vn

Name: Pham Le Huy Hoang<br>
Title: Member<br>
Description: Testing, quality assurance and GitLab CI/CD integration, and AI chat bot integration<br>
Email/Contact information: hoangplhse182670@fpt.edu.vn

#### **Project Escalation Contacts**

Name: Duong Minh Duc<br>
Title: Project Team Leader<br>
Description: Represent the internship team to contact the mentor and sponsor directly<br>
Email/Contact information: ducdmse182938@fpt.edu.vn

### 6. RESOURCES & COST ESTIMATES
#### Resources

| Role | Responsibilities| Fee (USD)/Hour|
| ------------------ | ------------------------------------- | :------------------: |
|Solution Architect(1)|Design overall solution, ensure technical feasibility, select appropriate AWS service | 35|
|Cloud Engineer(2)|Deploy AWS infrastructure, configure services (S3, IAM...), test and optimize system| 20 |
|Project Manager (1)|Monitor progress, coordinate team, manage scope and risk for the project. |15 |
|Support / Documentation (1) |Prepare communication documents, user manuals and summary reports. | 10|

#### Estimate costs by project phase

| Project Phase | Solution Architect (hrs) | 2 Engineers (hrs)|Project Manager (hrs)|Project Management/Suppor(hrs)| Total Hours|
|-----------------------------------------------------|:---------:|:----------:|:-------------:|:--------------:|:-------:|
|Survey & Solution Design| 53 | 40 | 13 | 13 |119|
|Implementation & Testing|67 |160 | 21|19 |267 |
|Handover & Support | 27 |53 |21 | 19 | 120 |
|Total Hours|147 | 253| 55 |51 | 506|
|Total Amount |$5145 |$5060 |$825|$510 |$11540 |

#### Cost Contribution Allocation
| Party | Contribution (USD) | % Contribution |
|-------------------------------------------|:--------:|:--------:|
|Customer |4616 |40% |
|Partner (Furious Five) |2308 |20% |
| AWS |4616 | 40%|

### 7.	ACCEPTANCE

Since this project is currently at the presentation stage and has not yet been formally evaluated by a customer, the following acceptance process is proposed for future delivery phases:

#### 7.1 Acceptance Criteria (Proposed)
A deliverable will be considered acceptable when it meets the following criteria:
-	Functional features work as specified (authentication, recipe management, social features, AI functions).
-	All APIs respond correctly and integrate with AWS services (Lambda, API Gateway, RDS, S3).
-	Security requirements are met (JWT verification, HTTPS, access control, data encryption).
-	UI works as expected on supported devices.
-	No critical errors appear during test execution.
#### 7.2 ACCEPTANCE PROCESS
-	Review period: 8 business days for evaluation and testing.
-	If accepted → Deliverable is signed off.
-	If issues are found → A rejection notice will be issued with feedback.
-	Fixes will be applied and a revised version will be resubmitted for review.
-	If no response is received by the end of the review period → Deliverable is deemed accepted.
-	After completing each milestone, the team submits the deliverables and documentation. <br>
[Tải file .docx](/static/files/Proposal_FFF.docx)