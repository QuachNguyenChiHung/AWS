---
title: "CDK Bootstrap"
date: 2025-12-01
weight: 3
chapter: false
pre: " <b> 5.03. </b> "
---

#### Tổng quan

AWS CDK Bootstrap chuẩn bị tài khoản AWS của bạn để triển khai các ứng dụng CDK bằng cách tạo các tài nguyên cơ sở hạ tầng thiết yếu. Đây là quy trình thiết lập một lần cho mỗi tổ hợp tài khoản/khu vực AWS, thiết lập nền tảng cho tất cả các triển khai CDK.

#### CDK Bootstrap tạo ra những gì?

Quy trình bootstrap cung cấp các tài nguyên AWS sau trong tài khoản của bạn:

**1. S3 Bucket (CDK Assets Bucket)**
- Lưu trữ các gói triển khai hàm Lambda
- Lưu trữ các mẫu CloudFormation
- Lưu trữ các tài nguyên tệp được tham chiếu bởi ngăn xếp CDK của bạn
- Mẫu đặt tên: `cdk-hnb659fds-assets-{ACCOUNT-ID}-{REGION}`
- Kích hoạt tính năng quản lý phiên bản để hỗ trợ khôi phục

**2. CloudFormation Stack (CDKToolkit)**
- Ngăn xếp cơ sở hạ tầng chính chứa tất cả các tài nguyên bootstrap
- Tên ngăn xếp: `CDKToolkit`
- Quản lý vòng đời của các tài nguyên bootstrap

**3. Vai trò IAM**
- `cdk-hnb659fds-cfn-exec-role`: Vai trò thực thi CloudFormation để triển khai các ngăn xếp
- `cdk-hnb659fds-deploy-role`: Vai trò được CDK CLI sử dụng trong quá trình triển khai
- `cdk-hnb659fds-file-publishing-role`: Vai trò để tải tài sản lên S3
- `cdk-hnb659fds-lookup-role`: Vai trò để đọc ngữ cảnh môi trường

**4. Tham số SSM**
- `/cdk-bootstrap/hnb659fds/version`: Lưu trữ số phiên bản bootstrap
- Được sử dụng để kiểm tra tính tương thích

#### Tổng quan về Ngăn xếp Cơ sở hạ tầng

Sau khi khởi động, dự án Thương mại điện tử OJT của chúng tôi sẽ triển khai nhiều ngăn xếp CDK được định nghĩa trong `OJT_infrastructure/lib/stacks`:

| Ngăn xếp | Tệp | Mô tả |
|-------|------|-------------|
| NetworkStack | network-stack.ts | VPC, Mạng con, Cổng NAT, Nhóm bảo mật |

| StorageStack | storage-stack.ts | Nhóm S3 (Hình ảnh, Nhật ký) |

| AuthStack | auth-stack.ts | Nhóm người dùng & Nhóm danh tính Cognito |

| DatabaseStack | database-stack.ts | RDS SQL Server + Trình quản lý bí mật |
| ApiStack | api-stack.ts | Cổng API + 11 Mô-đun Lambda |

| FrontendStack | frontend-stack.ts | S3 + CloudFront (OAC) |

MonitoringStack | monitoring-stack.ts | Bảng điều khiển & Báo động CloudWatch |

Các ngăn xếp này phụ thuộc vào cơ sở hạ tầng bootstrap để triển khai thành công.

---

### Hướng dẫn thực hành: Khởi động tài khoản AWS của bạn

#### Điều kiện tiên quyết

Trước khi bắt đầu thực hành này, hãy đảm bảo bạn đã:

- AWS CLI đã được cài đặt và cấu hình (xem mục 5.02)
- AWS CDK CLI đã được cài đặt (xem mục 5.02)
- Thông tin đăng nhập AWS hợp lệ đã được cấu hình
- Người dùng IAM có quyền `AdministratorAccess` hoặc tương đương
- Truy cập Terminal/PowerShell vào thư mục dự án

#### Bước 1: Điều hướng đến Thư mục Cơ sở hạ tầng

```bash
cd OJT_infrastructure
```

#### Bước 2: Lấy ID tài khoản AWS của bạn

