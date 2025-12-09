---
title: "Triển khai Backend Services"
date: 2025-12-01
weight: 7
chapter: false
pre: " <b> 5.07. </b> "
---

#### Tổng quan

Dự án Thương mại điện tử OJT sử dụng kiến ​​trúc **Triển khai 2 bước**: Cơ sở hạ tầng (CDK) và Mã Lambda (riêng biệt). Trong bước này, bạn sẽ triển khai cả cơ sở hạ tầng và mã Lambda.

#### Kiến trúc triển khai

```
┌─ ...�
│ Triển khai 2 bước │
├────────────────────────────────────────────────────────────────────┤
│ Bước 1: Triển khai Cơ sở hạ tầng (CDK) │
│ ├─ NetworkStack: VPC, Mạng con, Cổng NAT │
│ ├─ StorageStack: S3 Buckets │
│ ├─ AuthStack: Cognito (tùy chọn) │
│ ├─ DatabaseStack: RDS SQL Server │
│ ├─ ApiStack: API Gateway + Placeholder Lambda │
│ ├─ FrontendStack: S3 + CloudFront │
│ └─ MonitoringStack: CloudWatch │
├───────────────────────────────────────────────────────────────────┤
│ Bước 2: Triển khai mã Lambda │
│ └─ 11 Mô-đun Lambda (63 API) → Cập nhật mã hàm │
└─ ...
```

#### Mô-đun Lambda (11 mô-đun - 63 API)

| Mô-đun | Hàm | Mô tả |
|--------|-----------|-------------|
| Xác thực | 4 | Đăng nhập, Đăng ký, Đăng xuất, Tôi |
| Sản phẩm | 12 | CRUD, Tìm kiếm, Lọc |
| Chi tiết sản phẩm | 7 | CRUD, Hình ảnh |
| Giỏ hàng | 6 | Thêm, Nhận, Cập nhật, Xóa |
| Đơn hàng | 9 | CRUD, COD, Trạng thái |
| Danh mục | 6 | CRUD, Tìm kiếm |
| Thương hiệu | 5 | CRUD |
| Biểu ngữ | 7 | CRUD, Chuyển đổi |
| Xếp hạng | 3 | Nhận, Thống kê, Tạo |
| Người dùng | 3 | Lấy tất cả, Lấy theo ID, Cập nhật |
| Hình ảnh | 1 | Tải lên S3 |

---

### Bước 1: Triển khai Cơ sở hạ tầng (CDK)

#### 1.1 Điều hướng đến Thư mục Cơ sở hạ tầng

```powershell
cd OJT_infrastructure
```

#### 1.2 Cài đặt các Dependencies

```powershell
npm install
```

#### 1.3 Biên dịch TypeScript

```powershell
npm run build
```

#### 1.4 Triển khai Ngăn xếp Lõi

```powershell
# Triển khai Ngăn xếp Mạng, Lưu trữ, Xác thực, Cơ sở dữ liệu
npm run deploy:core
```

**Kết quả mong đợi:**
```
OJT-NetworkStack
OJT-StorageStack
OJT-AuthStack
OJT-DatabaseStack

Kết quả:
OJT-NetworkStack.VpcId = vpc-0123456789abcdef0
OJT-DatabaseStack.DbEndpoint = ojt-database.xxx.ap-southeast-1.rds.amazonaws.com
OJT-StorageStack.ImagesBucketName = ojt-ecommerce-images-123456789012
```

**Thời gian triển khai:** ~15-20 phút (RDS mất nhiều thời gian nhất)

*Ảnh chụp màn hình: CDK đang triển khai các ngăn xếp lõi*

#### 1.5 Triển khai Ngăn xếp API

```powershell
# Triển khai Cổng API + Lambda giữ chỗ
npm run deploy:api
```

**Đầu ra dự kiến:**
```
✅ OJT-ApiStack

Đầu ra:
OJT-ApiStack.ApiUrl = https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod
OJT-ApiStack.AuthModuleName = OJT-Ecommerce-AuthModule
OJT-ApiStack.ProductsModuleName = OJT-Ecommerce-ProductsModule
...
```

**Thời gian triển khai:** ~3-5 phút

*Ảnh chụp màn hình: CDK đang triển khai ngăn xếp API*

#### 1.6 Xác minh việc triển khai CDK

```powershell
# Liệt kê tất cả các ngăn xếp đã triển khai
aws cloudformation list-stacks `
--stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
--query "StackSummaries[?contains(StackName, 'OJT')].StackName" `
--output table
```

