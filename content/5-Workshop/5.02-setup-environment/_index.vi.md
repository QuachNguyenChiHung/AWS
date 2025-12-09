---
title: "Thiết lập Môi trường"
date: 2025-12-01
weight: 2
chapter: false
pre: " <b> 5.02. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ cài đặt tất cả các công cụ cần thiết để phát triển và triển khai ứng dụng Thương mại điện tử OJT.

#### Công cụ cần thiết

**1. Node.js 20.x**

```bash
# Tải xuống từ https://nodejs.org/
# Hoặc sử dụng nvm (khuyến nghị)
nvm install 20
nvm use 20

# Xác minh cài đặt
node --version # Phải là v20.x
npm --version
```

![Cài đặt Node.js]
*Ảnh chụp màn hình: Terminal hiển thị Node.js 20.x đã được cài đặt*

**2. AWS CLI v2**

```bash
# Windows: Tải xuống từ https://aws.amazon.com/cli/
# macOS: brew install awscli
# Linux:

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Xác minh cài đặt
aws --version
```

![Cài đặt AWS CLI]
*Ảnh chụp màn hình: Terminal hiển thị AWS CLI v2 đã được cài đặt*

**3. AWS CDK CLI**

```bash
# Cài đặt CDK toàn cục
npm install -g aws-cdk

# Xác minh cài đặt
cdk --version
```

![Cài đặt CDK CLI]
*Ảnh chụp màn hình: Terminal hiển thị CDK CLI đã được cài đặt*

**4. Git**

```bash
# Tải xuống từ https://git-scm.com/
# Hoặc sử dụng trình quản lý gói

# Xác minh cài đặt
git --version
```
![Cài đặt Git]

**5. Trình soạn thảo mã**

Khuyến nghị: Visual Studio Code với các tiện ích mở rộng:
- AWS Toolkit
- GitLab Workflow
- ESLint
- Prettier

#### Thiết lập tài khoản AWS

**1. Tạo tài khoản AWS**

Nếu bạn chưa có tài khoản AWS:
1. Truy cập https://aws.amazon.com/
2. Nhấp vào "Tạo tài khoản AWS"
3. Làm theo quy trình đăng ký
4. Thêm phương thức thanh toán

**2. Tạo người dùng IAM**

Vì lý do bảo mật, không sử dụng tài khoản root

1. Truy cập Bảng điều khiển IAM → Người dùng → Tạo người dùng
2. Tạo người dùng và lưu thông tin đăng nhập

![Người dùng IAM đã được tạo]
*Ảnh chụp màn hình: Bảng điều khiển IAM hiển thị người dùng được tạo bằng AdministratorAccess*

**3. Cấu hình AWS CLI**

```bash
# Cấu hình thông tin xác thực AWS
aws configure

# Nhập:
# ID Khóa Truy cập AWS: [Khóa Truy cập của Bạn]
# Khóa Truy cập Bí mật AWS: [Khóa Bí mật của Bạn]
# Tên vùng mặc định: ap-southeast-1
# Định dạng đầu ra mặc định: json
```

![Cấu hình AWS CLI]
*Ảnh chụp màn hình: Terminal hiển thị quá trình cấu hình aws đã hoàn tất*

**4. Xác minh Quyền Truy cập AWS**

```bash
# Kiểm tra thông tin xác thực AWS
aws sts get-caller-identity

# Sẽ trả về ID tài khoản và ARN người dùng của bạn
```
![Quyền Truy cập AWS đã được Xác minh]

#### Thiết lập Tên miền (Tùy chọn)

Nếu bạn muốn sử dụng tên miền tùy chỉnh:

**1. Đăng ký Tên miền**

Mua tên miền trên hpanel.hostinger
Tuyến đường 53 tạo bản ghi DNS cho hostinger
Trong hội thảo này, chúng tôi sử dụng: `yourdomain.com`

**2. Lưu ý Nhà đăng ký tên miền**

Bạn sẽ cần quyền truy cập vào nhà đăng ký tên miền để cập nhật máy chủ tên sau.

