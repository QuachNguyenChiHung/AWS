---
title: "Configure Infrastructure Stacks"
date: 2025-12-01
weight: 4
chapter: false
pre: " <b> 5.04. </b> "
---

# Configure Infrastructure Stacks

## OJT E-commerce Project Overview

### Introduction
OJT E-commerce is a serverless e-commerce platform built entirely on AWS Cloud. The project uses a serverless architecture with Lambda functions replacing traditional Spring Boot backend, leveraging AWS managed services to ensure scalability, security, and cost optimization.

### System Architecture

The project is designed with a **7-Stack Architecture** featuring clear layers:

```
┌─────────────────────────────────────────────────────────────┐
│                   OJT E-commerce Platform                    │
├─────────────────────────────────────────────────────────────┤
│  Layer 1: Network (Foundation)                              │
│  └─ Network Stack: VPC, Subnets, NAT Gateway, Security Groups│
├─────────────────────────────────────────────────────────────┤
│  Layer 2: Data & Storage                                    │
│  ├─ Storage Stack: S3 Buckets (Images, Logs)               │
│  └─ Database Stack: RDS SQL Server, Secrets Manager         │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: Authentication                                    │
│  └─ Auth Stack: Cognito User Pool, Identity Pool            │
├─────────────────────────────────────────────────────────────┤
│  Layer 4: Application & Business Logic                      │
│  └─ API Stack: API Gateway, 11 Lambda Modules     │
├─────────────────────────────────────────────────────────────┤
│  Layer 5: Content Delivery                                  │
│  └─ Frontend Stack: S3 Static Hosting, CloudFront CDN       │
├─────────────────────────────────────────────────────────────┤
│  Layer 6: Monitoring & Observability                        │
│  └─ Monitoring Stack: CloudWatch Dashboard, Alarms          │
└─────────────────────────────────────────────────────────────┘
```

### Technologies Used

#### Infrastructure as Code
- **AWS CDK (TypeScript)**: Infrastructure management with code
- **CloudFormation**: Underlying template engine for CDK
- **Git**: Version control for infrastructure code

#### Backend Services
- **API Gateway REST API**: RESTful API endpoint 
- **Lambda Functions**: Serverless compute for business logic 
- **RDS SQL Server**: Relational database 
- **S3**: Object storage for product images and frontend
- **Secrets Manager**: Secure credential storage

#### Security & Authentication
- **Cognito User Pool**: User authentication & management
- **JWT Authentication**: Custom JWT-based auth in Lambda
- **VPC**: Network isolation with public/private/isolated subnets
- **Security Groups**: Firewall rules for resources
- **IAM Roles & Policies**: Fine-grained access control

#### Content Delivery & Networking
- **CloudFront**: CDN with Origin Access Control (OAC)
- **NAT Gateway**: Internet access for private subnets
- **VPC Endpoints**: Private connectivity to AWS services

#### Monitoring & Operations
- **CloudWatch Logs**: Centralized logging for Lambda functions
- **CloudWatch Metrics**: Performance metrics tracking
- **CloudWatch Dashboard**: Visualization of metrics
- **CloudWatch Alarms**: Real-time monitoring alerts

### Project Directory Structure

```
OJT/
├── OJT_infrastructure/           # AWS CDK Infrastructure
│   ├── bin/
│   │   └── infrastructure.ts     # CDK app entry point
│   ├── lib/
│   │   └── stacks/              # Stack definitions
│   │       ├── network-stack.ts  # VPC, Subnets, NAT Gateway
│   │       ├── storage-stack.ts  # S3 Buckets
│   │       ├── auth-stack.ts     # Cognito User Pool
│   │       ├── database-stack.ts # RDS SQL Server
│   │       ├── api-stack.ts      # API Gateway + Lambda
│   │       ├── frontend-stack.ts # S3 + CloudFront
│   │       └── monitoring-stack.ts # CloudWatch
│   ├── .env.example             # Environment template
│   ├── package.json             # Node.js dependencies
│   └── tsconfig.json            # TypeScript configuration
│
├── OJT_lambda/                  # Lambda Functions (63 APIs)
│   ├── auth/                    # Authentication (4 functions)
│   ├── products/                # Products (12 functions)
│   ├── product-details/         # Product Details (7 functions)
│   ├── cart/                    # Cart (6 functions)
│   ├── orders/                  # Orders (9 functions)
│   ├── categories/              # Categories (6 functions)
│   ├── brands/                  # Brands (5 functions)
│   ├── banners/                 # Banners (7 functions)
│   ├── ratings/                 # Ratings (3 functions)
│   ├── users/                   # Users (3 functions)
│   ├── images/                  # Images (1 function)
│   └── shared/                  # Shared utilities
│
├── OJT_frontendDev/             # Frontend (React + Vite)
│   ├── src/                     # Source code
│   └── public/                  # Static assets
│
└── database/                    # Database Scripts
    ├── schema/                  # SQL schema files
    ├── migrations/              # Migration scripts
    └── seeds/                   # Sample data
```