**Đầu ra dự kiến:**
```
---------------------------------
| ListStacks |
+---------------------------+
| OJT-NetworkStack |
| OJT-StorageStack |
| OJT-AuthStack |
| OJT-DatabaseStack |
| OJT-ApiStack |
+---------------------------+
```

---

### Bước 2: Triển khai mã Lambda

Sau khi triển khai CDK xong, các hàm Lambda có mã giữ chỗ. Bây giờ hãy triển khai mã thực tế.

#### 2.1 Điều hướng đến Thư mục Lambda

```powershell
cd ..\OJT_lambda
```

#### 2.2 Cài đặt các phụ thuộc

```powershell
# Cài đặt các phụ thuộc chính
npm install

# Cài đặt tất cả các phụ thuộc mô-đun
npm run install:all
```

#### 2.3 Cấu hình Môi trường

```powershell
# Sao chép mẫu môi trường
copy .env.example .env

# Chỉnh sửa .env với đầu ra CDK
notepad .env
```

**Cập nhật .env với các giá trị từ đầu ra CDK:**

```bash
# Cấu hình AWS
AWS_REGION=ap-southeast-1
AWS_ACCOUNT_ID=123456789012

# Cơ sở dữ liệu (từ OJT-DatabaseStack (đầu ra)
DB_HOST=ojt-database.xxx.ap-southeast-1.rds.amazonaws.com
DB_NAME=demoaws
DB_SECRET_ARN=arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:OJT/RDS/Credentials

# JWT
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d

# S3 (từ đầu ra OJT-StorageStack)
S3_IMAGES_BUCKET=ojt-ecommerce-images-123456789012
```

#### 2.4 Xây dựng các gói Lambda

```powershell
# Xây dựng tất cả các gói Lambda
npm run build
```

**Đầu ra dự kiến:**
```
Đang xây dựng xác thực module... Hoàn tất
Đang xây dựng module sản phẩm... Hoàn tất
Đang xây dựng module chi tiết sản phẩm... Hoàn tất
Đang xây dựng module giỏ hàng... Hoàn tất
Đang xây dựng module đơn hàng... Hoàn tất
Đang xây dựng module danh mục... Hoàn tất
Đang xây dựng module thương hiệu... Hoàn tất
Đang xây dựng module biểu ngữ... Hoàn tất
Đang xây dựng module xếp hạng... Hoàn tất
Đang xây dựng module người dùng... Hoàn tất
Đang xây dựng module hình ảnh... Hoàn tất

Đã hoàn tất xây dựng! Các tệp ZIP trong builthư mục d/.
```

**Bản dựng tạo:**
```
bản dựng/
├── auth.zip (~500 KB)
├── products.zip (~600 KB)
├── product-details.zip (~550 KB)
├── cart.zip (~450 KB)
├── orders.zip (~500 KB)
├── categories.zip (~400 KB)
├── brands.zip (~350 KB)
├── banners.zip (~400 KB)
├── ratings.zip (~350 KB)
├── users.zip (~400 KB)
└── images.zip (~300 KB)
```

#### 2.5 Triển khai mã Lambda

```powershell
# Triển khai tất cả các hàm Lambda
npm run deploy
```

**Kết quả mong đợi:**
```
Đang triển khai module auth vào OJT-Ecommerce-AuthModule... Hoàn tất
Đang triển khai module products vào OJT-Ecommerce-ProductsModule... Hoàn tất
Đang triển khai module product-details vào OJT-Ecommerce-ProductDetailsModule... Hoàn tất
Đang triển khai module cart vào OJT-Ecommerce-CartModule... Hoàn tất
Đang triển khai module orders vào OJT-Ecommerce-OrdersModule... Hoàn tất
Đang triển khai module categories vào OJT-Ecommerce-CategoriesModule... Hoàn tất
Đang triển khai module brands vào OJT-Ecommerce-BrandsModule... Hoàn tất
Đang triển khai module banners vào OJT-Ecommerce-BannersModule... Đã hoàn tất
Đang triển khai module ratings vào OJT-Ecommerce-RatingsModule... Đã hoàn tất
Đang triển khai module users vào OJT-Ecommerce-UsersModule... Đã hoàn tất
Đang triển khai module images vào OJT-Ecommerce-ImagesModule... Đã hoàn tất

Tất cả các hàm Lambda đã được triển khai thành công!
```

**Thời gian triển khai:** ~1-2 phút

