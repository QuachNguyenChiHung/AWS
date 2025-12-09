---
title: "Tổng quan Workshop"
date: 2025-12-01
weight: 1
chapter: false
pre: " <b> 5.01. </b> "
---

#### Kiến trúc Thương mại điện tử OJT

**OJT E-commerce** là một nền tảng thương mại điện tử không máy chủ hiện đại được xây dựng hoàn toàn trên AWS. Kiến trúc này tuân thủ các phương pháp hay nhất về khả năng mở rộng, bảo mật và tối ưu hóa chi phí, thay thế backend Spring Boot truyền thống bằng các hàm AWS Lambda.

#### Các thành phần chính

+ **Frontend**: Ứng dụng React + Vite được lưu trữ trên S3 với CloudFront CDN
+ **Backend**: API không máy chủ sử dụng API Gateway với 11 mô-đun Lambda (63 điểm cuối)
+ **Cơ sở dữ liệu**: RDS SQL Server Express 2019 trong mạng con riêng
+ **Lưu trữ**: Các thùng S3 cho hình ảnh và tệp tĩnh frontend
+ **Xác thực**: Xác thực dựa trên JWT với Cognito User Pool (tùy chọn)
+ **Bảo mật**: VPC với mạng con công khai/riêng tư, Nhóm bảo mật, Trình quản lý bí mật
+ **Giám sát**: Bảng điều khiển CloudWatch, Nhóm nhật ký và Cảnh báo

#### Sơ đồ kiến ​​trúc

<img src="/images/2-Proposal/OJT-SS5.drawio.png"/>

#### Quy trình hội thảo

Hội thảo này tuân theo quy trình phát triển ứng dụng thực tế:

1. **Thiết lập môi trường** - Cài đặt các công cụ (Node.js, AWS) CLI, CDK CLI)
2. **CDK Bootstrap** - Chuẩn bị tài khoản AWS cho triển khai CDK
3. **Triển khai Cơ sở hạ tầng Lõi** - VPC, RDS, S3, Cognito (NetworkStack, StorageStack, AuthStack, DatabaseStack)
4. **Triển khai API Stack** - API Gateway + Hàm Lambda giữ chỗ
5. **Triển khai Mã Lambda** - Triển khai mã hàm Lambda thực tế (63 API)
6. **Triển khai Giao diện Người dùng** - Xây dựng ứng dụng React và triển khai lên S3 + CloudFront
7. **Triển khai Giám sát** - Bảng điều khiển và Cảnh báo CloudWatch
8. **Kiểm tra Điểm cuối** - Xác minh tất cả 63 điểm cuối API hoạt động từ đầu đến cuối
9. **Giám sát & Bảo trì** - Sử dụng CloudWatch để giám sát và gỡ lỗi

#### Những điều bạn sẽ học

+ Cơ sở hạ tầng dưới dạng Mã với AWS CDK (TypeScript)
+ Kiến trúc không máy chủ thay thế Spring Boot bằng Lambda
+ Chiến lược triển khai 2 bước: Cơ sở hạ tầng + Phân tách mã Lambda
+ RDS SQL Máy chủ trong mạng con riêng với Secrets Manager
+ CloudFront CDN với Origin Access Control (OAC)
+ Xác thực JWT với tích hợp Cognito tùy chọn
+ Tổ chức mô-đun chức năng Lambda (11 mô-đun: Xác thực, Sản phẩm, Chi tiết sản phẩm, Giỏ hàng, Đơn hàng, Danh mục, Thương hiệu, Biểu ngữ, Xếp hạng, Người dùng, Hình ảnh)
+ Thiết kế VPC với NAT Gateway để truy cập internet mạng con riêng
+ Giám sát CloudWatch với Bảng điều khiển và Cảnh báo
+ Chiến lược tối ưu hóa chi phí cho môi trường phát triển

#### Ước tính chi phí thực tế

**Môi trường phát triển (Đã tối ưu hóa):**
- **Chi phí hàng tháng ước tính**: **44 đô la/tháng** (giảm 60% so với giá gốc 111 đô la/tháng)

**Chi tiết chi phí:**

| Dịch vụ | Cấu hình | Chi phí hàng tháng |
|---------|---------------|--------------|
| NAT Gateway | 1 phiên bản | 23 đô la |
| RDS SQL Server | t3.micro | 15 đô la |
| Lambda | 11 mô-đun, 128MB | 2 đô la |
| Lưu trữ S3 | Hình ảnh + Giao diện người dùng | 1,25 đô la |
| CloudFront | Phân phối CDN | 1,50 đô la |
| CloudWatch | Bảng điều khiển + Nhật ký | 1,50 đô la |
| **Tổng cộng** | | **~44 đô la/tháng** |

