---
title: "Worklog Tuần 7"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 7:

* Di chuyển cơ sở dữ liệu dự án từ SQL Server sang DynamoDB do chi phí vận hành cao.
* Ôn tập dịch vụ AWS và kiến trúc trong chương trình OJT.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                  |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------------------------------------------------------------- |
| 2   | - Phân tích chi phí vận hành SQL Server và các chỉ số hiệu năng <br> - Nghiên cứu mô hình giá DynamoDB và so sánh chi phí <br> - Quyết định chuyển sang DynamoDB và ghi nhận lý do | 10/20/2025   | 10/20/2025      | https://aws.amazon.com/dynamodb/pricing/                                                                       |
| 3   | - Thiết kế cấu trúc bảng DynamoDB (partition/sort keys, GSI) <br> - Lập kế hoạch di chuyển dữ liệu từ SQL Server sang DynamoDB <br> - Tạo bảng DynamoDB trong AWS console     | 10/21/2025   | 10/21/2025      | https://docs.aws.amazon.com/dynamodb/                                                                          |
| 4   | - Triển khai script/tool di chuyển dữ liệu (AWS DMS hoặc tùy chỉnh) <br> - Thực hiện di chuyển và xác thực tính toàn vẹn dữ liệu <br> - Cập nhật code ứng dụng để sử dụng DynamoDB SDK | 10/22/2025   | 10/22/2025      | https://docs.aws.amazon.com/dynamodb/latest/developerguide/                                                   |
| 5   | - Ôn tập tất cả dịch vụ AWS sử dụng trong dự án (Cognito, API Gateway, S3, CloudWatch) <br> - Phân tích kiến trúc hiện tại và xác định cơ hội tối ưu hóa <br> - Tham gia các buổi OJT và đào tạo thực hành     | 10/23/2025   | 10/23/2025      | https://aws.amazon.com/architecture/well-architected-framework/                                               |
| 6   | - Thực hiện ôn tập kiến trúc cuối cùng với nhóm <br> - Luyện tập tình huống OJT và bài tập thực hành <br> - Chuẩn bị tài liệu OJT và phản hồi                                     | 10/24/2025   | 10/24/2025      | https://aws.amazon.com/certification/                                                                          |


### Kết quả đạt được tuần 7:

* Thành công di chuyển cơ sở dữ liệu dự án từ SQL Server sang DynamoDB
  * Phân tích chi phí SQL Server và xác định tiết kiệm đáng kể với giá theo nhu cầu của DynamoDB
  * Thiết kế bảng DynamoDB với partition/sort keys và Global Secondary Indexes phù hợp
  * Thực hiện di chuyển dữ liệu bằng AWS Database Migration Service, đảm bảo 100% tính toàn vẹn dữ liệu
  * Cập nhật code ứng dụng để sử dụng DynamoDB SDK, đạt chuyển đổi liền mạch

* Ôn tập toàn diện dịch vụ AWS và kiến trúc
  * Đánh giá việc sử dụng dịch vụ AWS hiện tại (Cognito cho xác thực, API Gateway cho định tuyến, S3 cho lưu trữ, CloudWatch cho giám sát)
  * Xác định cơ hội tối ưu hóa kiến trúc bao gồm reserved instances và auto-scaling
  * Tham gia tích cực vào chương trình OJT với đào tạo thực hành và bài tập thực tế
  * Phát triển hiểu biết vững chắc về dịch vụ AWS thông qua ứng dụng thực tế

* Tối ưu hóa chi phí và cải thiện kiến trúc
  * Đạt tiết kiệm chi phí ước tính 60-70% bằng cách chuyển sang DynamoDB từ SQL Server
  * Nâng cao khả năng mở rộng và hiệu năng hệ thống với thiết kế cơ sở dữ liệu NoSQL
  * Tăng cường hiểu biết về các nguyên tắc AWS Well-Architected Framework
  * Cải thiện khả năng của nhóm trong việc đưa ra quyết định dựa trên dữ liệu cho hạ tầng đám mây


