---
title: "Workshop Overview"
date: 2025-12-01
weight: 1
chapter: false
pre: " <b> 5.01. </b> "
---

#### OJT E-commerce Architecture

**OJT E-commerce** is a modern serverless e-commerce platform built entirely on AWS. The architecture follows best practices for scalability, security, and cost optimization, replacing traditional Spring Boot backend with AWS Lambda functions.

#### Key Components

+ **Frontend**: React + Vite application hosted on S3 with CloudFront CDN
+ **Backend**: Serverless API using API Gateway with 11 Lambda modules (63 endpoints)
+ **Database**: RDS SQL Server Express 2019 in private subnet
+ **Storage**: S3 buckets for images and frontend static files
+ **Authentication**: JWT-based authentication with Cognito User Pool (optional)
+ **Security**: VPC with public/private subnets, Security Groups, Secrets Manager
+ **Monitoring**: CloudWatch Dashboard, Log Groups, and Alarms

#### Architecture Diagram

<img src="/images/2-Proposal/OJT-SS5.drawio.png"/>

#### Workshop Flow

This workshop follows a practical application development workflow:

1. **Setup Environment** - Install tools (Node.js, AWS CLI, CDK CLI)
2. **CDK Bootstrap** - Prepare AWS account for CDK deployments
3. **Deploy Core Infrastructure** - VPC, RDS, S3, Cognito (NetworkStack, StorageStack, AuthStack, DatabaseStack)
4. **Deploy API Stack** - API Gateway + Placeholder Lambda functions
5. **Deploy Lambda Code** - Deploy actual Lambda function code (63 APIs)
6. **Deploy Frontend** - Build React app and deploy to S3 + CloudFront
7. **Deploy Monitoring** - CloudWatch Dashboard and Alarms
8. **Test Endpoints** - Verify all 63 API endpoints work end-to-end
9. **Monitor & Maintain** - Use CloudWatch for monitoring and debugging

#### What You'll Learn

+ Infrastructure as Code with AWS CDK (TypeScript)
+ Serverless architecture replacing Spring Boot with Lambda
+ 2-step deployment strategy: Infrastructure + Lambda code separation
+ RDS SQL Server in private subnet with Secrets Manager
+ CloudFront CDN with Origin Access Control (OAC)
+ JWT authentication with optional Cognito integration
+ Lambda function modular organization (11 modules: Auth, Products, ProductDetails, Cart, Orders, Categories, Brands, Banners, Ratings, Users, Images)
+ VPC design with NAT Gateway for private subnet internet access
+ CloudWatch monitoring with Dashboard and Alarms
+ Cost optimization strategies for development environment

#### Realistic Cost Estimate

**Development Environment (Optimized):**
- **Estimated Monthly Cost**: **$44/month** (60% reduction from original $111/month)

**Cost Breakdown:**

| Service | Configuration | Monthly Cost |
|---------|--------------|--------------|
| NAT Gateway | 1 instance | $23 |
| RDS SQL Server | t3.micro | $15 |
| Lambda | 11 modules, 128MB | $2 |
| S3 Storage | Images + Frontend | $1.25 |
| CloudFront | CDN distribution | $1.50 |
| CloudWatch | Dashboard + Logs | $1.50 |
| **Total** | | **~$44/month** |

**Cost Optimization Applied:**
+ RDS instance size: t3.small → t3.micro (saves $39/month)
+ NAT Gateway: 2 → 1 instance (saves $23/month)
+ Lambda memory: 256MB → 128MB (saves 50% per invocation)
+ Lambda timeout: 30s → 10s (faster execution)
+ Log retention: 7 days → 1 day (saves 85% CloudWatch cost)
+ Backup retention: 7 days → 1 day for development

**Free Tier Benefits (First 12 months):**
+ Lambda: 1M requests/month free
+ S3: 5GB storage + 20K GET requests free
+ RDS: t3.micro 750 hours/month free (single-AZ)

#### Key Features

+ **E-commerce Platform**: Products, Cart, Orders, Categories, Brands
+ **63 API Endpoints**: Complete CRUD operations for all modules
+ **Image Upload**: S3 integration for product images
+ **Search & Filter**: Products by category, brand, price range
+ **Order Management**: Create, track, and manage orders
+ **Rating System**: Product ratings and statistics
+ **Banner Management**: Dynamic banners for promotions
+ **Admin Functions**: User management, order status updates

#### Lambda Modules Summary

| Module | Functions | Description |
|--------|-----------|-------------|
| Auth | 4 | Login, Signup, Logout, Me |
| Products | 12 | CRUD, Search, Filter, Best-selling, Newest |
| ProductDetails | 7 | CRUD, Images upload |
| Cart | 6 | Add, Get, Update, Remove, Clear, Count |
| Orders | 9 | CRUD, COD, Status, Date-range filter |
| Categories | 6 | CRUD, Search |
| Brands | 5 | CRUD |
| Banners | 7 | CRUD, Toggle |
| Ratings | 3 | Get, Stats, Create |
| Users | 3 | GetAll, GetById, UpdateProfile |
| Images | 1 | Upload to S3 |
| **Total** | **63** | |

#### Deployment Strategy

**2-Step Deployment Process:**

1. **Deploy Infrastructure (CDK)** - 5-10 minutes
   - VPC, Subnets, NAT Gateway
   - RDS SQL Server + Secrets Manager
   - S3 Buckets (Images, Frontend)
   - API Gateway + Placeholder Lambda
   - Cognito User Pool (optional)

2. **Deploy Lambda Code** - 1-2 minutes
   - Package Lambda functions with dependencies
   - Upload to AWS Lambda
   - Update function code independently

**Benefits of Separation:**
+ CDK deploy faster (no Lambda code build)
+ No dependency resolution errors
+ Update Lambda code independently (30 seconds)
+ Clear separation: Infrastructure vs Application code
+ CI/CD friendly deployment

