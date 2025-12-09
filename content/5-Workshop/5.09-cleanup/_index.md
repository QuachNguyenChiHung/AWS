---
title: "Cleanup"
date: 2025-12-01
weight: 9
chapter: false
pre: " <b> 5.09. </b> "
---

#### Overview

When you complete the workshop and no longer want to continue using it, delete all resources to avoid incurring charges.

**Warning:** This process cannot be undone. All data will be permanently deleted.

#### Resources to Delete

OJT E-commerce project includes:

| Stack | Resources | Monthly Cost |
|-------|-----------|--------------|
| MonitoringStack | CloudWatch Dashboard, Alarms | ~$1.50 |
| FrontendStack | S3, CloudFront | ~$1.50 |
| ApiStack | API Gateway, 11 Lambda functions | ~$2 |
| DatabaseStack | RDS SQL Server, Secrets Manager | ~$15 |
| AuthStack | Cognito User Pool | $0 |
| StorageStack | S3 Buckets | ~$1.25 |
| NetworkStack | VPC, NAT Gateway | ~$23 |
| **Total** | | **~$44/month** |

#### Cleanup Order

Must delete in reverse order of deployment:

```
1. MonitoringStack
2. FrontendStack
3. ApiStack
4. DatabaseStack
5. AuthStack
6. StorageStack
7. NetworkStack
8. CDK Bootstrap (optional)
```

---

### Step 1: Navigate to Infrastructure Directory

```powershell
cd D:\AWS\Src\OJT\OJT_infrastructure
```

---

### Step 2: Empty S3 Buckets

**S3 buckets must be emptied before deletion:**

```powershell
# List all OJT buckets
aws s3 ls | Select-String "ojt"

# Empty Images bucket
aws s3 rm s3://ojt-ecommerce-images-123456789012 --recursive

# Empty Logs bucket
aws s3 rm s3://ojt-ecommerce-logs-123456789012 --recursive

# Empty Frontend bucket (if deployed)
aws s3 rm s3://ojt-ecommerce-frontend-123456789012 --recursive
```

**Or use PowerShell script:**

```powershell
# Get all OJT buckets and empty them
$buckets = aws s3 ls | Select-String "ojt" | ForEach-Object { 
  $_.ToString().Split()[-1] 
}

foreach ($bucket in $buckets) {
  Write-Host "Emptying bucket: $bucket" -ForegroundColor Yellow
  aws s3 rm "s3://$bucket" --recursive
  Write-Host "Bucket emptied: $bucket" -ForegroundColor Green
}
```

---

### Step 3: Delete CDK Stacks

#### 3.1 Delete Monitoring Stack

```powershell
# Delete Monitoring stack
npx cdk destroy OJT-MonitoringStack --force

# Or with confirmation
npx cdk destroy OJT-MonitoringStack
# Type 'y' to confirm
```

#### 3.2 Delete Frontend Stack

```powershell
# Delete Frontend stack
npx cdk destroy OJT-FrontendStack --force
```

#### 3.3 Delete API Stack

```powershell
# Delete API stack (Lambda functions + API Gateway)
npx cdk destroy OJT-ApiStack --force
```

#### 3.4 Delete Database Stack

```powershell
# Delete Database stack (RDS SQL Server)
# This takes 5-10 minutes
npx cdk destroy OJT-DatabaseStack --force
```

**Note:** RDS deletion creates a final snapshot by default.

#### 3.5 Delete Auth Stack

```powershell
# Delete Auth stack (Cognito)
npx cdk destroy OJT-AuthStack --force
```

#### 3.6 Delete Storage Stack

```powershell
# Delete Storage stack (S3 buckets)
npx cdk destroy OJT-StorageStack --force
```

#### 3.7 Delete Network Stack

```powershell
# Delete Network stack (VPC, NAT Gateway)
# This takes 3-5 minutes
npx cdk destroy OJT-NetworkStack --force
```

---

### Step 4: Delete All Stacks at Once (Alternative)

```powershell
# Delete all stacks in correct order
npm run destroy

# Or using CDK directly
npx cdk destroy --all --force
```

**Expected output:**

```
OJT-MonitoringStack: destroying...
OJT-MonitoringStack: destroyed

OJT-FrontendStack: destroying...
OJT-FrontendStack: destroyed

OJT-ApiStack: destroying...
OJT-ApiStack: destroyed

OJT-DatabaseStack: destroying...
OJT-DatabaseStack: destroyed

OJT-AuthStack: destroying...
OJT-AuthStack: destroyed

OJT-StorageStack: destroying...
OJT-StorageStack: destroyed

OJT-NetworkStack: destroying...
OJT-NetworkStack: destroyed
```

---

### Step 5: Verify All Stacks Deleted

```powershell
# List remaining stacks
aws cloudformation list-stacks `
  --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
  --query "StackSummaries[?contains(StackName, 'OJT')].StackName" `
  --output table

# Should return empty or no OJT stacks
```

---

### Step 6: Delete CDK Bootstrap (Optional)

**⚠️ Only do this if you're done with CDK completely in this region:**

```powershell
# Get CDK assets bucket name
$BUCKET_NAME = aws s3 ls | Select-String "cdk-" | ForEach-Object { 
  $_.ToString().Split()[-1] 
}

# Empty CDK assets bucket
aws s3 rm "s3://$BUCKET_NAME" --recursive

# Delete CDK bootstrap stack
aws cloudformation delete-stack --stack-name CDKToolkit

# Wait for deletion
aws cloudformation wait stack-delete-complete --stack-name CDKToolkit

Write-Host "CDK Bootstrap deleted" -ForegroundColor Green
```

---

### Step 7: Verify Complete Cleanup

#### 7.1 Check CloudFormation