**Tối ưu hóa chi phí:**
+ Kích thước phiên bản RDS: t3.small → t3.micro (tiết kiệm 39 đô la/tháng)
+ Cổng NAT: 2 → 1 phiên bản (tiết kiệm 23 đô la/tháng)
+ Bộ nhớ Lambda: 256MB → 128MB (tiết kiệm 50% mỗi lần gọi)
+ Thời gian chờ Lambda: 30 giây → 10 giây (thực thi nhanh hơn)
+ Lưu giữ nhật ký: 7 ngày → 1 ngày (tiết kiệm 85% chi phí CloudWatch)
+ Lưu giữ bản sao lưu: 7 ngày → 1 ngày để phát triển

**Lợi ích của gói miễn phí (12 tháng đầu):**
+ Lambda: 1 triệu yêu cầu/tháng miễn phí
+ S3: 5GB dung lượng lưu trữ + 20.000 yêu cầu GET miễn phí
+ RDS: t3.micro 750 giờ/tháng miễn phí (một AZ)

#### Các tính năng chính

+ **Nền tảng thương mại điện tử**: Sản phẩm, Giỏ hàng, Đơn hàng, Danh mục, Thương hiệu
+ **63 Điểm cuối API**: Hoàn thiện các thao tác CRUD cho tất cả các mô-đun
+ **Tải lên hình ảnh**: Tích hợp S3 cho hình ảnh sản phẩm
+ **Tìm kiếm & Lọc**: Sản phẩm theo danh mục, thương hiệu, mức giá
+ **Quản lý đơn hàng**: Tạo, theo dõi và quản lý đơn hàng
+ **Hệ thống xếp hạng**: Xếp hạng và thống kê sản phẩm
+ **Quản lý biểu ngữ**: Biểu ngữ động cho các chương trình khuyến mãi
+ **Chức năng quản trị**: Quản lý người dùng, cập nhật trạng thái đơn hàng

#### Tóm tắt Mô-đun Lambda

| Mô-đun | Chức năng | Mô tả |
|--------|-----------|-------------|
| Xác thực | 4 | Đăng nhập, Đăng ký, Đăng xuất, Tôi |
| Sản phẩm | 12 | CRUD, Tìm kiếm, Lọc, Bán chạy nhất, Mới nhất |
| Chi tiết sản phẩm | 7 | CRUD, Tải lên hình ảnh |
| Giỏ hàng | 6 | Thêm, Nhận, Cập nhật, Xóa, Xóa, Đếm |
| Đơn hàng | 9 | CRUD, COD, Trạng thái, Bộ lọc theo khoảng thời gian |

| Danh mục | 6 | CRUD, Tìm kiếm |
| Thương hiệu | 5 | CRUD |
| Biểu ngữ | 7 | CRUD, Chuyển đổi |
| Xếp hạng | 3 | Lấy, Thống kê, Tạo |
| Người dùng | 3 | Lấy tất cả, Lấy theo ID, Cập nhật hồ sơ |
| Hình ảnh | 1 | Tải lên S3 |
| **Tổng cộng** | **63** | |

#### Chiến lược triển khai

**Quy trình triển khai 2 bước:**

1. **Triển khai Cơ sở hạ tầng (CDK)** - 5-10 phút
- VPC, Mạng con, Cổng NAT
- RDS SQL Server + Trình quản lý Bí mật
- S3 Buckets (Hình ảnh, Giao diện người dùng)
- API Gateway + Lambda giữ chỗ
- Cognito User Pool (tùy chọn)

2. **Triển khai Mã Lambda** - 1-2 phút
- Đóng gói các hàm Lambda với các phần phụ thuộc
- Tải lên AWS Lambda
- Cập nhật mã hàm độc lập

**Lợi ích của việc Tách biệt:**
+ Triển khai CDK nhanh hơn (không cần xây dựng mã Lambda)
+ Không cần phân tích phần phụ thuộclỗi giải pháp
+ Cập nhật mã Lambda độc lập (30 giây)
+ Phân tách rõ ràng: Mã cơ sở hạ tầng so với Mã ứng dụng
+ Triển khai thân thiện với CI/CD