```bash

aws sts get-caller-identity --query Account --output text

```bash
# Bootstrap CDK cho vùng ap-southeast-1
cdk bootstrap aws://YOUR_ACCOUNT_ID/ap-southeast-1

# Ví dụ:
cdk bootstrap aws://123456789012/ap-southeast-1
```

**Đầu ra dự kiến:**
```
Môi trường khởi tạo aws://123456789012/ap-southeast-1...
Môi trường aws://123456789012/ap-southeast-1 đã được khởi tạo.
```

![CDK Bootstrap Success]
*Ảnh chụp màn hình: Terminal hiển thị quá trình bootstrap CDK đã hoàn tất thành công*

#### Bước 4: Xác minh Bootstrap qua CLI

```bash
# Kiểm tra phiên bản bootstrap
aws ssm get-parameter \
--name /cdk-bootstrap/hnb659fds/version \
--region ap-southeast-1 \
--query Parameter.Value \
--output text

# Đầu ra dự kiến: Số phiên bản (ví dụ: 19)
```

![Bootstrap Verification CLI]
*Ảnh chụp màn hình: Terminal hiển thị quá trình xác minh phiên bản bootstrap*

---

### Bước 5: Xác minh trên AWS Console

Xác nhận tài nguyên trong AWS Management Console.

**5.1. Xác minh ngăn xếp CloudFormation**

1. Mở [Bảng điều khiển CloudFormation]
2. Tìm ngăn xếp có tên `CDKToolkit`
3. Trạng thái phải là `CREATE_COMPLETE`
4. Nhấp vào tab **Tài nguyên** để xem các tài nguyên đã tạo

![Ngăn xếp CloudFormation CDKToolkit]
*Ảnh chụp màn hình: Bảng điều khiển CloudFormation hiển thị ngăn xếp CDKToolkit*

**5.2. Xác minh thùng S3**

1. Mở [Bảng điều khiển S3](https://s3.console.aws.amazon.com/s3/buckets?region=ap-southeast-1)
2. Tìm thùng: `cdk-hnb659fds-assets-{ACCOUNT-ID}-ap-southeast-1`
3. Xác minh thuộc tính thùng:
- Phiên bản: Đã bật

![Bảng điều khiển S3 Bucket]
*Ảnh chụp màn hình: Bảng điều khiển S3 hiển thị thùng tài sản CDK*

**5.3. Xác minh Vai trò IAM**

1. Mở [Bảng điều khiển Vai trò IAM](https://console.aws.amazon.com/iam/home#/roles)
2. Tìm kiếm: `cdk-hnb659fds`
3. Xác minh các vai trò sau tồn tại:
- `cdk-hnb659fds-cfn-exec-role-{ACCOUNT-ID}-ap-southeast-1`
- `cdk-hnb659fds-deploy-role-{ACCOUNT-ID}-ap-southeast-1`
- `cdk-hnb659fds-file-publishing-role-{ACCOUNT-ID}-ap-southeast-1`
- `cdk-hnb659fds-lookup-role-{ACCOUNT-ID}-ap-southeast-1`

![Bảng điều khiển Vai trò IAM]
*Ảnh chụp màn hình: Bảng điều khiển IAM hiển thị Vai trò khởi động CDK*

---

### Tìm hiểu về Tài nguyên Bootstrap

#### Chi tiết về Thùng S3

Thùng tài sản CDK đóng vai trò là kho lưu trữ trung tâm cho tất cả các hiện vật triển khai:

- **Mục đích**: Lưu trữ các mẫu CloudFormation, các gói triển khai Lambda và các tài sản tĩnh
- **Vòng đời**: Duy trì xuyên suốt các lần triển khai, cho phép khả năng khôi phục
- **Bảo mật**: Được mã hóa khi không hoạt động, các chính sách thùng hạn chế quyền truy cập vào các vai trò được ủy quyền- **Đặt tên**: Đặt tên xác định dựa trên ID tài khoản và vùng

#### Chi tiết Vai trò IAM

**1. Vai trò Thực thi CloudFormation (`cfn-exec-role`)**
- Được CloudFormation sử dụng để tạo/cập nhật/xóa tài nguyên ngăn xếp
- Có quyền rộng để quản lý tài nguyên AWS
- Mối quan hệ tin cậy với dịch vụ CloudFormation

**2. Vai trò Triển khai (`deploy-role`)**
- Được CDK CLI sử dụng trong các thao tác `cdk deploy`
- Có thể đảm nhận vai trò thực thi CloudFormation
- Có quyền tải lên tài nguyên và khởi tạo triển khai

**3. Vai trò Xuất bản Tệp (`file-publishing-role`)**
- Tải các gói Lambda và tài nguyên lên S3
- Có quyền ghi S3 vào thùng tài nguyên

**4. Vai trò Tra cứu (`lookup-role`)**
- Đọc ngữ cảnh môi trường (VPC, mạng con, v.v.)
- Quyền chỉ đọc cho tra cứu tài nguyên
- Được sử dụng trong quá trình tổng hợp cho các truy vấn ngữ cảnh

---

### Phân tích Chi phí

#### Chi phí Tài nguyên Bootstrap

**Thiết lập một lần:**
- Tạo ngăn xếp CloudFormation: **Miễn phí**
- Tạo vai trò IAM: **Miễn phí**
- Tham số SSM: **Miễn phí**

**Chi phí Hàng tháng Liên tục:**

| Tài nguyên | Mức sử dụng | Chi phí (Ước tính) |
|--------|--------|------------------|
| Lưu trữ S3 | < 1 GB (thông thường) | 0,023 đô la/GB = ~0,02 đô la/tháng |
| Yêu cầu S3 | Tối thiểu (GET/PUT) | ~0,01 đô la/tháng |
| Vai trò IAM | Miễn phí | 0,00 đô la |

**Tổng chi phí ước tính:** ~$0,03/tháng (không đáng kể)

**Lưu ý:** Đối với các ứng dụng không máy chủ, chi phí vẫn ở mức tối thiểu vì chúng tôi chỉ lưu trữ các gói triển khai Lambda trong S3.

---

### Khắc phục sự cố

**Sự cố: Bootstrap không thành công do lỗi quyền**
```bash
# Đảm bảo người dùng IAM của bạn có quyền AdministratorAccess
aws iam list-attached-user-policies --user-name YOUR_USERNAME
```

**Sự cố: Vùng không khớp**
```bash
# Xác minh vùng mặc định của bạn
aws configure get region