*Ảnh chụp màn hình: Triển khai mã Lambda*

---

### Bước 3: Xác minh việc triển khai Lambda

#### 3.1 Liệt kê các hàm Lambda

```powershell
aws lambda list-functions `
--query "Functions[?contains(FunctionName, 'OJT-Ecommerce')].{Name:FunctionName,Runtime:Runtime,Updated:LastModified}" `
--output table
```

**Đầu ra dự kiến:**
```
--------------------------------------------------------------------
| ListFunctions |
+----------------------------------+------------+------------------+
| Name | Runtime | Updated |
+----------------------------------+------------+------------------+
| OJT-Ecommerce-AuthModule | nodejs20.x | 2025-12-09T... |

| OJT-Mô-đun Sản phẩm Thương mại Điện tử | nodejs20.x | 09/12/2025... |
| OJT-Mô-đun Chi tiết Sản phẩm Thương mại Điện tử | nodejs20.x | 09/12/2025... |
| OJT-Mô-đun Giỏ hàng Thương mại Điện tử | nodejs20.x | 09/12/2025... |
| OJT-Mô-đun Đơn hàng Thương mại Điện tử | nodejs20.x | 09/12/2025... |
| OJT-Mô-đun Danh mục Thương mại Điện tử | nodejs20.x | 09/12/2025... |
| OJT-Mô-đun Thương mại Điện tử Thương hiệu | nodejs20.x | 09/12/2025... |
| OJT-Mô-đun Biểu ngữ Thương mại Điện tử | nodejs20.x | 09/12/2025... |
| OJT-Ecommerce-RatingsModule | nodejs20.x | 09/12/2025... |
| OJT-Ecommerce-UsersModule | nodejs20.x | 09/12/2025... |
| OJT-Ecommerce-ImagesModule | nodejs20.x | 09/12/2025... |
+----------------------------------+------------+-------------------+
```

#### 3.2 Kiểm tra Cấu hình Hàm

```powershell
# Kiểm tra cấu hình Mô-đun Xác thực
aws lambda get-function-configuration `
--function-name OJT-Ecommerce-AuthModule `
--query '{Runtime:Runtime,Handler:Handler,Timeout:Timeout,Memory:MemorySize}'
```

**Đầu ra dự kiến:**
```json
{
"Runtime": "nodejs20.x",
"Handler": "index.handler",
"Timeout": 30,
"Memory": 128
}
```

#### 3.3 Xác minh Mã đã Cập nhật

```powershell
# Kiểm tra mã SHA256 (thay đổi khi mã cập nhật)
aws lambda get-function `
--function-name OJT-Ecommerce-AuthModule `
--query 'Configuration.CodeSha256'
```

---

### Bước 4: Kiểm tra các hàm Lambda

#### 4.1 Kiểm tra Đăng nhập Xác thực

```powershell
# Gọi Mô-đun Xác thực
aws lambda invoke `
--function-name OJT-Ecommerce-AuthModule `
--payload '{\"httpMethod\":\"POST\",\"path\":\"/auth/login\",\"body\":\"{\\\"email\\\":\\\"test@test.com\\\",\\\"password\\\":\\\"Test123!\\\"}\"}' `
response.json

# Kiểm tra phản hồi
Get-Content response.json
```

#### 4.2 Kiểm tra Danh sách Sản phẩm

```powershell
# Gọi Mô-đun Sản phẩm
aws lambda invoke `
--function-name OJT-Ecommerce-ProductsModule `
--payload '{\"httpMethod\":\"GET\",\"path\":\"/products\",\"queryStringParameters\":{\"page\":\"1\",\"limit\":\"10\"}}' `
response.json

# Kiểm tra phản hồi
Get-Content response.json
```

#### 4.3 Kiểm tra qua API Gateway

```powershell
# Lấy URL API
$apiUrl = aws cloudformation describe-stacks `
--stack-name OJT-ApiStack `
--query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' `
--output text

