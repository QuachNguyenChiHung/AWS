---
title: "Workshop"
date: 2025-12-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Building Furious Five Fashion: AWS Full-Stack Infrastructure Workshop

#### Overview

The system architecture is built on a full-stack serverless model on AWS, focusing on automatic scalability, multi-layer security and cost optimization. All frontend – backend – data – AI – security components operate in a private environment, connected through VPC, PrivateLink and AWS management services

You will deploy seven CDK stacks linked together to create a scalable, secure and cost-optimized application:

Frontend Layer – Deploy the interface on Amplify and distribute content via CloudFront.

* Routing & Protection – Protect access with Route 53, WAF and ACM SSL certificates.

* Authentication Layer – Create a Cognito User Pool and integrate authentication for API Gateway.

* API Layer – Build a private API Gateway to securely communicate with the backend.

* Compute Layer – Run business logic using Lambda functions inside a private VPC.

* Storage Layer – Store static data and uploads on S3 via VPC Endpoint.

* Data Layer – Run RDS in a private subnet and control access using IAM/SG.

* AI Layer – Integrate Amazon Bedrock to handle AI tasks via PrivateLink.

* Security & Observability – Monitor the entire system using CloudWatch, send alerts via SNS and manage security using IAM.

#### Content

1. [Workshop Overview](5.1-workshop-overview)
2. [Setup Environment](5.2-setup-environment/)
3. [CDK Bootstrap](5.3-cdk-bootstrap/)
4. [Configure Infrastructure Stacks](5.4-configure-stacks/)
5. [Configure API & Lambda](5.6-configure-api-lambda/)
6. [Deploy Backend Services](5.7-deploy-backend/)
7. [Test Endpoints End-to-End](5.8-test-endpoints/)
8. [Push to GitLab](5.9-push-gitlab/)
9. [Clean up](5.11-cleanup/)
