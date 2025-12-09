---
title: "Dọn dẹp tài nguyên"
date: 2025-12-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

#### Tổng quan

Khi bạn hoàn thành hội thảo và không muốn tiếp tục sử dụng, vui lòng xóa tất cả tài nguyên để tránh phát sinh phí.

**Cảnh báo:** Quá trình này không thể hoàn tác. Tất cả dữ liệu sẽ bị xóa vĩnh viễn.

#### Tài nguyên cần xóa

Dự án thương mại điện tử OJT bao gồm:

| Stack | Tài nguyên | Chi phí hàng tháng |
|-------|-----------|--------------|
| MonitoringStack | Bảng điều khiển CloudWatch, Báo động | ~$1.50 |
| FrontendStack | S3, CloudFront | ~$1.50 |
| ApiStack | API Gateway, 11 hàm Lambda | ~$2 |
| DatabaseStack | RDS SQL Server, Secrets Manager | ~$15 |
| AuthStack | Cognito User Pool | $0 |
| StorageStack | S3 Buckets | ~$1.25 |
| NetworkStack | VPC, NAT Gateway | ~$23 |
| **Tổng cộng** | | **~$44/tháng** |

#### Thứ tự dọn dẹp

Phải xóa theo thứ tự ngược lại với thứ tự triển khai:

```
1. MonitoringStack
2. FrontendStack
3. ApiStack
4. DatabaseStack
5. AuthStack
6. StorageStack
7. NetworkStack
8. CDK Bootstrap (tùy chọn)
```

---

### Bước 1: Điều hướng đến Thư mục Cơ sở hạ tầng

```powershell
cd D:\AWS\Src\OJT\OJT_infrastructure
```

---

### Bước 2: Làm trống các thùng S3

**Các thùng S3 phải được làm trống trước khi xóa:**

```powershell
# Liệt kê tất cả các thùng OJT
aws s3 ls | Select-String "ojt"

# Thùng chứa hình ảnh trống
aws s3 rm s3://ojt-ecommerce-images-123456789012 --recursive

# Thùng chứa nhật ký trống
aws s3 rm s3://ojt-ecommerce-logs-123456789012 --recursive

# Thùng chứa giao diện người dùng trống (nếu đã triển khai)
aws s3 rm s3://ojt-ecommerce-frontend-123456789012 --recursive
```

**Hoặc sử dụng tập lệnh PowerShell:**

```powershell
# Lấy tất cả các thùng chứa OJT và làm trống chúng
$buckets = aws s3 ls | Select-String "ojt" | ForEach-Object {
$_.ToString().Split()[-1]
}

foreach ($bucket in $buckets) {
Write-Host "Đang làm trống bucket: $bucket" -ForegroundColor Yellow
aws s3 rm "s3://$bucket" --recursive
Write-Host "Bucket đã làm trống: $bucket" -ForegroundColor Green
}
```

---

### Bước 3: Xóa Ngăn xếp CDK

#### 3.1 Xóa Ngăn xếp Giám sát

```powershell
# Xóa Ngăn xếp Giám sát
npx cdk destroy OJT-MonitoringStack --force

# Hoặc xác nhận
npx cdk destroy OJT-MonitoringStack
# Nhập 'y' để xác nhận
```

#### 3.2 Xóa Ngăn xếp Giao diện Người dùng

```powershell
# Xóa Ngăn xếp Giao diện Người dùng
npx cdk destroy OJT-FrontendStack --force
```

#### 3.3 Xóa ngăn xếp API

```powershell
# Xóa ngăn xếp API (Hàm Lambda + API Gateway)
npx cdk destroy OJT-ApiStack --force
```

#### 3.4 Xóa ngăn xếp cơ sở dữ liệu

```powershell
# Xóa ngăn xếp cơ sở dữ liệu (RDS SQL Server)
# Quá trình này mất 5-10 phút
npx cdk destroy OJT-DatabaseStack --force
```

**Lưu ý:** Việc xóa RDS sẽ tạo một bản chụp nhanh cuối cùng theo mặc định.

#### 3.5 Xóa Ngăn xếp Xác thực

```powershell
# Xóa Ngăn xếp Xác thực (Cognito)
npx cdk destroy OJT-AuthStack --force
```

#### 3.6 Xóa Ngăn xếp Lưu trữ

```powershell
# Xóa Ngăn xếp Lưu trữ (S3 bucket)
npx cdk destroy OJT-StorageStack --force
```

#### 3.7 Xóa Ngăn xếp Mạng

```powershell
# Xóa Ngăn xếp Mạng (VPC, NAT Gateway)
# Việc này mất 3-5 phút
npx cdk destroy OJT-NetworkStack --force
```

---

### Bước 4: Xóa Tất cả Ngăn xếp Cùng lúc (Phương án Khác)

