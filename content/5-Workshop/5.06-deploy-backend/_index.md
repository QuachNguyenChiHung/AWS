---
title: "Deploy Backend Services"
date: 2025-12-01
weight: 6
chapter: false
pre: " <b> 5.06. </b> "
---

#### Overview

Dự án OJT E-commerce sử dụng kiến trúc **2-step deployment**: Infrastructure (CDK) và Lambda Code (riêng biệt). Trong bước này, bạn sẽ deploy cả infrastructure và Lambda code.

#### Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    2-Step Deployment                         │
├─────────────────────────────────────────────────────────────┤
│  Step 1: Deploy Infrastructure (CDK)                        │
│  ├─ NetworkStack: VPC, Subnets, NAT Gateway                │
│  ├─ StorageStack: S3 Buckets                               │
│  ├─ AuthStack: Cognito (optional)                          │
│  ├─ DatabaseStack: RDS SQL Server                          │
│  ├─ ApiStack: API Gateway + Placeholder Lambda             │
│  ├─ FrontendStack: S3 + CloudFront                         │
│  └─ MonitoringStack: CloudWatch                            │
├─────────────────────────────────────────────────────────────┤
│  Step 2: Deploy Lambda Code                                 │
│  └─ 11 Lambda Modules (63 APIs) → Update function code     │
└─────────────────────────────────────────────────────────────┘
```

#### Lambda Modules (11 modules - 63 APIs)

| Module | Functions | Description |
|--------|-----------|-------------|
| Auth | 4 | Login, Signup, Logout, Me |
| Products | 12 | CRUD, Search, Filter |
| ProductDetails | 7 | CRUD, Images |
| Cart | 6 | Add, Get, Update, Remove |
| Orders | 9 | CRUD, COD, Status |
| Categories | 6 | CRUD, Search |
| Brands | 5 | CRUD |
| Banners | 7 | CRUD, Toggle |
| Ratings | 3 | Get, Stats, Create |
| Users | 3 | GetAll, GetById, Update |
| Images | 1 | Upload to S3 |

---

### Step 1: Deploy Infrastructure (CDK)

#### 1.1 Navigate to Infrastructure Directory

```powershell
cd OJT_infrastructure
```

#### 1.2 Install Dependencies

```powershell
npm install
```

#### 1.3 Build TypeScript

```powershell
npm run build
```

#### 1.4 Deploy Core Stacks

```powershell
# Deploy Network, Storage, Auth, Database stacks
npm run deploy:core
```

**Expected output:**
```
   OJT-NetworkStack
   OJT-StorageStack
   OJT-AuthStack
   OJT-DatabaseStack

Outputs:
OJT-NetworkStack.VpcId = vpc-0123456789abcdef0
OJT-DatabaseStack.DbEndpoint = ojt-database.xxx.ap-southeast-1.rds.amazonaws.com
OJT-StorageStack.ImagesBucketName = ojt-ecommerce-images-123456789012
```

**Deploy time:** ~15-20 minutes (RDS takes longest)


*Screenshot: CDK deploying core stacks*

#### 1.5 Deploy API Stack

```powershell
# Deploy API Gateway + Placeholder Lambda
npm run deploy:api
```

**Expected output:**
```
 ✅  OJT-ApiStack

Outputs:
OJT-ApiStack.ApiUrl = https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod
OJT-ApiStack.AuthModuleName = OJT-Ecommerce-AuthModule
OJT-ApiStack.ProductsModuleName = OJT-Ecommerce-ProductsModule
...
```

**Deploy time:** ~3-5 minutes


*Screenshot: CDK deploying API stack*

#### 1.6 Verify CDK Deployment

```powershell
# List all deployed stacks
aws cloudformation list-stacks `
  --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
  --query "StackSummaries[?contains(StackName, 'OJT')].StackName" `
  --output table
```

**Expected output:**
```
---------------------------------
|         ListStacks            |
+-------------------------------+
|  OJT-NetworkStack             |
|  OJT-StorageStack             |
|  OJT-AuthStack                |
|  OJT-DatabaseStack            |
|  OJT-ApiStack                 |
+-------------------------------+
```

---

### Step 2: Deploy Lambda Code

Sau khi CDK deploy xong, Lambda functions có placeholder code. Bây giờ deploy actual code.

#### 2.1 Navigate to Lambda Directory

```powershell
cd ..\OJT_lambda
```

#### 2.2 Install Dependencies

```powershell
# Install main dependencies
npm install

# Install all module dependencies
npm run install:all
```

#### 2.3 Configure Environment

```powershell
# Copy environment template
copy .env.example .env

