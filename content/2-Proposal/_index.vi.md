---
title: "Bản đề xuất"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# Bán thời trang : FFF

### 1. Tóm tắt điều hành  
Giải pháp được đề xuất là nền tảng trang web bán thông tin phát triển khai trên AWS, tận dụng công nghệ serverless và AI để quản lý doanh nghiệp một hệ thống thương mại điện tử an toàn, ổn định, chi phí tối ưu và dễ dàng mở rộng.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Doanh nghiệp nhỏ hoặc startup thường không đủ ngân sách để xây dựng hạ tầng phức tạp.
Việc tự vận hành server truyền thống tốn kém (phần cứng, bảo trì, nhân sự vận hành).
Khi số lượng khách hàng tăng đột biến, hệ thống truyền thống dễ bị quá tải, gây gián đoạn dịch vụ.
Không có AI gợi ý sản phẩm, chatbot tư vấn, hoặc hệ thống dữ liệu hỗ trợ ra quyết định.
Khó theo dõi hành vi khách hàng, tối ưu bán hàng và marketing.

*Giải pháp*  
Kiến trúc serverless → tự động mở rộng, tối ưu chi phí, không cần quản lý hạ tầng phức tạp.
Đảm bảo tính an toàn, ổn định, khả dụng cao nhờ dịch vụ AWS bảo mật và giám sát.
Phù hợp cho doanh nghiệp vừa & nhỏ cần thử nghiệm thị trường trước khi đầu tư lớn.
Chỉ từ 50–100 USD/tháng, giúp giảm rủi ro tài chính.
AI Chatbot hỗ trợ khách hàng 24/7, giảm tải nhân viên tư vấn, tăng sự hài lòng, gợi ý sản phẩm giúp tăng tỷ lệ chuyển đổi và giá trị đơn hàng.


### 3. Kiến trúc giải pháp  

*Dịch vụ AWS sử dụng*  
#### 1. Frontend (Web + CDN)
- **Amazon S3** → Lưu trữ và host static web (Next.js build). Giúp giảm chi phí, không cần web server truyền thống.
- **Amazon CloudFront** → CDN phân phối nội dung toàn cầu, hỗ trợ SSL, giảm độ trễ tải trang.
- **Amazon Route 53** → Quản lý domain, DNS, routing traffic đến CloudFront/S3.

#### 2. Backend & API
- **Amazon API Gateway** → Cổng API nhận request từ frontend, điều phối đến Lambda/Fargate.
- **AWS Lambda** → Chạy code serverless (Node.js, Python...) cho API mà không cần server.
- **Amazon ECS Fargate** → Chạy container backend (nếu có service phức tạp, cần runtime dài hơn Lambda).
- **Amazon ECR** → Lưu trữ Docker image để ECS Fargate lấy và chạy.

#### 3. Database & Storage
- **Amazon RDS (PostgreSQL)** → Database quan hệ chính (lưu users, products, orders...).
- **Amazon DynamoDB** → NoSQL database để lưu session, metadata, cache nhẹ.
- **Amazon S3** → Lưu ảnh sản phẩm, hóa đơn, dữ liệu phân tích (data lake).
- **Amazon ElastiCache (Redis)** → Cache in-memory để tăng tốc đọc dữ liệu, giữ session tạm thời.

#### 4. Search & Events
- **Amazon OpenSearch** → Search engine cho sản phẩm, phân tích log.
- **Amazon SQS** → Queue để xử lý đơn hàng hoặc tác vụ async (giảm tải API chính).
- **Amazon SNS** → Gửi thông báo (email, SMS, push notification) khi có sự kiện.
- **Amazon EventBridge** → Event bus để kết nối service (ví dụ trigger AI pipeline khi có order mới).

#### 5. AI & Machine Learning
- **Amazon Personalize** → Tạo gợi ý sản phẩm cá nhân hóa cho từng khách hàng.
- **Amazon Lex** → Chatbot AI hỗ trợ khách hàng 24/7.
- **Amazon Comprehend** → Phân tích cảm xúc và từ khóa trong đánh giá khách hàng.
- **Amazon Fraud Detector** → Phát hiện giao dịch gian lận.
- **Amazon Forecast** → Dự báo tồn kho và nhu cầu bán hàng (triển khai giai đoạn sau).
- **Amazon SageMaker** → Train/deploy model ML tùy chỉnh nếu giải pháp AI mặc định không đủ.

