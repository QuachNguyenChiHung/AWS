---
title: "Cấu hình các Infrastructure Stack"
date: 2025-12-01
weight: 4
chapter: false
pre: " <b> 5.04. </b> "
---

# Cấu hình Cơ sở hạ tầng Stacks

## Tổng quan về Dự án Thương mại điện tử OJT

### Giới thiệu
OJT E-commerce là một nền tảng thương mại điện tử không máy chủ được xây dựng hoàn toàn trên nền tảng AWS Cloud. Dự án sử dụng kiến ​​trúc không máy chủ với các hàm Lambda thay thế cho nền tảng Spring Boot truyền thống, tận dụng các dịch vụ được quản lý của AWS để đảm bảo khả năng mở rộng, bảo mật và tối ưu hóa chi phí.

### Kiến trúc Hệ thống

Dự án được thiết kế theo **Kiến trúc 7 tầng** với các lớp rõ ràng:

```
┌─ ...�
│ Nền tảng Thương mại Điện tử OJT │
├───────────────────────────────────────────────────────────────┤
│ Lớp 1: Mạng (Nền tảng) │
│ └─ Ngăn xếp mạng: VPC, Mạng con, Cổng NAT, Bảo mật Nhóm│
├──────────────────────────────────────────────────────────────────┤
│ Lớp 2: Dữ liệu & Lưu trữ │
│ ├─ Ngăn xếp lưu trữ: S3 Bucket (Hình ảnh, Nhật ký) │
│ └─ Cơ sở dữ liệu: RDS SQL Server, Secrets Manager │
├───────────────────────────────────────────────────────────────────────┤
│ Lớp 3: Xác thực │
│ └─ Cơ sở dữ liệu xác thực: Cognito User Pool, Identity Pool │
├───────────────────────────────────────────────────────────────┤
│ Lớp 4: Logic ứng dụng và nghiệp vụ │
│ └─ API Stack: API Gateway, 11 Mô-đun Lambda │
├─────────────────────────────────────────────────────────────────┤
│ Lớp 5: Phân phối nội dung │
│ └─ Nền tảng giao diện người dùng: Lưu trữ tĩnh S3, CloudFront CDN │
├──────────────────────────────────────────────────────────────────┤
│ Lớp 6: Giám sát & Khả năng quan sát │
│ └─ Ngăn xếp giám sát: Bảng điều khiển CloudWatch, Cảnh báo │
└──────────────────────────────────────────────────────────────────────────────────────────────
```

### Công nghệ được sử dụng

#### Cơ sở hạ tầng dưới dạng mã
- **AWS CDK (TypeScript)**: Quản lý cơ sở hạ tầng bằng mã
- **CloudFormation**: Công cụ mẫu cơ bản cho CDK
- **Git**: Kiểm soát phiên bản cho mã cơ sở hạ tầng

#### Dịch vụ Backend
- **API Gateway REST API**: Điểm cuối API RESTful
- **Lambda Functions**: Điện toán không máy chủ cho logic nghiệp vụ
- **RDS SQL Server**: Cơ sở dữ liệu quan hệ
- **S3**: Lưu trữ đối tượng cho hình ảnh sản phẩm và giao diện người dùng
- **Secrets Manager**: Lưu trữ thông tin xác thực an toàn

#### Bảo mật & Xác thực
- **Cognito User Pool**: Xác thực & quản lý người dùng
- **JWT Authentication**: Xác thực dựa trên JWT tùy chỉnh trong Lambda
- **VPC**: Cô lập mạng với các mạng con công khai/riêng tư/cô lập
- **Security Groups**: Quy tắc tường lửa cho tài nguyên
- **IAM Roles & Policies**: Kiểm soát truy cập chi tiết

#### Phân phối nội dung & Mạng
- **CloudFront**: CDN với Kiểm soát truy cập gốc (OAC)
- **NAT Gateway**: Truy cập Internet cho các mạng con riêng tư
- **VPC Endpoints**: Kết nối riêng tư đến các dịch vụ AWS

#### Giám sát & Vận hành
- **Nhật ký CloudWatch**: Ghi nhật ký tập trung cho các hàm Lambda
- **Chỉ số CloudWatch**: Theo dõi chỉ số hiệu suất
- **Bảng điều khiển CloudWatch**: Hiển thị các chỉ số
- **Cảnh báo CloudWatch**: Cảnh báo giám sát theo thời gian thực

### Cấu trúc thư mục dự án