# Edit .env with CDK outputs
notepad .env
```

**Update .env with values from CDK outputs:**

```bash
# AWS Configuration
AWS_REGION=ap-southeast-1
AWS_ACCOUNT_ID=123456789012

# Database (from OJT-DatabaseStack outputs)
DB_HOST=ojt-database.xxx.ap-southeast-1.rds.amazonaws.com
DB_NAME=demoaws
DB_SECRET_ARN=arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:OJT/RDS/Credentials

# JWT
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d

# S3 (from OJT-StorageStack outputs)
S3_IMAGES_BUCKET=ojt-ecommerce-images-123456789012
```

#### 2.4 Build Lambda Packages

```powershell
# Build all Lambda packages
npm run build
```

**Expected output:**
```
Building auth module... Done
Building products module... Done
Building product-details module... Done
Building cart module... Done
Building orders module... Done
Building categories module... Done
Building brands module... Done
Building banners module... Done
Building ratings module... Done
Building users module... Done
Building images module... Done

Build completed! ZIP files in build/ directory.
```

**Build creates:**
```
build/
├── auth.zip           (~500 KB)
├── products.zip       (~600 KB)
├── product-details.zip (~550 KB)
├── cart.zip           (~450 KB)
├── orders.zip         (~500 KB)
├── categories.zip     (~400 KB)
├── brands.zip         (~350 KB)
├── banners.zip        (~400 KB)
├── ratings.zip        (~350 KB)
├── users.zip          (~400 KB)
└── images.zip         (~300 KB)
```

#### 2.5 Deploy Lambda Code

```powershell
# Deploy all Lambda functions
npm run deploy
```

**Expected output:**
```
Deploying auth module to OJT-Ecommerce-AuthModule... Done
Deploying products module to OJT-Ecommerce-ProductsModule... Done
Deploying product-details module to OJT-Ecommerce-ProductDetailsModule... Done
Deploying cart module to OJT-Ecommerce-CartModule... Done
Deploying orders module to OJT-Ecommerce-OrdersModule... Done
Deploying categories module to OJT-Ecommerce-CategoriesModule... Done
Deploying brands module to OJT-Ecommerce-BrandsModule... Done
Deploying banners module to OJT-Ecommerce-BannersModule... Done
Deploying ratings module to OJT-Ecommerce-RatingsModule... Done
Deploying users module to OJT-Ecommerce-UsersModule... Done
Deploying images module to OJT-Ecommerce-ImagesModule... Done

All Lambda functions deployed successfully!
```

**Deploy time:** ~1-2 minutes


*Screenshot: Lambda code deployment*

---

### Step 3: Verify Lambda Deployment

#### 3.1 List Lambda Functions

```powershell
aws lambda list-functions `
  --query "Functions[?contains(FunctionName, 'OJT-Ecommerce')].{Name:FunctionName,Runtime:Runtime,Updated:LastModified}" `
  --output table
```

**Expected output:**
```
--------------------------------------------------------------------
|                         ListFunctions                            |
+----------------------------------+------------+------------------+
|              Name                |  Runtime   |    Updated       |
+----------------------------------+------------+------------------+
|  OJT-Ecommerce-AuthModule        | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-ProductsModule    | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-ProductDetailsModule | nodejs20.x | 2025-12-09T...|
|  OJT-Ecommerce-CartModule        | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-OrdersModule      | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-CategoriesModule  | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-BrandsModule      | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-BannersModule     | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-RatingsModule     | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-UsersModule       | nodejs20.x | 2025-12-09T...   |
|  OJT-Ecommerce-ImagesModule      | nodejs20.x | 2025-12-09T...   |
+----------------------------------+------------+------------------+
```

#### 3.2 Check Function Configuration

```powershell
# Check Auth Module configuration
aws lambda get-function-configuration `
  --function-name OJT-Ecommerce-AuthModule `
  --query '{Runtime:Runtime,Handler:Handler,Timeout:Timeout,Memory:MemorySize}'
```

**Expected output:**
```json
{
    "Runtime": "nodejs20.x",
    "Handler": "index.handler",
    "Timeout": 30,
    "Memory": 128
}
```

#### 3.3 Verify Code Updated

```powershell
# Check code SHA256 (changes when code updates)
aws lambda get-function `
  --function-name OJT-Ecommerce-AuthModule `
  --query 'Configuration.CodeSha256'
```

---

### Step 4: Test Lambda Functions

#### 4.1 Test Auth Login

```powershell
# Invoke Auth Module
aws lambda invoke `
  --function-name OJT-Ecommerce-AuthModule `
  --payload '{\"httpMethod\":\"POST\",\"path\":\"/auth/login\",\"body\":\"{\\\"email\\\":\\\"test@test.com\\\",\\\"password\\\":\\\"Test123!\\\"}\"}' `
  response.json

# Check response
Get-Content response.json
```