#### 6. Data Lake & ETL
- **Amazon S3 (Data Lake)** → Lưu dữ liệu raw + processed để phân tích.
- **AWS Glue** → ETL job dọn dẹp và chuẩn hóa dữ liệu.
- **Amazon Athena** → Query dữ liệu trực tiếp trên S3 bằng SQL (serverless).
- **AWS Lake Formation** → Quản lý quyền truy cập dữ liệu trong data lake.
- **Amazon Kinesis** → Thu thập sự kiện, clickstream từ người dùng theo thời gian thực.

#### 7. Authentication & Security
- **Amazon Cognito** → Đăng ký, đăng nhập, quản lý user pool (SSO, OAuth2).
- **AWS IAM** → Quản lý role và quyền truy cập giữa các dịch vụ.
- **AWS KMS** → Mã hóa dữ liệu trong RDS, S3, DynamoDB.
- **AWS Secrets Manager** → Lưu API keys, mật khẩu DB, xoay vòng secret tự động.
- **AWS WAF** → Lọc request xấu, chống tấn công OWASP Top 10.
- **AWS Shield** → Bảo vệ khỏi tấn công DDoS.

#### 8. Observability & Monitoring
- **Amazon CloudWatch** → Thu thập log, metric, thiết lập cảnh báo.
- **AWS X-Ray** → Trace request end-to-end, tìm bottleneck.
- **Amazon Managed Grafana** → Dashboard trực quan hóa metric, logs.
- **OpenSearch Dashboards** → Giao diện tìm kiếm & phân tích log từ OpenSearch.
- **Amazon SNS** → Gửi cảnh báo khi có lỗi hoặc vượt ngưỡng.

#### 9. CI/CD & IaC
- **Terraform / AWS CDK** → IaC (Infrastructure as Code) để triển khai hạ tầng tự động.
- **GitHub Actions** → CI/CD pipeline: lint → test → build → deploy lên AWS.
- **Amazon CodeArtifact** (optional) → Quản lý private package repository.

#### 10. Backup & DR
- **Amazon RDS Snapshot** → Backup database tự động.
- **Amazon S3 Versioning + Cross-Region Replication** → Backup file và data lake sang vùng khác (DR).

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  

#### 1. Tech Lead / Architect

**Tuần 0–2**
- Thiết kế kiến trúc: VPC, RDS, S3, Cognito, OpenSearch.
- Viết tài liệu kiến trúc + sơ đồ mạng.
- Review Terraform modules.

**Tuần 3–12**
- Code review, approve PRs.
- Đảm bảo SLO: latency, error budget, cost.
- Kiểm thử DR drill, review runbooks.

✅ **Deliverables:**  
- Diagram, tài liệu kiến trúc  
- SLO/SLA  
- IAM model

---

#### 2. Frontend Engineer

**Tuần 1–2**
- Scaffold Vite project (JavaScript + Bootstrap).
- Tích hợp Cognito auth (login/signup).

**Tuần 3–6**
- Xây dựng UI: home, product list, product detail, cart, checkout.
- Upload hình ảnh qua S3 pre-signed URL.

**Tuần 7–9**
- Tích hợp chatbot widget (Lex).
- Tích hợp gợi ý sản phẩm từ Personalize.

**Tuần 10–12**
- E2E tests (Playwright).
- Tối ưu SEO + Lighthouse (>= 90).
- Deploy build → CloudFront.

✅ **Deliverables:**  
- Source code web  
- UI hoàn chỉnh  
- Test report  
- Build production

---

#### 3. Backend Engineer

**Tuần 1–3**
- Scaffold NestJS API, định nghĩa OpenAPI spec.
- Thiết kế DB schema (Postgres, Prisma migration).

**Tuần 4–6**
- API: sản phẩm, giỏ hàng, đơn hàng.
- Payment integration (stub).
- Upload media (S3).

**Tuần 7–9**
- Lambda consumer cho SQS (order events).
- API gợi ý (Personalize).
- Fraud Detector scoring API.

**Tuần 10–12**
- Hardening: rate limit, validation.
- Integration tests (Jest + Supertest).
- Blue/green deployment support.