```
OJT/
├── OJT_infrastructure/ # Cơ sở hạ tầng CDK AWS
│ ├── bin/
│ │ └── infrastructure.ts # Điểm vào ứng dụng CDK
│ ├── lib/
│ │ └── stacks/ # Định nghĩa ngăn xếp
│ │ ├── network-stack.ts # VPC, Mạng con, Cổng NAT
│ │ ├── storage-stack.ts # S3 Buckets
│ │ ├── auth-stack.ts # Cognito User Pool
│ │ ├── database-stack.ts # RDS SQL Server
│ │ ├── api-stack.ts # API Gateway + Lambda
│ │ ├── frontend-stack.ts # S3 + CloudFront
│ │ └── monitoring-stack.ts # CloudWatch
│ ├── .env.example # Mẫu môi trường
│ ├── package.json # Các phụ thuộc của Node.js
│ └── tsconfig.json # Cấu hình TypeScript
│
├── OJT_lambda/ # Hàm Lambda (63 API)
│ ├── auth/ # Xác thực (4 hàm)
│ ├── products/ # Sản phẩm (12 hàm)
│ ├── product-details/ # Chi tiết sản phẩm (7 hàm)
│ ├── cart/ # Giỏ hàng (6 hàm)
│ ├── orders/ # Đơn hàng (9 hàm)
│ ├── categories/ # Danh mục (6 hàm)
│ ├── brands/ # Thương hiệu (5 hàm)
│ ├── banners/ # Banners (7 hàm)
│ ├── ratings/ # Đánh giá (3 hàm)
│ ├── users/ # Người dùng (3 chức năng)
│ ├── images/ # Images (1 chức năng)
│ └── shared/ # Tiện ích dùng chung
│
├── OJT_frontendDev/ # Giao diện người dùng (React + Vite)
│ ├── src/ # Mã nguồn
│ └── public/ # Tài nguyên tĩnh
│
└── database/ # Tập lệnh cơ sở dữ liệu
├── schema/ # Tệp lược đồ SQL
├── migrations/ #Tập lệnh di chuyển
└── seeds/ # Dữ liệu mẫu
```

---

## Tổng quan về Kiến trúc Stack

### 1. Network Stack (Thứ tự triển khai: 1)
**Mục đích**: Tạo VPC và cơ sở hạ tầng mạng

**Tài nguyên chính**:
- **VPC**: Khối CIDR 10.0.0.0/16
- **Mạng con công cộng**: 2 AZ - Tài nguyên kết nối Internet
- **Mạng con riêng tư**: 2 AZ - Chức năng Lambda, dịch vụ nội bộ
- **Mạng con biệt lập**: 2 AZ - Cơ sở dữ liệu RDS (không có internet)
- **Cổng NAT**: 1 phiên bản (tối ưu hóa chi phí)
- **Cổng Internet**: Truy cập internet VPC
- **Nhóm bảo mật**: Lambda SG, RDS SG
- **Điểm cuối VPC**: S3, Secrets Manager

**Chi phí ước tính**: ~$23/tháng (NAT Cổng)

---

### 2. Ngăn xếp lưu trữ (Thứ tự triển khai: 2)
**Mục đích**: Tạo thùng S3 cho hình ảnh và nhật ký

**Tài nguyên chính**:
- **Xô hình ảnh**: Lưu trữ hình ảnh sản phẩm
- Cho phép quản lý phiên bản
- Quy tắc vòng đời để tối ưu hóa chi phí
- **Xô nhật ký**: Nhật ký truy cập CloudFront
- Tự động xóa sau 90 ngày

**Chi phí ước tính**: ~$1-3/tháng

---

### 3. Ngăn xếp xác thực (Thứ tự triển khai: 2)
**Mục đích**: Xác thực người dùng bằng Cognito (Tùy chọn)

**Tài nguyên chính**:
- **Nhóm người dùng Cognito**: Đăng ký và xác thực người dùng
- Yêu cầu xác minh email
- Chính sách mật khẩu: 8+ ký tự, kết hợp chữ hoa và chữ thường, số, ký hiệu
- **Máy khách nhóm người dùng Cognito**: Xác thực giao diện người dùng
- **Nhóm danh tính Cognito**: Thông tin xác thực AWS cho người dùng đã xác thực

**Chi phí ước tính**: $0/tháng (Cấp miễn phí: 50.000 MAU)

> **Lưu ý**: Gói này là tùy chọn. Dự án cũng hỗ trợ xác thực JWT tùy chỉnh trong Lambda.

