---
title: "Workshop"
date: 2025-12-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng Furious Five Fashion: Workshop Hạ tầng AWS Full-Stack

#### Tổng quan

Kiến trúc của hệ thống được xây dựng theo mô hình full-stack serverless trên AWS, tập trung vào khả năng mở rộng tự động, bảo mật nhiều lớp và tối ưu chi phí. Toàn bộ các thành phần frontend – backend – data – AI – security đều hoạt động trên môi trường private, kết nối thông qua VPC, PrivateLink và các dịch vụ quản lý của AWS

Bạn sẽ triển khai bảy CDK stack liên kết với nhau để tạo ra một ứng dụng có khả năng mở rộng, bảo mật và tối ưu chi phí:
Frontend Layer – Triển khai giao diện trên Amplify và phân phối nội dung qua CloudFront.

* Routing & Protection – Bảo vệ truy cập bằng Route 53, WAF và chứng chỉ SSL của ACM.

* Authentication Layer – Tạo Cognito User Pool và tích hợp xác thực cho API Gateway.

* API Layer – Xây dựng API Gateway private để giao tiếp an toàn với backend.

* Compute Layer – Chạy logic nghiệp vụ bằng các Lambda functions bên trong private VPC.

* Storage Layer – Lưu trữ dữ liệu tĩnh và uploads trên S3 thông qua VPC Endpoint.

* Data Layer – Vận hành RDS trong private subnet và kiểm soát truy cập bằng IAM/SG.

* AI Layer – Tích hợp Amazon Bedrock để xử lý các tác vụ AI thông qua PrivateLink.

* Security & Observability – Theo dõi toàn hệ thống bằng CloudWatch, gửi cảnh báo qua SNS và quản lý bảo mật bằng IAM.

#### Nội dung

1. [Tổng quan Workshop](5.1-workshop-overview)
2. [Thiết lập Môi trường](5.2-setup-environment/)
3. [CDK Bootstrap](5.3-cdk-bootstrap/)
4. [Cấu hình Infrastructure Stacks](5.4-configure-stacks/)
5. [Triển khai Infrastructure](5.5-deploy-infrastructure/)
6. [Cấu hình API & Lambda](5.6-configure-api-lambda/)
7. [Triển khai Backend Services](5.7-deploy-backend/)
8. [Kiểm tra Endpoints End-to-End](5.8-test-endpoints/)
9. [Push lên GitLab](5.9-push-gitlab/)
10. [Dọn dẹp tài nguyên](5.11-cleanup/)