# Điểm cuối kiểm tra sản phẩm
Invoke-RestMethod -Uri "$apiUrl/products" -Method Get
```

---

### Bước 5: Kiểm tra CloudWatch Nhật ký

#### 5.1 Nhật ký Tail theo thời gian thực

```powershell
# Nhật ký Mô-đun Xác thực Tail
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --follow
```

#### 5.2 Xem Nhật ký Gần đây

```powershell
# Xem 10 phút gần nhất
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --since 10m
```

#### 5.3 Tìm kiếm Lỗi

```powershell
# Tìm kiếm lỗi
aws logs filter-log-events `
--log-group-name /aws/lambda/OJT-Ecommerce-AuthModule `
--filter-pattern "ERROR"
```

![CloudWatch Nhật ký](/images/5-Workshop/5.6-deploy-backend/cloudwatch-logs.png)*Ảnh chụp màn hình: Nhật ký CloudWatch hiển thị quá trình thực thi Lambda*

---

### Bước 6: Triển khai Frontend & Giám sát (Tùy chọn)

#### 6.1 Triển khai Ngăn xếp Frontend

```powershell
cd ..\OJT_infrastructure
npm run deploy:frontend
```

#### 6.2 Triển khai Ngăn xếp Giám sát

```powershell
npm run deploy:monitoring
```

---

### Tóm tắt Triển khai

### Ước tính Thời gian

| Bước | Thời gian | Ghi chú |
|------|------|-------|
| CDK Deploy Core | 15-20 phút | RDS mất nhiều thời gian nhất |
| CDK Deploy API | 3-5 phút | API Gateway + Lambda |
| Lambda Build | 1-2 phút | Gói ZIP |
| Lambda Deploy | 1-2 phút | Cập nhật mã chức năng |
| **Tổng cộng** | **~25 phút** | Lần triển khai đầu tiên |

#### Chỉ cập nhật mã Lambda

Khi chỉ thay đổi mã Lambda (không thay đổi cơ sở hạ tầng):

```powershell
cd OJT_lambda

# Build và triển khai (bỏ qua CDK)
npm run build
npm run deploy

# Thời gian: ~2-3 phút
```

---

### Danh sách kiểm tra triển khai

#### Cơ sở hạ tầng (CDK)
- [ ] NetworkStack đã triển khai (VPC, Mạng con, Cổng NAT)
- [ ] StorageStack đã triển khai (Gói S3)
- [ ] AuthStack đã triển khai (Cognito - tùy chọn)
- [ ] DatabaseStack đã triển khai (RDS SQL Server)
- [ ] ApiStack đã triển khai (API Gateway + Lambda giữ chỗ)

#### Mã Lambda
- [ ] Các dependency đã cài đặt (`npm install` + `npm run install:all`)
- [ ] Môi trường đã được cấu hình (tệp `.env`)
- [ ] Các gói Lambda đã được xây dựng (`npm run build`)
- [ ] Mã Lambda đã được triển khai (`npm run deploy`)

#### Xác minh
- [ ] Tất cả 11 hàm Lambda đã được liệt kê
- [ ] Thời gian chạy: nodejs20.x
- [ ] Mã SHA256 đã được cập nhật
- [ ] Đã gọi thử nghiệm thành công
- [ ] API Gateway đang phản hồi
- [ ] Nhật ký CloudWatch hiển thị các lần thực thi

---

### Khắc phục sự cố

#### Sự cố: Triển khai CDK không thành công

```powershell
# Kiểm tra thông tin xác thực AWS
aws sts get-caller-identity

# Kiểm tra phiên bản CDK
cdk --version

# Dọn dẹp và xây dựng lại
Remove-Item -Recurse -Force node_modules, cdk.out
npm install
npm run build
```

#### Sự cố: Triển khai Lambda không thành công

```powershell
# Kiểm tra sự tồn tại của hàm
aws lambda list-functions | Select-String "OJT-Ecommerce"

# Kiểm tra tệp ZIP đã tạo
Get-ChildItem build/*.zip

# Xây dựng lại và triển khai lại
npm run build
npm run deploy
```

#### Sự cố: Hết thời gian chờ của hàm

```powershell
# Tăng thời gian chờ
aws lambda update-function-configuration `
--function-name OJT-Ecommerce-AuthModule `
--timeout 60
```

#### Sự cố: Lỗi kết nối cơ sở dữ liệu

```powershell
# Xác minh điểm cuối RDS
aws rds describe-db-instances `
--db-instance-identifier ojt-database `
--query 'DBInstances[0].Endpoint'

# Kiểm tra Nhóm bảo mật cho phép Lambda
aws ec2 describe-security-groups `
--filters "Name=group-name,Values=*OJT*RDS*"
```

---

### Các bước tiếp theo

Backend đã được triển khai thành công! Tiếp theo:

1. **Test Endpoints**: Xác minh tất cả 63 API endpoints →

2. **Deploy Frontend**: Ứng dụng React →

3. **Monitor**: Bảng điều khiển CloudWatch →