---

### 4. Database Stack (Thứ tự triển khai: 2)
**Mục đích**: Cơ sở dữ liệu SQL Server với Secrets Manager

**Tài nguyên chính**:
- **RDS SQL Server Express 2019**:
- Phiên bản: db.t3.micro (tối ưu hóa chi phí)
- Lưu trữ: 20 GB gp3 SSD
- Đa vùng truy cập (Multi-AZ): Đã tắt (dev/staging)
- Sao lưu: Lưu trữ 1 ngày
- **Secrets Manager**: Thông tin đăng nhập cơ sở dữ liệu
- Mật khẩu mạnh được tạo tự động
- Tùy chọn xoay vòng

**Chi phí ước tính**: ~$15/tháng (tối ưu hóa từ $54)

---

### 5. API Stack (Thứ tự triển khai: 3)
**Mục đích**: REST API với API Gateway và các hàm Lambda

**Tài nguyên chính**:
- **API REST Gateway**:
- 63 điểm cuối trên 11 mô-đun
- Hỗ trợ CORS
- Ghi nhật ký CloudWatch
- **Hàm Lambda** (11 module):
- Xác thực, Sản phẩm, Chi tiết sản phẩm, Giỏ hàng, Đơn hàng
- Danh mục, Thương hiệu, Biểu ngữ, Xếp hạng, Người dùng, Hình ảnh
- Thời gian chạy: Node.js 20.x
- Bộ nhớ: 128-512 MB (đã tối ưu hóa)
- VPC: Mạng con riêng

**Chi phí ước tính**: ~$2-5/tháng

---

### 6. Frontend Stack (Lệnh triển khai: 4)
**Mục đích**: Lưu trữ trang web tĩnh với CDN

**Tài nguyên chính**:
- **S3 Bucket**: Tệp build React
- **Phân phối CloudFront**:
- Kiểm soát truy cập gốc (OAC)
- Chỉ HTTPS
- Nén Gzip

**Chi phí ước tính**: ~$1-2/tháng

---

### 7. Monitoring Stack (Lệnh triển khai: 5)
**Mục đích**: Giám sát, ghi nhật ký và cảnh báo

**Tài nguyên chính**:
- **Bảng điều khiển CloudWatch**: Số liệu hệ thống Hiển thị hóa
- **Cảnh báo CloudWatch**:
- Lỗi API Gateway 5xx
- Lỗi Lambda
- Mức sử dụng CPU RDS
- **Nhóm Nhật ký CloudWatch**: Nhật ký hàm Lambda

**Chi phí ước tính**: ~$1-2/tháng

---

### Luồng Phụ thuộc Ngăn xếp

```
Ngăn xếp Mạng (VPC, Mạng con, Cổng NAT)
↓
┌─────────────────────────────────────────────┐
│ Ngăn xếp Lưu trữ Ngăn xếp Xác thực │
│ (S3 Buckets) (Cognito) │
│ │
│ Cơ sở dữ liệu Stack │
│ (RDS SQL Server) │
└──────────────────────────────────────────
↓
Ngăn xếp API (API Gateway + Lambda)
↓
Ngăn xếp Frontend (S3 + CloudFront)
↓
Ngăn xếp Giám sát (CloudWatch)
```

---

### Tổng chi phí ước tính

**Môi trường phát triển:**

| Dịch vụ | Chi phí/tháng | Ghi chú |
|---------|------------|-------|
| Cổng NAT | 23 đô la | 1 phiên bản |
| RDS SQL Server | 15 đô la | t3.micro |
| Lambda | 2 đô la | 11 mô-đun, 128MB |
| Lưu trữ S3 | 1,25 đô la | Hình ảnh + Giao diện người dùng |
| CloudFront | 1,50 đô la | Phân phối CDN |
| CloudWatch | 1,50 đô la | Bảng điều khiển + Nhật ký |
| Cognito | 0 đô la | Gói miễn phí (50.000 người dùng hoạt động hằng tháng) |
| Cổng API | 0-3 đô la | Gói miễn phí (1 triệu yêu cầu) |
| **TỔNG CỘNG** | **~44 đô la/tháng** | Giảm 60% từ 111 đô la |

---

## Hướng dẫn cấu hình

### Bước 1: Điều hướng đến Thư mục Cơ sở hạ tầng

```powershell
cd OJT_infrastructure
```

### Bước 2: Cài đặt các Dependencies

```powershell
npm install
```

