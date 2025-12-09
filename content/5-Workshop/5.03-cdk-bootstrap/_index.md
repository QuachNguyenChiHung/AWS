---
title: "CDK Bootstrap"
date: 2025-12-01
weight: 3
chapter: false
pre: " <b> 5.03. </b> "
---

#### Overview

AWS CDK Bootstrap prepares your AWS account for deploying CDK applications by creating essential infrastructure resources. This is a one-time setup process per AWS account/region combination that establishes the foundation for all CDK deployments.

#### What CDK Bootstrap Creates

The bootstrap process provisions the following AWS resources in your account:

**1. S3 Bucket (CDK Assets Bucket)**
- Stores Lambda function deployment packages
- Stores CloudFormation templates
- Stores file assets referenced by your CDK stacks
- Naming pattern: `cdk-hnb659fds-assets-{ACCOUNT-ID}-{REGION}`
- Versioning enabled for rollback support

**2. CloudFormation Stack (CDKToolkit)**
- Main infrastructure stack containing all bootstrap resources
- Stack name: `CDKToolkit`
- Manages lifecycle of bootstrap resources

**3. IAM Roles**
- `cdk-hnb659fds-cfn-exec-role`: CloudFormation execution role for deploying stacks
- `cdk-hnb659fds-deploy-role`: Role used by CDK CLI during deployment
- `cdk-hnb659fds-file-publishing-role`: Role for uploading assets to S3
- `cdk-hnb659fds-lookup-role`: Role for reading environment context

**4. SSM Parameters**
- `/cdk-bootstrap/hnb659fds/version`: Stores bootstrap version number
- Used for compatibility checking

#### Infrastructure Stacks Overview

After bootstrapping, our OJT E-commerce project will deploy multiple CDK stacks defined in `OJT_infrastructure/lib/stacks`:

| Stack | File | Description |
|-------|------|-------------|
| NetworkStack | network-stack.ts | VPC, Subnets, NAT Gateway, Security Groups |
| StorageStack | storage-stack.ts | S3 Buckets (Images, Logs) |
| AuthStack | auth-stack.ts | Cognito User Pool & Identity Pool |
| DatabaseStack | database-stack.ts | RDS SQL Server + Secrets Manager |
| ApiStack | api-stack.ts | API Gateway + 11 Lambda Modules |
| FrontendStack | frontend-stack.ts | S3 + CloudFront (OAC) |
| MonitoringStack | monitoring-stack.ts | CloudWatch Dashboard & Alarms |

These stacks depend on the bootstrap infrastructure to deploy successfully.

---

### Lab Instructions: Bootstrap Your AWS Account

#### Prerequisites

Before beginning this lab, ensure you have:

- AWS CLI installed and configured (see section 5.02)
- AWS CDK CLI installed (see section 5.02)
- Valid AWS credentials configured
- IAM user with `AdministratorAccess` permissions or equivalent
- Terminal/PowerShell access to the project directory

#### Step 1: Navigate to Infrastructure Directory

```bash
cd OJT_infrastructure
```

#### Step 2: Get Your AWS Account ID

```bash

aws sts get-caller-identity --query Account --output text




```bash
# Bootstrap CDK for ap-southeast-1 region
cdk bootstrap aws://YOUR_ACCOUNT_ID/ap-southeast-1

# Example:
cdk bootstrap aws://123456789012/ap-southeast-1
```

**Expected Output:**
```
Bootstrapping environment aws://123456789012/ap-southeast-1...
Environment aws://123456789012/ap-southeast-1 bootstrapped.
```

![CDK Bootstrap Success]
*Screenshot: Terminal showing CDK bootstrap completed successfully*

#### Step 4: Verify Bootstrap via CLI

```bash
# Check bootstrap version
aws ssm get-parameter \
  --name /cdk-bootstrap/hnb659fds/version \
  --region ap-southeast-1 \
  --query Parameter.Value \
  --output text

