---
title: "Setup Environment"
date: 2025-12-01
weight: 2
chapter: false
pre: " <b> 5.02. </b> "
---

#### Overview

In this step, you will install all the necessary tools to develop and deploy the OJT E-commerce application.

#### Required Tools

**1. Node.js 20.x**

```bash
# Download from https://nodejs.org/
# Or use nvm (recommended)
nvm install 20
nvm use 20

# Verify installation
node --version  # Should be v20.x
npm --version
```

![Node.js Installation]
*Screenshot: Terminal showing Node.js 20.x installed*

**2. AWS CLI v2**

```bash
# Windows: Download from https://aws.amazon.com/cli/
# macOS: brew install awscli
# Linux: 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version
```

![AWS CLI Installation]
*Screenshot: Terminal showing AWS CLI v2 installed*

**3. AWS CDK CLI**

```bash
# Install CDK globally
npm install -g aws-cdk

# Verify installation
cdk --version
```

![CDK CLI Installation]
*Screenshot: Terminal showing CDK CLI installed*

**4. Git**

```bash
# Download from https://git-scm.com/
# Or use package manager

# Verify installation
git --version
```
![Git Installation]

**5. Code Editor**

Recommended: Visual Studio Code with extensions:
- AWS Toolkit
- GitLab Workflow
- ESLint
- Prettier

#### AWS Account Setup

**1. Create AWS Account**

If you don't have an AWS account:
1. Go to https://aws.amazon.com/
2. Click "Create an AWS Account"
3. Follow the registration process
4. Add payment method

**2. Create IAM User**

For security, don't use root account

1. Go to IAM Console → Users → Create user
2. Create user and save credentials

![IAM User Created]
*Screenshot: IAM console showing user created with AdministratorAccess*

**3. Configure AWS CLI**

```bash
# Configure AWS credentials
aws configure

# Enter:
# AWS Access Key ID: [Your Access Key]
# AWS Secret Access Key: [Your Secret Key]
# Default region name: ap-southeast-1
# Default output format: json
```

![AWS CLI Configure]
*Screenshot: Terminal showing aws configure completed*

**4. Verify AWS Access**

```bash
# Test AWS credentials
aws sts get-caller-identity

# Should return your account ID and user ARN
```
![AWS Access Verified]

#### Domain Setup (Optional)

If you want to use a custom domain:

**1. Register Domain**

Buy domain name on hpanel.hostinger
Route 53 creates dns record to hostinger 
For this workshop, we use: `yourdomain.com`


**2. Note Domain Registrar**

You'll need access to domain registrar to update nameservers later.

#### GitLab Setup

**Create GitLab Repo**

**3. Configure Git**

```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify configuration
git config --list
```

#### Project Setup

**1. Clone or Create Project**

Option A: Clone existing project
```bash
git clone https://gitlab.com/your-username/ojt-ecommerce.git
cd ojt-ecommerce
```

Option B: Create new project
```bash
mkdir OJT
cd OJT
git init
```

**2. Project Structure**

The OJT E-commerce project has the following structure:
```
OJT/
├── OJT_infrastructure/      # CDK Infrastructure (TypeScript)
│   └── Deploy AWS resources: VPC, RDS, S3, API Gateway, etc.
│
├── OJT_lambda/             # Lambda Functions (JavaScript) - 63 APIs
│   └── Application code for API endpoints
│
├── OJT_frontendDev/        # Frontend (React + Vite)
│   └── Web application
│
└── database/               # Database Scripts (MySQL)
    ├── schema/             # Main schema
    ├── migrations/         # Migration scripts
    └── seeds/              # Sample data
```

**3. Install Dependencies**

```bash
# Install CDK Infrastructure dependencies
cd OJT_infrastructure
npm install

# Install Lambda dependencies
cd ../OJT_lambda
npm install
npm run install:all

# Install Frontend dependencies
cd ../OJT_frontendDev
npm install
```

**4. Copy Environment Variables**

For CDK Infrastructure:
```bash
cd OJT_infrastructure
cp .env.example .env
```

Edit `.env` with your values:
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

For Lambda:
```bash
cd ../OJT_lambda
cp .env.example .env
```

Edit `.env` with your values:
```bash
# AWS Configuration
AWS_REGION=ap-southeast-1
AWS_ACCOUNT_ID=your-account-id

# JWT Configuration
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d
```

#### Verification

Check that everything is installed correctly:

```bash
# Check Node.js
node --version  # v20.x

# Check npm
npm --version   # 10.x

# Check AWS CLI
aws --version   # aws-cli/2.x

# Check CDK
cdk --version   # 2.x

# Check Git
git --version   # 2.x

# Check AWS credentials
aws sts get-caller-identity

# Check project dependencies
cd OJT_infrastructure
npm list --depth=0
```

#### Troubleshooting

**Issue: Node.js version mismatch**
```bash
# Use nvm to switch versions
nvm install 20
nvm use 20
```

**Issue: AWS CLI not found**
- Restart terminal after installation
- Check PATH environment variable

**Issue: CDK command not found**
```bash
# Reinstall CDK globally
npm uninstall -g aws-cdk
npm install -g aws-cdk
```

**Issue: AWS credentials invalid**
```bash
# Reconfigure AWS CLI
aws configure
# Enter correct credentials
```

#### Next Steps

Once your environment is set up, proceed to [CDK Bootstrap] to prepare your AWS account for CDK deployments.

