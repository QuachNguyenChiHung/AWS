---
title: "Worklog Tuần 12"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.12 </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu Tuần 12:

* Rà soát toàn bộ dự án và sửa các lỗi trước khi trình bày với người hướng dẫn.
* Triển khai backend, cơ sở dữ liệu và frontend lên AWS (staging và production nếu phù hợp) với các pipeline CI/CD tái sử dụng được.
* Chuẩn bị demo, runbook và giám sát để người hướng dẫn có thể xem hệ thống ổn định và có khả năng quan sát.

### Các công việc cần triển khai trong tuần này:
| Ngày | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - Rà soát mã nguồn và phân loại lỗi: chạy phân tích tĩnh, ưu tiên lỗi critical/major, lên kế hoạch sửa lỗi                                                                                              | 11/24/2025 | 11/24/2025      |                                         |
| 2   | - Sửa các lỗi backend quan trọng: chạy unit & integration tests, tối ưu truy vấn cơ sở dữ liệu, xử lý các đường lỗi và trường hợp biên                                                                    | 11/25/2025 | 11/25/2025      |                                         |
| 3   | - Chuẩn bị artifact để deploy: containerize dịch vụ, build artifact, kiểm tra template hạ tầng (CloudFormation/CDK), và cấu hình pipeline CI cho deploy tới staging                                           | 11/26/2025 | 11/26/2025      |                                         |
| 4   | - Triển khai backend & cơ sở dữ liệu lên AWS staging (hoặc production nếu sẵn sàng): cấu hình Secrets Manager/SSM, load balancer/ALB, autoscaling, backup; xác thực kết nối và migration                    | 11/27/2025 | 11/27/2025      |                                         |
| 5   | - Triển khai frontend (S3 + CloudFront hoặc hosting tương đương), chạy end-to-end smoke tests, kiểm tra hiệu năng và tải, chuẩn bị kịch bản demo và diễn tập cho đội                                   | 11/28/2025 | 11/28/2025      |                                         |


### Kết quả đạt được Tuần 12:

* Hoàn thành rà soát mã nguồn và sửa các lỗi critical/major nhằm chuẩn bị cho buổi trình bày.

* Backend và cơ sở dữ liệu đã được triển khai lên AWS staging (và production nếu đã được phê duyệt)
  * Pipeline CI/CD đã được kiểm chứng cho build và deploy tự động tới môi trường staging
  * Các migration cơ sở dữ liệu đã được áp dụng và backup được cấu hình

* Frontend đã được triển khai và cấu hình phía CDN (S3 + CloudFront) cùng TLS và routing

* Giám sát và khả năng quan sát được thiết lập
  * Logs được tập trung, dashboards cơ bản và các cảnh báo cho lỗi quan trọng đã được cấu hình

* Đã thực hiện smoke tests end-to-end và diễn tập demo cho người hướng dẫn
  * Chuẩn bị kịch bản demo, runbook và các bước rollback

* Dự án sẵn sàng để trình bày với tài liệu triển khai và kết quả kiểm thử đi kèm.