# Expected output: A version number (e.g., 19)
```

![Bootstrap Verification CLI]
*Screenshot: Terminal showing bootstrap version verification*

---

### Step 5: Verify on AWS Console

Confirm resources in the AWS Management Console.

**5.1. Verify CloudFormation Stack**

1. Open [CloudFormation Console]
2. Look for stack named `CDKToolkit`
3. Status should be `CREATE_COMPLETE`
4. Click on **Resources** tab to view created resources

![CloudFormation CDKToolkit Stack]
*Screenshot: CloudFormation console showing CDKToolkit stack*

**5.2. Verify S3 Bucket**

1. Open [S3 Console](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1)
2. Find bucket: `cdk-hnb659fds-assets-{ACCOUNT-ID}-ap-southeast-1`
3. Verify bucket properties:
   - Versioning: Enabled

![S3 Bucket Console]
*Screenshot: S3 console showing CDK assets bucket*

**5.3. Verify IAM Roles**

1. Open [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles)
2. Search for: `cdk-hnb659fds`
3. Verify the following roles exist:
   - `cdk-hnb659fds-cfn-exec-role-{ACCOUNT-ID}-ap-southeast-1`
   - `cdk-hnb659fds-deploy-role-{ACCOUNT-ID}-ap-southeast-1`
   - `cdk-hnb659fds-file-publishing-role-{ACCOUNT-ID}-ap-southeast-1`
   - `cdk-hnb659fds-lookup-role-{ACCOUNT-ID}-ap-southeast-1`

![IAM Roles Console]
*Screenshot: IAM console showing CDK bootstrap roles*

---

### Understanding Bootstrap Resources

#### S3 Bucket Details

The CDK assets bucket serves as a central repository for all deployment artifacts:

- **Purpose**: Stores CloudFormation templates, Lambda deployment packages, and static assets
- **Lifecycle**: Persists across deployments, enabling rollback capabilities
- **Security**: Encrypted at rest, bucket policies restrict access to authorized roles
- **Naming**: Deterministic naming based on account ID and region

#### IAM Roles Details

**1. CloudFormation Execution Role (`cfn-exec-role`)**
- Used by CloudFormation to create/update/delete stack resources
- Has broad permissions to manage AWS resources
- Trust relationship with CloudFormation service

**2. Deploy Role (`deploy-role`)**
- Used by CDK CLI during `cdk deploy` operations
- Can assume the CloudFormation execution role
- Has permissions to upload assets and initiate deployments

**3. File Publishing Role (`file-publishing-role`)**
- Uploads Lambda packages and assets to S3
- Has S3 write permissions to the assets bucket

**4. Lookup Role (`lookup-role`)**
- Reads environment context (VPCs, subnets, etc.)
- Read-only permissions for resource lookups
- Used during synthesis for context queries

---

### Cost Analysis

#### Bootstrap Resources Cost

**One-time Setup:**
- CloudFormation stack creation: **Free**
- IAM roles creation: **Free**
- SSM parameters: **Free**

**Ongoing Monthly Costs:**

| Resource | Usage | Cost (Estimated) |
|----------|-------|------------------|
| S3 Storage | < 1 GB (typical) | $0.023/GB = ~$0.02/month |
| S3 Requests | Minimal (GET/PUT) | ~$0.01/month |
| IAM Roles | No charge | $0.00 |

**Total Estimated Cost:** ~$0.03/month (negligible)

**Note:** For serverless applications, costs remain minimal as we only store Lambda deployment packages in S3.

---

### Troubleshooting

**Issue: Bootstrap fails with permission error**
```bash
# Ensure your IAM user has AdministratorAccess
aws iam list-attached-user-policies --user-name YOUR_USERNAME
```

**Issue: Region mismatch**
```bash
# Verify your default region
aws configure get region

# Bootstrap specific region
cdk bootstrap aws://ACCOUNT_ID/ap-southeast-1
```

**Issue: CDK version mismatch**
```bash
# Update CDK CLI
npm update -g aws-cdk

# Verify version
cdk --version
```

---

### Lab Completion Checklist

Ensure you have completed all steps:

- [ ] Step 1: Navigate to OJT_infrastructure directory
- [ ] Step 2: Retrieved your AWS account ID
- [ ] Step 3: Executed CDK bootstrap command
- [ ] Step 4: Verified bootstrap version via CLI
- [ ] Step 5.1: Verified CDKToolkit CloudFormation stack
- [ ] Step 5.2: Verified CDK assets S3 bucket
- [ ] Step 5.3: Verified IAM roles created

#### Summary

In this lab, you successfully:

1. Executed CDK bootstrap command for your AWS account (ap-southeast-1)
2. Created CDKToolkit CloudFormation stack
3. Provisioned CDK assets S3 bucket for storing Lambda packages
4. Created necessary IAM roles for CDK deployments
5. Verified all resources via CLI and AWS Console

Your AWS account is now ready to deploy serverless CDK applications, including the infrastructure stacks for the OJT E-commerce project.

#### Next Steps

Proceed to [Deploy Core Infrastructure] to deploy VPC, Database, Storage, and Auth stacks.