```powershell
# Xóa tất cả các ngăn xếp theo đúng thứ tự
npm run destroy

# Hoặc sử dụng trực tiếp CDK
npx cdk destroy --all --force
```

**Dự kiến đầu ra:**

```
OJT-MonitoringStack: đang hủy...
OJT-MonitoringStack: đang hủy

OJT-FrontendStack: đang hủy...
OJT-FrontendStack: đang hủy

OJT-ApiStack: đang hủy...
OJT-ApiStack: đang hủy

OJT-DatabaseStack: đang hủy...
OJT-DatabaseStack: đang hủy

OJT-AuthStack: đang hủy...
OJT-AuthStack: đang hủy

OJT-StorageStack: đang hủy...
OJT-StorageStack: đang hủy

OJT-NetworkStack: đang hủy...
OJT-NetworkStack: đang hủy
```

---

### Bước 5: Xác minh tất cả các ngăn xếp đã xóa

```powershell
# Liệt kê các ngăn xếp còn lại
aws cloudformation list-stacks `
--stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
--query "StackSummaries[?contains(StackName, 'OJT')].StackName" `
--output table

# Nên trả về ngăn xếp OJT trống hoặc không có
```

---

### Bước 6: Xóa CDK Bootstrap (Tùy chọn)

**⚠️ Chỉ thực hiện việc này nếu bạn đã hoàn tất việc sử dụng CDK trong vùng này:**

```powershell
# Lấy tên bucket tài sản CDK
$BUCKET_NAME = aws s3 ls | Select-String "cdk-" | ForEach-Object {
$_.ToString().Split()[-1]
}

# Làm trống thùng chứa tài sản CDK
aws s3 rm "s3://$BUCKET_NAME" --recursive

# Xóa ngăn xếp khởi động CDK
aws cloudformation delete-stack --stack-name CDKToolkit

# Chờ xóa
aws cloudformation wait stack-delete-complete --stack-name CDKToolkit

Write-Host "CDK Bootstrap deleted" -ForegroundColor Green
```

---

### Bước 7: Xác minh việc dọn dẹp hoàn tất

#### 7.1 Kiểm tra CloudFormation

```powershell
aws cloudformation list-stacks `
--stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
| Select-String "OJT"

# Không trả về giá trị nào
```

#### 7.2 Kiểm tra S3

```powershell
aws s3 ls | Select-String "ojt"

# Không trả về giá trị nào
```

#### 7.3 Kiểm tra Lambda

```powershell
aws lambda list-functions `
--query "Functions[?contains(FunctionName, 'OJT')].FunctionName"

# Không trả về giá trị nào
```

#### 7.4 Kiểm tra RDS

```powershell
aws rds describe-db-instances `
--query "DBInstances[?contains(DBInstanceIdentifier, 'ojt')].DBInstanceIdentifier"

# Phải trả về mảng trống
```

#### 7.5 Kiểm tra API Gateway

```powershell
aws apigateway get-rest-apis `
--query "items[?contains(name, 'OJT')].name"

# Phải trả về mảng trống
```

#### 7.6 Kiểm tra Cognito

```powershell
aws cognito-idp list-user-pools --max-results 10 `
--query "UserPools[?contains(Name, 'OJT')].Name"

# Phải trả về mảng trống
```

#### 7.7 Kiểm tra NAT Gateway

```powershell
aws ec2 describe-nat-gateways `
--filter "Name=state,Values=available" `
--query "NatGateways[].NatGatewayId"

# Không nên hiển thị Cổng NAT OJT
```

---

### Bước 8: Xóa Ảnh chụp nhanh RDS (Tùy chọn)

```powershell
# Liệt kê ảnh chụp nhanh RDS
aws rds describe-db-snapshots `
--query "DBSnapshots[?contains(DBSnapshotIdentifier, 'ojt')].DBSnapshotIdentifier"

# Xóa từng ảnh chụp nhanh
aws rds delete-db-snapshot --db-snapshot-identifier ojt-database-final-snapshot
```

---

### Bước 9: Xóa Nhóm Nhật ký CloudWatch (Tùy chọn)

```powershell
# Liệt kê nhóm nhật ký
aws logs describe-log-groups `
--log-group-name-prefix /aws/lambda/OJT `
--query "logGroups[].logGroupName"

# Xóa từng nhóm nhật ký
$logGroups = aws logs describe-log-groups `
--log-group-name-prefix /aws/lambda/OJT `
--query "logGroups[].logGroupName" `
--output text