### Bước 3: Cấu hình Biến Môi trường

**1. Sao chép mẫu môi trường**

```powershell
sao chép .env.example .env
```

**2. Chỉnh sửa tệp .env với các giá trị của bạn**

```bash
# Cấu hình AWS
AWS_ACCOUNT_ID=123456789012
AWS_REGION=ap-southeast-1

# Cấu hình Cơ sở dữ liệu
DB_NAME=demoaws
DB_USERNAME=admin
DB_PASSWORD=YourSecurePassword123!

# Cấu hình ứng dụng
APP_NAME=OJT-Ecommerce
ENVIRONMENT=dev

# Bí mật JWT
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
```

**3. Xác minh ID tài khoản AWS**

```powershell
aws sts get-caller-identity
```

Đầu ra:
```json
{
"UserId": "AIDAXXXXXXXXXXXXXXXXXX",
"Account": "123456789012",
"Arn": "arn:aws:iam::123456789012:user/your-username"
}
```

### Bước 4:Xác thực Cấu hình

**1. Biên dịch TypeScript**

```powershell
npm run build
```

**2. Liệt kê tất cả các ngăn xếp CDK**

```powershell
npx cdk list
```

Đầu ra dự kiến:
```
OJT-NetworkStack
OJT-StorageStack
OJT-AuthStack
OJT-DatabaseStack
OJT-ApiStack
OJT-FrontendStack
OJT-MonitoringStack
```

**3. Tổng hợp các mẫu CloudFormation**

```powershell
npx cdk synth
```

Thư mục `cdk.out/` được tạo bằng các mẫu CloudFormation.

### Bước 5: Xem lại Mã Stack (Tùy chọn)

**Network Stack** (`lib/stacks/network-stack.ts`):
- VPC với 10.0.0.0/16 CIDR
- Mạng con Công khai, Riêng tư, Riêng biệt
- 1 NAT Gateway (tối ưu hóa chi phí)
- Nhóm Bảo mật cho Lambda và RDS

**Database Stack** (`lib/stacks/database-stack.ts`):
- RDS SQL Server Express 2019
- Phiên bản db.t3.micro (tối ưu hóa chi phí)
- Secrets Manager cho thông tin đăng nhập
- Lưu trữ bản sao lưu trong 1 ngày

**API Stack** (`lib/stacks/api-stack.ts`):
- API REST của API Gateway
- 11 mô-đun Lambda với mã giữ chỗ
- Tích hợp VPC để truy cập cơ sở dữ liệu

---

## Danh sách Kiểm tra Cấu hình

Trước khi triển khai, hãy xác minh những điều sau:

- [ ] **Cấu hình Môi trường**
- [ ] ID Tài khoản AWS đã được xác minh và cập nhật trong `.env`
- [ ] Vùng được đặt thành `ap-southeast-1`
- [ ] Cấu hình thông tin xác thực cơ sở dữ liệu
- [ ] Cấu hình bí mật JWT

- [ ] **Phụ thuộc**
- [ ] Đã cài đặt Node.js 20.x
- [ ] Đã cấu hình AWS CLI với thông tin xác thực
- [ ] AWS CDK CLI được cài đặt toàn cục
- [ ] Đã cài đặt các phụ thuộc npm (`npm install`)

- [ ] **Xác thực**
- [ ] Biên dịch TypeScript thành công (`npm run build`)
- [ ] Danh sách CDK hiển thị tất cả 7 ngăn xếp
- [ ] CDK synth tạo mẫu CloudFormation

- [ ] **Chuẩn bị**
- [ ] CDK bootstrap hoàn tất
- [ ] Tài khoản AWS có quyền AdministratorAccess

---

## Các bước tiếp theo

Sau khi hoàn tất cấu hình và xác thực, hãy tiếp tục:

- Triển khai tất cả các ngăn xếp lên AWS

Trong bước tiếp theo, bạn sẽ:
1. Triển khai Network Stack (VPC, Mạng con, Cổng NAT)
2. Triển khai Ngăn xếp Lưu trữ (S3 Bucket)
3. Triển khai Ngăn xếp Xác thực (Cognito - tùy chọn)
4. Triển khai Ngăn xếp Cơ sở dữ liệu (RDS SQL Server)
5. Triển khai Ngăn xếp API (API Gateway + Lambda)
6. Triển khai Ngăn xếp Giao diện Người dùng (S3 + CloudFront)
7. Triển khai Ngăn xếp Giám sát (CloudWatch)