✅ **Deliverables:**  
- OpenAPI docs  
- API tested  
- DB migrations  
- Docker image

---

#### 4. DevOps / SRE

**Tuần 0–2**
- Repo `/infra` + Terraform skeleton.
- Setup GitHub Actions (lint → test → plan → apply).
- State backend: S3 + DynamoDB lock.

**Tuần 3–6**
- Provision VPC, RDS, S3, Cognito, OpenSearch.
- Deploy staging environment.

**Tuần 7–9**
- Monitoring: Grafana dashboards.
- Alerts: SNS → Slack/email.
- Backup policies: RDS snapshots, S3 versioning.

**Tuần 10–12**
- DR drill (restore RDS, cross-region S3).
- Cost alerts, rightsizing.
- Prod cutover: Route53 switch.

✅ **Deliverables:**  
- Terraform repo  
- CI/CD pipelines  
- Monitoring dashboards  
- Runbooks

---

#### 5. Data / ML Engineer

**Tuần 1–3**
- Thiết kế schema event (click, view, purchase).
- Pipeline: Kinesis → S3 raw.
- Glue job ETL → Parquet.

**Tuần 4–6**
- Feed data vào Personalize.
- Train + deploy campaign.

**Tuần 7–9**
- Chatbot Lex intents: FAQ, tracking order, gợi ý sản phẩm.
- Fulfillment Lambda: query RDS, gọi Personalize API.

**Tuần 10–12**
- Forecast model cho tồn kho.
- Model Monitor (drift detection).
- Evaluation report.

✅ **Deliverables:**  
- ETL jobs + Parquet data  
- Personalize campaign + chatbot intents  
- Forecast model + monitoring  
- Báo cáo đánh giá


### 5. Lộ trình & Mốc triển khai  
- *Trước thực tập (Tháng 0)*: 1 tháng lên kế hoạch và đánh giá trạm cũ.  
- *Thực tập (Tháng 1–3)*:  
    - Tháng 1: Học AWS và nâng cấp phần cứng.  
    - Tháng 2: Thiết kế và điều chỉnh kiến trúc.  
    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  
Hoặc tải [tệp ước tính ngân sách](../attachments/budget_estimation.pdf).  

*Chi phí hạ tầng*  
- AWS Lambda: 0,00 USD/tháng (1.000 request, 512 MB lưu trữ).  
- S3 Standard: 0,15 USD/tháng (6 GB, 2.100 request, 1 GB quét).  
- Truyền dữ liệu: 0,02 USD/tháng (1 GB vào, 1 GB ra).  
- AWS Amplify: 0,35 USD/tháng (256 MB, request 500 ms).  
- Amazon API Gateway: 0,01 USD/tháng (2.000 request).  
- AWS Glue ETL Jobs: 0,02 USD/tháng (2 DPU).  
- AWS Glue Crawlers: 0,07 USD/tháng (1 crawler).  
- MQTT (IoT Core): 0,08 USD/tháng (5 thiết bị, 45.000 tin nhắn).  

*Tổng*: 0,7 USD/tháng, 8,40 USD/12 tháng  
- *Phần cứng*: 265 USD một lần (Raspberry Pi 5 và cảm biến).  

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Mất mạng: Ảnh hưởng trung bình, xác suất trung bình.  
- Hỏng cảm biến: Ảnh hưởng cao, xác suất thấp.  
- Vượt ngân sách: Ảnh hưởng trung bình, xác suất thấp.  

*Chiến lược giảm thiểu*  
- Mạng: Lưu trữ cục bộ trên Raspberry Pi với Docker.  
- Cảm biến: Kiểm tra định kỳ, dự phòng linh kiện.  
- Chi phí: Cảnh báo ngân sách AWS, tối ưu dịch vụ.  

*Kế hoạch dự phòng*  
- Quay lại thu thập thủ công nếu AWS gặp sự cố.  
- Sử dụng CloudFormation để khôi phục cấu hình liên quan đến chi phí.  

### 8. Kết quả kỳ vọng  
*Cải tiến kỹ thuật*: Dữ liệu và phân tích thời gian thực thay thế quy trình thủ công. Có thể mở rộng tới 10–15 trạm.  
*Giá trị dài hạn*: Nền tảng dữ liệu 1 năm cho nghiên cứu AI, có thể tái sử dụng cho các dự án tương lai.