# Vùng cụ thể của Bootstrap
cdk bootstrap aws://ACCOUNT_ID/ap-southeast-1
```

**Sự cố: Phiên bản CDK không khớp**
```bash
# Cập nhật CLI CDK
npm update -g aws-cdk

# Xác minh phiên bản
cdk --version
```

---

### Danh sách kiểm tra hoàn thành bài thực hành

Đảm bảo bạn đã hoàn thành tất cả các bước:

- [ ] Bước 1: Điều hướng đến thư mục OJT_infrastructure
- [ ] Bước 2: Lấy ID tài khoản AWS của bạn
- [ ] Bước 3: Thực thi lệnh khởi động CDK
- [ ] Bước 4: Xác minh phiên bản khởi động qua CLI
- [ ] Bước 5.1: Xác minh ngăn xếp CDKToolkit CloudFormation
- [ ] Bước 5.2: Xác minh thùng S3 chứa tài sản CDK
- [ ] Bước 5.3: Đã xác minh vai trò IAM

#### Tóm tắt

Trong bài thực hành này, bạn đã thành công:

1. Thực thi lệnh khởi động CDK cho tài khoản AWS của bạn (ap-southeast-1)
2. Tạo ngăn xếp CDKToolkit CloudFormation
3. Cung cấp thùng S3 chứa tài sản CDK để lưu trữ các gói Lambda
4. Tạo các vai trò IAM cần thiết cho việc triển khai CDK
5. Xác minh tất cả tài nguyên qua CLI và Bảng điều khiển AWS

Tài khoản AWS của bạn hiện đã sẵn sàng để triển khai các ứng dụng CDK không máy chủ, bao gồm các ngăn xếp cơ sở hạ tầng cho dự án Thương mại điện tử OJT.

#### Các bước tiếp theo

Tiến hành [Triển khai Cơ sở hạ tầng Lõi] để triển khai các ngăn xếp VPC, Cơ sở dữ liệu, Lưu trữ và Xác thực.