#### Thiết lập GitLab

**Tạo Kho lưu trữ GitLab**

**3. Cấu hình Git**

```bash
# Đặt tên và email của bạn
git config --global user.name "Tên của bạn"
git config --global user.email "your.email@example.com"

# Xác minh cấu hình
git config --list
```

#### Thiết lập Dự án

**1. Sao chép hoặc Tạo Dự án**

Tùy chọn A: Sao chép dự án hiện có
```bash
git clone https://gitlab.com/your-username/ojt-ecommerce.git
cd ojt-ecommerce
```

Tùy chọn B: Tạo dự án mới
```bash
mkdir OJT
cd OJT
git init
```

**2. Cấu trúc dự án**

Dự án Thương mại điện tử OJT có cấu trúc như sau:
```
OJT/
├── OJT_infrastructure/ # Cơ sở hạ tầng CDK (TypeScript)
│ └── Triển khai tài nguyên AWS: VPC, RDS, S3, API Gateway, v.v.
│
├── OJT_lambda/ # Hàm Lambda (JavaScript) - 63 API
│ └── Mã ứng dụng cho các điểm cuối API
│
├── OJT_frontendDev/ # Giao diện người dùng (React + Vite)
│ └── Ứng dụng web
│
└── database/ # Tập lệnh cơ sở dữ liệu (MySQL)
├── schema/ # Lược đồ chính
├── migrations/ # Tập lệnh di chuyển
└── seeds/ # Dữ liệu mẫu
```

**3. Cài đặt các phụ thuộc**

```bash
# Cài đặt các phụ thuộc của Cơ sở hạ tầng CDK
cd OJT_infrastructure
npm install

# Cài đặt các phụ thuộc Lambda
cd ../OJT_lambda
npm install
npm run install:all

# Cài đặt các phụ thuộc Frontend
cd ../OJT_frontendDev
npm install
```

**4. Sao chép Biến Môi trường**

Đối với Cơ sở hạ tầng CDK:
```bash
cd OJT_infrastructure
cp .env.example .env
```

Chỉnh sửa `.env` với các giá trị của bạn:
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

Đối với Lambda:
```bash
cd ../OJT_lambda
cp .env.example .env
```

Chỉnh sửa `.env` với các giá trị của bạn:
```bash
# Cấu hình AWS
AWS_REGION=ap-southeast-1
AWS_ACCOUNT_ID=your-account-id

# Cấu hình JWT
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d
```

#### Xác minh

Kiểm tra xem mọi thứ đã được cài đặt đúng chưa:

```bash
# Kiểm tra Node.js
node --version # v20.x

# Kiểm tra npm
npm --version # 10.x

# Kiểm tra AWS CLI
aws --version # aws-cli/2.x

# Kiểm tra CDK
cdk --version # 2.x

# Kiểm tra Git
git --version # 2.x

# Kiểm tra thông tin đăng nhập AWS
aws sts get-caller-identity

# Kiểm tra các phụ thuộc của dự án
cd OJT_infrastructure
npm list --depth=0
```

#### Khắc phục sự cố

**Sự cố: Phiên bản Node.js không khớpch**
```bash
# Sử dụng nvm để chuyển đổi phiên bản
nvm install 20
nvm use 20
```

**Sự cố: Không tìm thấy AWS CLI**
- Khởi động lại terminal sau khi cài đặt
- Kiểm tra biến môi trường PATH

**Sự cố: Không tìm thấy lệnh CDK**
```bash
# Cài đặt lại CDK trên toàn cục
npm uninstall -g aws-cdk
npm install -g aws-cdk
```

**Sự cố: Thông tin đăng nhập AWS không hợp lệ**
```bash
# Cấu hình lại AWS CLI
aws configure
# Nhập thông tin đăng nhập chính xác
```

#### Các bước tiếp theo

Sau khi thiết lập môi trường, hãy chuyển đến [CDK Bootstrap] để chuẩn bị tài khoản AWS của bạn cho việc triển khai CDK.