foreach ($logGroup in $logGroups.Split()) {
Write-Host "Đang xóa nhóm nhật ký: $logGroup"

aws logs delete-log-group --log-group-name $logGroup
}
```

---

### Chi phí sau khi dọn dẹp

**Ngay lập tức:**
- Hầu hết tài nguyên: 0 đô la/tháng
- Ảnh chụp nhanh RDS: ~0,095 đô la/GB/tháng (nếu được giữ lại)

**Sau khi dọn dẹp hoàn toàn:**
- Tất cả: 0 đô la/tháng

---
### Khắc phục sự cố Dọn dẹp

#### Sự cố: Xóa Bucket S3 không thành công

```powershell
# Buộc xóa và xóa
aws s3 rb s3://bucket-name --force
```

#### Sự cố: Ngăn xếp CloudFormation bị kẹt trong DELETE_IN_PROGRESS

```powershell
# Kiểm tra sự kiện ngăn xếp xem có lỗi không
aws cloudformation describe-stack-events `
--stack-name OJT-NetworkStack `
--max-items 10

# Nếu bị kẹt, hãy đợi hoặc kiểm tra các phụ thuộc
```

#### Sự cố: Bảo vệ Xóa RDS đã được bật

```powershell
# Tắt bảo vệ xóa
aws rds modify-db-instance `
--db-instance-identifier ojt-database `
--no-deletion-protection `
--apply-immediately

# Chờ một vài phút, sau đó thử xóa lại
```

#### Sự cố: VPC có các phụ thuộc

```powershell
# Kiểm tra các ENI còn lại
aws ec2 describe-network-interfaces `
--filters "Name=vpc-id,Values=vpc-xxxxxxxx"

# Xóa thủ công bất kỳ ENI còn lại nào
aws ec2 delete-network-interface --network-interface-id eni-xxxxxxxx
```

#### Sự cố: Cổng NAT vẫn tồn tại

```powershell
# Xóa Cổng NAT thủ công
aws ec2 delete-nat-gateway --nat-gateway-id nat-xxxxxxxx

# Giải phóng Elastic IP
aws ec2 release-address --allocation-id eipalloc-xxxxxxxx
```

---

### Danh sách kiểm tra dọn dẹp

#### Ngăn xếp CDK
- [ ] Đã xóa MonitoringStack
- [ ] Đã xóa FrontendStack
- [ ] ApiStack đã xóa
- [ ] DatabaseStack đã xóa
- [ ] AuthStack đã xóa
- [ ] StorageStack đã xóa
- [ ] NetworkStack đã xóa

#### Tài nguyên AWS
- [ ] Tất cả các thùng S3 đã được làm trống và xóa
- [ ] Không còn hàm Lambda nào
- [ ] Không còn phiên bản RDS nào
- [ ] Không còn API Gateway nào
- [ ] Không còn Cognito User Pool nào
- [ ] Không còn NAT Gateway nào
- [ ] Không còn VPC nào

#### Dọn dẹp tùy chọn
- [ ] CDK Bootstrap đã xóa
- [ ] Ảnh chụp nhanh RDS đã xóa
- [ ] Nhóm nhật ký CloudWatch đã xóa
- [ ] Kho lưu trữ GitLab đã lưu trữ/xóa

#### Xác minh
- [ ] CloudFormation không hiển thị ngăn xếp OJT
- [ ] Thanh toán AWS cho thấy chi phí đang giảm
- [ ] Không có khoản phí bất ngờ nào

---

### Kết luận

Bạn đã hoàn thành OJT Thương mại điện tử Hội thảo! Bạn đã học được:

**Cơ sở hạ tầng dưới dạng Mã** với AWS CDK (TypeScript)
**Kiến trúc Không máy chủ** với Lambda và API Gateway
**Quản lý cơ sở dữ liệu RDS SQL Server**
**Thiết kế VPC** với các mạng con công khai/riêng tư/riêng biệt
**Lưu trữ S3** cho hình ảnh và tệp tĩnh
**CloudFront CDN** với Kiểm soát Truy cập Gốc
**Xác thực Cognito** (tùy chọn)
**Xác thực JWT** với Lambda tùy chỉnh
**Giám sát CloudWatch** với bảng điều khiển và cảnh báo
**Chiến lược Tối ưu hóa Chi phí** cho không máy chủ
**Kiểm soát phiên bản GitLab**
**Chiến lược Triển khai 2 Bước** (Cơ sở hạ tầng + Mã Lambda)

**Tổng số triển khai:**
- 7 ngăn xếp CDK
- 11 mô-đun Lambda (63 API)
- Cơ sở dữ liệu RDS SQL Server
- VPC với NAT Gateway
- Bộ chứa S3 để lưu trữ
- CloudFront CDN
- Giám sát CloudWatch
- Thương mại điện tử sẵn sàng cho sản xuất nền tảng

**Chi phí hàng tháng ước tính:** ~$44/tháng (đã tối ưu hóa)

Cảm ơn bạn đã hoàn thành hội thảo! 🎉