---
title: "Worklog Tuần 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 2:

* Thảo luận và thống nhất ý tưởng/phạm vi dự án cùng nhóm.
* Lựa chọn ngôn ngữ lập trình/ngăn xếp công nghệ cho dự án.
* Học MS SQL cơ bản (cài đặt, lược đồ/bảng, CRUD, truy vấn đơn giản).
* Thực hành cài đặt dependency và thiết lập môi trường dự án.

### Các công việc cần triển khai trong tuần này:
| Ngày | Công việc                                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                             |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------ |
| 1    |<br> - Tổng quan bảo mật: bộ ba CIA, các mối đe dọa phổ biến, thuật ngữ cơ bản                                                                        | 09/15/2025   | 09/15/2025      |                                            |
| 2    | - Làm quen với các thành viên FCJ <br> - Đọc và ghi chú nội quy, quy định của đơn vị thực tập <br> - Thảo luận nhóm: ý tưởng & phạm vi dự án <br> - Chọn ngôn ngữ/ngăn xếp công nghệ <br> - Nền tảng bảo mật: xác thực vs ủy quyền, least privilege, MFA, thói quen mật khẩu | 09/16/2025   | 09/16/2025      |    |
| 3    | - Học các khái niệm bảo mật cốt lõi (Bảo mật mạng): <br>&emsp; + Kiến thức TCP/IP & cổng phổ biến <br>&emsp; + Cơ bản về tường lửa và VPN <br>&emsp; + Tổng quan IDS/IPS <br>&emsp; + Phân đoạn mạng                                       | 09/17/2025   | 09/17/2025      |     |
| 4    | - Công cụ & thực hành bảo mật: <br>&emsp; + Cơ bản hardening hệ điều hành (Windows/Linux) <br>&emsp; + Cấu hình tường lửa/antivirus cục bộ <br>&emsp; + Thiết lập trình quản lý mật khẩu <br> - **Thực hành (Java):** <br>&emsp; + Quản lý dependency an toàn với Maven/Gradle (pin version, kiểm tra checksum) <br>&emsp; + Khởi tạo khung xương dự án Java <br>&emsp; + Dùng application.properties/.yaml cho cấu hình và không commit bí mật (gitignore) | 09/18/2025   | 09/18/2025      |       |
| 5    | - Nền tảng bảo mật cơ sở dữ liệu: <br>&emsp; + Nguyên tắc đặc quyền tối thiểu (vai trò/quyền) <br>&emsp; + Phòng chống SQL injection (truy vấn tham số hóa) <br>&emsp; + Mã hóa khi truyền/tồn trữ cơ bản <br>&emsp; + Thiết kế ERD đơn giản có xét đến bảo mật | 09/19/2025   | 09/19/2025      |    |
| 6    | - **Thực hành (secure coding, Java):** <br>&emsp; + Thêm driver JDBC MS SQL (mssql-jdbc) và cấu hình DataSource <br>&emsp; + Thêm JPA/Hibernate hoặc MyBatis và hiện thực CRUD tham số hóa <br>&emsp; + Xác thực đầu vào (Jakarta Validation) & xử lý ngoại lệ <br>&emsp; + Quản lý bí mật (biến môi trường/thuộc tính hệ thống; không đưa bí mật vào mã nguồn) <br>&emsp; + Phân tích tĩnh/định dạng: SpotBugs, Checkstyle (hoặc SonarLint); ghi lại phát hiện | 09/19/2025   | 09/19/2025      |    |



### Kết quả đạt được tuần 2:

* Đã thống nhất ý tưởng/phạm vi dự án và chọn ngôn ngữ/ngăn xếp công nghệ (Java).

* Đã học MS SQL cơ bản:
  * Cài đặt và thiết lập môi trường (ví dụ: SQL Server/SSMS hoặc công cụ tương đương)
  * Tạo cơ sở dữ liệu và bảng
  * Viết truy vấn CRUD cơ bản và JOIN đơn giản

* Đã thực hành quản lý dependency cho Java; khởi tạo skeleton dự án; cấu hình kết nối MS SQL an toàn (chuỗi kết nối qua biến môi trường); thêm driver JDBC và ORM.

* Đã áp dụng các nguyên tắc bảo mật nền tảng: xác thực/ủy quyền, nguyên tắc đặc quyền tối thiểu, quản lý bí mật; chạy phân tích tĩnh với SpotBugs/Checkstyle/SonarLint và ghi nhận phát hiện.


