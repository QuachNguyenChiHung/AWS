---
title: "Worklog Tuần 6"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 6:

* Học và thực hành Amazon Cognito (User Pool, App Client, luồng xác thực).
* Tìm hiểu Spring Data JpaRepository cho CRUD, phương thức truy vấn và phân trang.
* Thảo luận và chốt các dịch vụ AWS sẽ dùng cho dự án cùng cả nhóm.
* Chẩn đoán và khắc phục lỗi HTTP 500 của API backend Spring Boot.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                  |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------------------------------------------------------------- |
| 2   | - Tìm hiểu Amazon Cognito cơ bản (User Pool, App Client, domain) <br> - Thử luồng đăng ký/đăng nhập bằng Hosted UI hoặc SDK <br> - Kiểm tra ID/Access token và xác minh chữ ký JWT             | 10/13/2025   | 10/13/2025      | https://docs.aws.amazon.com/cognito/                                                                              |
| 3   | - Triển khai Spring Data JpaRepository cho các entity chính <br> - Thêm phương thức truy vấn, Pageable và sắp xếp <br> - Viết unit test cho CRUD của repository                                | 10/14/2025   | 10/14/2025      | https://docs.spring.io/spring-data/jpa/reference/                                                                 |
| 4   | - Họp nhóm: chọn dịch vụ AWS cho dự án (Cognito, API Gateway/ALB, RDS for SQL Server, S3, CloudWatch) <br> - Phác thảo sơ đồ kiến trúc tổng quan và luồng dữ liệu                             | 10/15/2025   | 10/15/2025      | https://wa.aws.amazon.com/                                                                                         |
| 5   | - Tái hiện lỗi 500 của backend Spring Boot <br> - Thêm logging và xử lý ngoại lệ (@ControllerAdvice) <br> - Sửa các trường hợp null/validation ở service/repository, trả mã trạng thái đúng | 10/16/2025   | 10/16/2025      | https://docs.spring.io/spring-boot/reference/web/servlet.html#web.servlet.spring-mvc.exception-handling           |
| 6   | - Thêm integration test cho API quan trọng <br> - Kiểm tra phản hồi 2xx/4xx bằng Postman/newman <br> - Cập nhật README và health checks/monitoring                                            | 10/17/2025   | 10/17/2025      | https://spring.io/guides/gs/testing-web/                                                                          |


### Kết quả đạt được tuần 6:
* Amazon Cognito: thiết lập và xác thực luồng đăng nhập
  * Tạo User Pool và App Client; cấu hình chính sách mật khẩu và (tuỳ chọn) MFA
  * Thử đăng ký/đăng nhập bằng Hosted UI/SDK và lấy ID/Access token
  * Xác minh JWT qua JWKs endpoint; xác nhận scope và claims cho truy cập API

* Spring Data JPA: triển khai repository và cải thiện lớp truy cập dữ liệu
  * Xây dựng repository mở rộng JpaRepository với phương thức truy vấn suy luận và phân trang
  * Viết unit test cho CRUD, đảm bảo tính nhất quán giao dịch
  * Giảm boilerplate và cải thiện khả năng đọc của tầng dữ liệu

* Quyết định của nhóm về dịch vụ AWS cho dự án
  * Chọn Cognito cho xác thực, Amazon RDS for SQL Server cho dữ liệu quan hệ, S3 cho lưu trữ đối tượng
  * Cân nhắc API Gateway so với ALB cho định tuyến; lựa chọn dựa trên nhu cầu tích hợp hiện tại
  * Lập kế hoạch giám sát với CloudWatch và log có cấu trúc cho API

* Khắc phục lỗi HTTP 500 của backend Spring Boot và tăng cường xử lý lỗi
  * Xác định nguyên nhân gốc (xử lý null, validation, ngoại lệ chưa bắt) qua log và trace
  * Thêm xử lý ngoại lệ toàn cục với @ControllerAdvice và phản hồi lỗi nhất quán
  * Áp dụng kiểm tra đầu vào (@Valid) và ánh xạ mã trạng thái đúng với ResponseEntity
  * Tạo integration test để ngăn hồi quy; xác thực endpoint trả 2xx/4xx chính xác