```powershell
aws cloudformation list-stacks `
  --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
  | Select-String "OJT"

# Should return nothing
```

#### 7.2 Check S3

```powershell
aws s3 ls | Select-String "ojt"

# Should return nothing
```

#### 7.3 Check Lambda

```powershell
aws lambda list-functions `
  --query "Functions[?contains(FunctionName, 'OJT')].FunctionName"

# Should return empty array
```

#### 7.4 Check RDS

```powershell
aws rds describe-db-instances `
  --query "DBInstances[?contains(DBInstanceIdentifier, 'ojt')].DBInstanceIdentifier"

# Should return empty array
```

#### 7.5 Check API Gateway

```powershell
aws apigateway get-rest-apis `
  --query "items[?contains(name, 'OJT')].name"

# Should return empty array
```

#### 7.6 Check Cognito

```powershell
aws cognito-idp list-user-pools --max-results 10 `
  --query "UserPools[?contains(Name, 'OJT')].Name"

# Should return empty array
```

#### 7.7 Check NAT Gateway

```powershell
aws ec2 describe-nat-gateways `
  --filter "Name=state,Values=available" `
  --query "NatGateways[].NatGatewayId"

# Should not show OJT NAT Gateways
```

---

### Step 8: Delete RDS Snapshots (Optional)

```powershell
# List RDS snapshots
aws rds describe-db-snapshots `
  --query "DBSnapshots[?contains(DBSnapshotIdentifier, 'ojt')].DBSnapshotIdentifier"

# Delete each snapshot
aws rds delete-db-snapshot --db-snapshot-identifier ojt-database-final-snapshot
```

---

### Step 9: Delete CloudWatch Log Groups (Optional)

```powershell
# List log groups
aws logs describe-log-groups `
  --log-group-name-prefix /aws/lambda/OJT `
  --query "logGroups[].logGroupName"

# Delete each log group
$logGroups = aws logs describe-log-groups `
  --log-group-name-prefix /aws/lambda/OJT `
  --query "logGroups[].logGroupName" `
  --output text

foreach ($logGroup in $logGroups.Split()) {
  Write-Host "Deleting log group: $logGroup"
  aws logs delete-log-group --log-group-name $logGroup
}
```

---

### Cost After Cleanup

**Immediate:**
- Most resources: $0/month
- RDS snapshots: ~$0.095/GB/month (if kept)

**After complete cleanup:**
- Everything: $0/month

---

### Troubleshooting Cleanup

#### Issue: S3 Bucket Deletion Fails

```powershell
# Force empty and delete
aws s3 rb s3://bucket-name --force
```

#### Issue: CloudFormation Stack Stuck in DELETE_IN_PROGRESS

```powershell
# Check stack events for errors
aws cloudformation describe-stack-events `
  --stack-name OJT-NetworkStack `
  --max-items 10

# If stuck, wait or check for dependencies
```

#### Issue: RDS Deletion Protection Enabled

```powershell
# Disable deletion protection
aws rds modify-db-instance `
  --db-instance-identifier ojt-database `
  --no-deletion-protection `
  --apply-immediately

# Wait a few minutes, then retry delete
```

#### Issue: VPC Has Dependencies

```powershell
# Check for remaining ENIs
aws ec2 describe-network-interfaces `
  --filters "Name=vpc-id,Values=vpc-xxxxxxxx"

# Delete any remaining ENIs manually
aws ec2 delete-network-interface --network-interface-id eni-xxxxxxxx
```

#### Issue: NAT Gateway Still Exists

```powershell
# Delete NAT Gateway manually
aws ec2 delete-nat-gateway --nat-gateway-id nat-xxxxxxxx

# Release Elastic IP
aws ec2 release-address --allocation-id eipalloc-xxxxxxxx
```

---

### Cleanup Checklist

#### CDK Stacks
- [ ] MonitoringStack deleted
- [ ] FrontendStack deleted
- [ ] ApiStack deleted
- [ ] DatabaseStack deleted
- [ ] AuthStack deleted
- [ ] StorageStack deleted
- [ ] NetworkStack deleted

#### AWS Resources
- [ ] All S3 buckets emptied and deleted
- [ ] No remaining Lambda functions
- [ ] No remaining RDS instances
- [ ] No remaining API Gateways
- [ ] No remaining Cognito User Pools
- [ ] No remaining NAT Gateways
- [ ] No remaining VPCs

#### Optional Cleanup
- [ ] CDK Bootstrap deleted
- [ ] RDS snapshots deleted
- [ ] CloudWatch Log Groups deleted
- [ ] GitLab repository archived/deleted

#### Verification
- [ ] CloudFormation shows no OJT stacks
- [ ] AWS Billing shows decreasing costs
- [ ] No unexpected charges

---

### Conclusion

You have completed the OJT E-commerce workshop! You have learned:

**Infrastructure as Code** with AWS CDK (TypeScript)  
**Serverless Architecture** with Lambda and API Gateway  
**RDS SQL Server** database management  
**VPC Design** with public/private/isolated subnets  
**S3 Storage** for images and static files  
**CloudFront CDN** with Origin Access Control  
**Cognito Authentication** (optional)  
**JWT Authentication** with custom Lambda  
**CloudWatch Monitoring** with dashboards and alarms  
**Cost Optimization** strategies for serverless  
**GitLab** version control  
**2-Step Deployment** strategy (Infrastructure + Lambda code)

**Total deployed:**
- 7 CDK stacks
- 11 Lambda modules (63 APIs)
- RDS SQL Server database
- VPC with NAT Gateway
- S3 buckets for storage
- CloudFront CDN
- CloudWatch monitoring
- Production-ready e-commerce platform

**Estimated monthly cost:** ~$44/month (optimized)

Thank you for completing the workshop! 🎉