#### 4.2 Test Products List

```powershell
# Invoke Products Module
aws lambda invoke `
  --function-name OJT-Ecommerce-ProductsModule `
  --payload '{\"httpMethod\":\"GET\",\"path\":\"/products\",\"queryStringParameters\":{\"page\":\"1\",\"limit\":\"10\"}}' `
  response.json

# Check response
Get-Content response.json
```

#### 4.3 Test via API Gateway

```powershell
# Get API URL
$apiUrl = aws cloudformation describe-stacks `
  --stack-name OJT-ApiStack `
  --query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' `
  --output text

# Test products endpoint
Invoke-RestMethod -Uri "$apiUrl/products" -Method Get
```

---

### Step 5: Check CloudWatch Logs

#### 5.1 Tail Logs Real-time

```powershell
# Tail Auth Module logs
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --follow
```

#### 5.2 View Recent Logs

```powershell
# View last 10 minutes
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --since 10m
```

#### 5.3 Search for Errors

```powershell
# Search for errors
aws logs filter-log-events `
  --log-group-name /aws/lambda/OJT-Ecommerce-AuthModule `
  --filter-pattern "ERROR"
```

![CloudWatch Logs](/images/5-Workshop/5.6-deploy-backend/cloudwatch-logs.png)
*Screenshot: CloudWatch Logs showing Lambda execution*

---

### Step 6: Deploy Frontend & Monitoring (Optional)

#### 6.1 Deploy Frontend Stack

```powershell
cd ..\OJT_infrastructure
npm run deploy:frontend
```

#### 6.2 Deploy Monitoring Stack

```powershell
npm run deploy:monitoring
```

---

### Deployment Summary

#### Time Estimates

| Step | Time | Notes |
|------|------|-------|
| CDK Deploy Core | 15-20 min | RDS takes longest |
| CDK Deploy API | 3-5 min | API Gateway + Lambda |
| Lambda Build | 1-2 min | ZIP packages |
| Lambda Deploy | 1-2 min | Update function code |
| **Total** | **~25 min** | First deployment |

#### Update Lambda Code Only

Khi chỉ thay đổi Lambda code (không thay đổi infrastructure):

```powershell
cd OJT_lambda

# Build and deploy (skip CDK)
npm run build
npm run deploy

# Time: ~2-3 minutes
```

---

### Deployment Checklist

#### Infrastructure (CDK)
- [ ] NetworkStack deployed (VPC, Subnets, NAT Gateway)
- [ ] StorageStack deployed (S3 Buckets)
- [ ] AuthStack deployed (Cognito - optional)
- [ ] DatabaseStack deployed (RDS SQL Server)
- [ ] ApiStack deployed (API Gateway + Placeholder Lambda)

#### Lambda Code
- [ ] Dependencies installed (`npm install` + `npm run install:all`)
- [ ] Environment configured (`.env` file)
- [ ] Lambda packages built (`npm run build`)
- [ ] Lambda code deployed (`npm run deploy`)

#### Verification
- [ ] All 11 Lambda functions listed
- [ ] Runtime: nodejs20.x
- [ ] Code SHA256 updated
- [ ] Test invocations successful
- [ ] API Gateway responding
- [ ] CloudWatch logs showing executions

---

### Troubleshooting

#### Issue: CDK Deploy Fails

```powershell
# Check AWS credentials
aws sts get-caller-identity

# Check CDK version
cdk --version

# Clean and rebuild
Remove-Item -Recurse -Force node_modules, cdk.out
npm install
npm run build
```

#### Issue: Lambda Deploy Fails

```powershell
# Check function exists
aws lambda list-functions | Select-String "OJT-Ecommerce"

# Check ZIP file created
Get-ChildItem build/*.zip

# Rebuild and redeploy
npm run build
npm run deploy
```

#### Issue: Function Timeout

```powershell
# Increase timeout
aws lambda update-function-configuration `
  --function-name OJT-Ecommerce-AuthModule `
  --timeout 60
```

#### Issue: Database Connection Error

```powershell
# Verify RDS endpoint
aws rds describe-db-instances `
  --db-instance-identifier ojt-database `
  --query 'DBInstances[0].Endpoint'

# Check Security Group allows Lambda
aws ec2 describe-security-groups `
  --filters "Name=group-name,Values=*OJT*RDS*"
```

---

### Next Steps

Backend đã deployed thành công! Tiếp theo:

1. **Test Endpoints**: Verify tất cả 63 API endpoints → 
2. **Deploy Frontend**: React application → 
3. **Monitor**: CloudWatch dashboards → 