---

## Stack Architecture Overview

### 1. Network Stack (Deploy Order: 1)
**Purpose**: Create VPC and network infrastructure

**Main resources**:
- **VPC**: 10.0.0.0/16 CIDR block
- **Public Subnets**: 2 AZs - Internet-facing resources
- **Private Subnets**: 2 AZs - Lambda functions, internal services
- **Isolated Subnets**: 2 AZs - RDS database (no internet)
- **NAT Gateway**: 1 instance (cost optimized)
- **Internet Gateway**: VPC internet access
- **Security Groups**: Lambda SG, RDS SG
- **VPC Endpoints**: S3, Secrets Manager

**Estimated cost**: ~$23/month (NAT Gateway)

---

### 2. Storage Stack (Deploy Order: 2)
**Purpose**: Create S3 buckets for images and logs

**Main resources**:
- **Images Bucket**: Product images storage
  - Versioning enabled
  - Lifecycle rules for cost optimization
- **Logs Bucket**: CloudFront access logs
  - Auto-delete after 90 days

**Estimated cost**: ~$1-3/month

---

### 3. Auth Stack (Deploy Order: 2)
**Purpose**: User authentication with Cognito (Optional)

**Main resources**:
- **Cognito User Pool**: User registration and authentication
  - Email verification required
  - Password policy: 8+ chars, mixed case, numbers, symbols
- **Cognito User Pool Client**: Frontend authentication
- **Cognito Identity Pool**: AWS credentials for authenticated users

**Estimated cost**: $0/month (Free tier: 50,000 MAU)

> **Note**: This stack is optional. The project also supports custom JWT authentication in Lambda.

---

### 4. Database Stack (Deploy Order: 2)
**Purpose**: SQL Server database with Secrets Manager

**Main resources**:
- **RDS SQL Server Express 2019**:
  - Instance: db.t3.micro (cost optimized)
  - Storage: 20 GB gp3 SSD
  - Multi-AZ: Disabled (dev/staging)
  - Backup: 1 day retention
- **Secrets Manager**: Database credentials
  - Auto-generated strong password
  - Optional rotation

**Estimated cost**: ~$15/month (optimized from $54)

---

### 5. API Stack (Deploy Order: 3)
**Purpose**: REST API with API Gateway and Lambda functions

**Main resources**:
- **API Gateway REST API**:
  - 63 endpoints across 11 modules
  - CORS enabled
  - CloudWatch logging
- **Lambda Functions** (11 modules):
  - Auth, Products, ProductDetails, Cart, Orders
  - Categories, Brands, Banners, Ratings, Users, Images
  - Runtime: Node.js 20.x
  - Memory: 128-512 MB (optimized)
  - VPC: Private subnets

**Estimated cost**: ~$2-5/month

---

### 6. Frontend Stack (Deploy Order: 4)
**Purpose**: Static website hosting with CDN

**Main resources**:
- **S3 Bucket**: React build files
- **CloudFront Distribution**:
  - Origin Access Control (OAC)
  - HTTPS only
  - Gzip compression

**Estimated cost**: ~$1-2/month

---

### 7. Monitoring Stack (Deploy Order: 5)
**Purpose**: Monitoring, logging, and alerting

**Main resources**:
- **CloudWatch Dashboard**: System metrics visualization
- **CloudWatch Alarms**:
  - API Gateway 5xx errors
  - Lambda errors
  - RDS CPU utilization
- **CloudWatch Log Groups**: Lambda function logs

**Estimated cost**: ~$1-2/month

---

### Stack Dependencies Flow

