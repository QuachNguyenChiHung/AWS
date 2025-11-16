---
title: "Worklog Tuần 9"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 9:

* Di chuyển dự án từ RDS SQL Server sang DynamoDB nhằm giảm chi phí vận hành.
* Hoàn thiện phần quản trị (admin) với các thao tác CRUD cơ bản cho cửa hàng bán quần áo.
* Học các lệnh cơ bản trên Linux để vận hành EC2 Ubuntu.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                 |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------ |
| 2   | - Phân tích và lập kế hoạch di chuyển từ RDS SQL Server sang DynamoDB <br> - So sánh mô hình chi phí và dự kiến tiết kiệm <br> - Ghi chép kế hoạch di chuyển và chiến lược rollback   | 11/03/2025   | 11/03/2025      | https://aws.amazon.com/dynamodb/pricing/                                       |
| 3   | - Thiết kế cấu trúc bảng DynamoDB (partition/sort keys, GSI) <br> - Triển khai script di chuyển dữ liệu (AWS DMS hoặc ETL tuỳ chỉnh) <br> - Bắt đầu di chuyển và xác thực mẫu dữ liệu | 11/04/2025   | 11/04/2025      | https://docs.aws.amazon.com/dynamodb/                                          |
| 4   | - Hoàn tất di chuyển và kiểm tra tính toàn vẹn dữ liệu <br> - Cập nhật backend để sử dụng DynamoDB SDK <br> - Giám sát hiệu năng và điều chỉnh capacity/index nếu cần         | 11/05/2025   | 11/05/2025      | https://docs.aws.amazon.com/dms/latest/userguide/                             |
| 5   | - Hoàn thành phần quản trị: triển khai CRUD cho sản phẩm, danh mục, đơn hàng <br> - Viết unit và integration test cho API quản trị <br> - Triển khai lên staging để kiểm thử | 11/06/2025   | 11/06/2025      | https://spring.io/guides/gs/accessing-data-jpa/                                |
| 6   | - Học và thực hành lệnh Linux cơ bản trên EC2 Ubuntu (ssh, systemctl, journalctl, apt, quyền file) <br> - Kiểm tra ứng dụng hoạt động trên instance và ghi chép lệnh thường dùng | 11/07/2025   | 11/07/2025      | https://linuxcommand.org/; https://help.ubuntu.com/community/UsingTheTerminal  |



### Kết quả đạt được tuần 9:

* Đã thành công di chuyển cơ sở dữ liệu dự án từ RDS SQL Server sang DynamoDB
  * Hoàn tất phân tích chi phí và xác nhận tiết kiệm vận hành đáng kể
  * Thiết kế schema DynamoDB và thực hiện di chuyển kèm kiểm tra xác thực dữ liệu
  * Cập nhật backend để sử dụng DynamoDB client; xác minh chức năng ứng dụng

* Hoàn thành phần quản trị với đầy đủ CRUD
  * Triển khai tạo/đọc/cập nhật/xoá cho sản phẩm, danh mục và đơn hàng
  * Thêm unit test và integration test cho API quản trị
  * Triển khai các thay đổi admin lên môi trường staging để kiểm thử

* Nâng cao kỹ năng Linux thực hành cho EC2 Ubuntu
  * Sử dụng SSH để truy cập instance và quản lý dịch vụ với `systemctl` và `journalctl`
  * Cài gói bằng `apt`, quản lý quyền file và kiểm tra log
  * Ghi chép các lệnh phổ biến để vận hành EC2 ổn định

* Cải thiện chi phí dự án và sẵn sàng triển khai
  * Việc chuyển sang DynamoDB cải thiện khả năng mở rộng và giảm chi phí dự kiến
  * Cải thiện phần quản trị và kỹ năng Linux giúp nhóm dễ dàng vận hành và bảo trì hệ thống


