---
title: "Worklog Tuần 10"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 10:

* Triển khai các thao tác CRUD phía người dùng: sửa hồ sơ người dùng, thêm/xoá đơn hàng.
* Tổ chức họp nhóm tại Phuc Long để thảo luận dự án và phương án kết nối Chatbot từ AWS Bedrock vào backend.
* Kiểm thử và triển khai các thay đổi lên môi trường staging.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                              |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------- |
| 2   | - Thêm chức năng sửa hồ sơ người dùng (avatar, địa chỉ, thông tin liên hệ) <br> - Kiểm thử frontend và API chỉnh sửa hồ sơ <br> - Viết unit test cho endpoint cập nhật       | 10/11/2025   | 10/11/2025      |                                                           |
| 3   | - Thêm chức năng tạo/huỷ đơn hàng ở phía người dùng <br> - Triển khai logic validate đơn hàng và kiểm tra edge cases <br> - Viết integration test cho order APIs     | 10/12/2025   | 10/12/2025      |                                                           |
| 4   | - Họp nhóm tại Phuc Long: thảo luận tích hợp Chatbot (AWS Bedrock) với backend <br> - Phác thảo API, flow xác thực và bảo mật cho Chatbot <br> - Ghi nhận nhiệm vụ và timeline | 10/13/2025   | 10/13/2025      | https://aws.amazon.com/bedrock/                              |
| 5   | - Tích hợp thử nghiệm Chatbot với backend (mock/stub) <br> - Kiểm thử end-to-end luồng chat tới backend <br> - Chuẩn bị tài liệu hướng dẫn tích hợp cho nhóm     | 10/14/2025   | 10/14/2025      | https://docs.aws.amazon.com/                                  |



### Kết quả đạt được tuần 10:

* Thêm chức năng CRUD phía người dùng thành công
  * Triển khai chức năng sửa hồ sơ người dùng (cập nhật avatar, địa chỉ, thông tin liên hệ)
  * Thêm/huỷ đơn hàng từ giao diện người dùng và đảm bảo các ràng buộc nghiệp vụ
  * Viết unit và integration test để đảm bảo chất lượng API

* Họp nhóm tại Phuc Long và lên kế hoạch tích hợp Chatbot (AWS Bedrock)
  * Thảo luận kiến trúc tích hợp Chatbot với backend, xác định điểm tích hợp và cơ chế xác thực
  * Phác thảo luồng dữ liệu và API cần thiết cho tương tác Chatbot
  * Ghi nhận nhiệm vụ và timeline cho việc tích hợp

* Kiểm thử tích hợp cơ bản và tài liệu hoá
  * Thực hiện thử nghiệm mock Chatbot kết nối tới backend và kiểm tra luồng end-to-end
  * Chuẩn bị hướng dẫn tích hợp và ví dụ code mẫu cho nhóm

* Cải thiện trải nghiệm người dùng và khả năng triển khai
  * Nâng cao khả năng quản lý đơn hàng trực tiếp từ giao diện người dùng
  * Tạo nền tảng tích hợp Chatbot để hỗ trợ mua sắm và tự động hoá tương tác khách hàng