```
Network Stack (VPC, Subnets, NAT Gateway)
  ↓
┌─────────────────────────────────────┐
│  Storage Stack    Auth Stack        │
│  (S3 Buckets)     (Cognito)         │
│                                     │
│  Database Stack                     │
│  (RDS SQL Server)                   │
└─────────────────────────────────────┘
  ↓
API Stack (API Gateway + Lambda)
  ↓
Frontend Stack (S3 + CloudFront)
  ↓
Monitoring Stack (CloudWatch)
```

---

### Total Estimated Cost

**Development Environment:**

| Service | Cost/month | Notes |
|---------|------------|-------|
| NAT Gateway | $23 | 1 instance  |
| RDS SQL Server | $15 | t3.micro  |
| Lambda | $2 | 11 modules, 128MB |
| S3 Storage | $1.25 | Images + Frontend |
| CloudFront | $1.50 | CDN distribution |
| CloudWatch | $1.50 | Dashboard + Logs |
| Cognito | $0 | Free tier (50K MAU) |
| API Gateway | $0-3 | Free tier (1M requests) |
| **TOTAL** | **~$44/month** | 60% reduction from $111 |

---

## Configuration Guide

### Step 1: Navigate to Infrastructure Directory

```powershell
cd OJT_infrastructure
```

### Step 2: Install Dependencies

```powershell
npm install
```

### Step 3: Configure Environment Variables

**1. Copy environment template**

```powershell
copy .env.example .env
```

**2. Edit .env file with your values**

```bash
# AWS Configuration
AWS_ACCOUNT_ID=123456789012
AWS_REGION=ap-southeast-1

# Database Configuration
DB_NAME=demoaws
DB_USERNAME=admin
DB_PASSWORD=YourSecurePassword123!

# Application Configuration
APP_NAME=OJT-Ecommerce
ENVIRONMENT=dev

# JWT Secret
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
```

**3. Verify AWS Account ID**

```powershell
aws sts get-caller-identity
```

Output:
```json
{
    "UserId": "AIDAXXXXXXXXXXXXXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/your-username"
}
```

### Step 4: Validate Configuration

**1. Compile TypeScript**

```powershell
npm run build
```

**2. List all CDK stacks**

```powershell
npx cdk list
```

Expected output:
```
OJT-NetworkStack
OJT-StorageStack
OJT-AuthStack
OJT-DatabaseStack
OJT-ApiStack
OJT-FrontendStack
OJT-MonitoringStack
```

**3. Synthesize CloudFormation templates**

```powershell
npx cdk synth
```

The `cdk.out/` folder is created with CloudFormation templates.

### Step 5: Review Stack Code (Optional)

**Network Stack** (`lib/stacks/network-stack.ts`):
- VPC with 10.0.0.0/16 CIDR
- Public, Private, Isolated subnets
- 1 NAT Gateway (cost optimized)
- Security Groups for Lambda and RDS

**Database Stack** (`lib/stacks/database-stack.ts`):
- RDS SQL Server Express 2019
- db.t3.micro instance (cost optimized)
- Secrets Manager for credentials
- 1-day backup retention

**API Stack** (`lib/stacks/api-stack.ts`):
- API Gateway REST API
- 11 Lambda modules with placeholder code
- VPC integration for database access

---

## Configuration Checklist

Before deploying, verify the following:

- [ ] **Environment Configuration**
  - [ ] AWS Account ID verified and updated in `.env`
  - [ ] Region set to `ap-southeast-1`
  - [ ] Database credentials configured
  - [ ] JWT secret configured

- [ ] **Dependencies**
  - [ ] Node.js 20.x installed
  - [ ] AWS CLI configured with credentials
  - [ ] AWS CDK CLI installed globally
  - [ ] npm dependencies installed (`npm install`)

- [ ] **Validation**
  - [ ] TypeScript compilation successful (`npm run build`)
  - [ ] CDK list shows all 7 stacks
  - [ ] CDK synth generates CloudFormation templates

- [ ] **Preparation**
  - [ ] CDK bootstrap completed 
  - [ ] AWS account has AdministratorAccess permissions

---

## Next Steps

After completing configuration and validation, continue to:

- Deploy all stacks to AWS

In the next step, you will:
1. Deploy Network Stack (VPC, Subnets, NAT Gateway)
2. Deploy Storage Stack (S3 Buckets)
3. Deploy Auth Stack (Cognito - optional)
4. Deploy Database Stack (RDS SQL Server)
5. Deploy API Stack (API Gateway + Lambda)
6. Deploy Frontend Stack (S3 + CloudFront)
7. Deploy Monitoring Stack (CloudWatch)

