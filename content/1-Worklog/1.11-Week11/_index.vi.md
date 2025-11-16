---
title: "Worklog Tuần 11"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}



### Mục tiêu Tuần 11:

* Thêm chức năng giao dịch bằng thẻ tín dụng vào dự án để người dùng có thể thanh toán an toàn.
* Tham dự AWS Cloud Mastery Series (Bitexco Financial Tower) và áp dụng kiến thức về DevOps/CI-CD và giám sát vào dự án.
* Hoàn thành tích hợp, kiểm thử và triển khai luồng thanh toán, đồng thời viết tài liệu triển khai.

### Công việc cần triển khai trong tuần này:
| Ngày | Công việc                                                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tham dự AWS Cloud Mastery Series tại Bitexco Financial Tower (cả ngày) <br> - Các phiên bao gồm: DevOps mindset, CI/CD (CodeCommit/CodeBuild/CodeDeploy/CodePipeline), IaC (CloudFormation/CDK), container (ECR/ECS/EKS), giám sát (CloudWatch/X-Ray) và best practices. | 11/17/2025   | 11/17/2025      |                                         |
| 3   | - Thiết kế và tích hợp luồng thanh toán bằng thẻ tín dụng (chọn nhà cung cấp/gateway/tokenization) <br> - Triển khai endpoint server-side xử lý thanh toán và lưu trữ token <br> - Đảm bảo không lưu trữ dữ liệu nhạy cảm dưới dạng text thuần | 11/18/2025   | 11/18/2025      |                                         |
| 4   | - Triển khai giao diện thanh toán phía client và kết nối tới API tokenization backend <br> - Thêm xác thực phía server, logging và xử lý retry/lỗi <br> - Thêm giám sát cho tỷ lệ thành công/thất bại thanh toán | 11/19/2025   | 11/19/2025      |                                         |
| 5   | - Kiểm thử end-to-end luồng thanh toán (thành công, từ chối, lỗi mạng) <br> - Chạy kiểm tra bảo mật và đảm bảo các cân nhắc tuân thủ (tokenization, TLS) <br> - Sửa lỗi phát hiện trong quá trình test | 11/20/2025   | 11/20/2025      |                                         |
| 6   | - Triển khai tính năng thanh toán lên môi trường staging <br> - Demo cho đội và thu phản hồi <br> - Viết tài liệu triển khai, runbook và cảnh báo giám sát cho production                     | 11/21/2025   | 11/21/2025      |                                         |


### Thành tựu Tuần 11:

* Đã tham dự AWS Cloud Mastery Series (Bitexco Financial Tower) vào 11/17/2025
  * Tham gia các phiên về DevOps mindset, pipeline CI/CD (CodeCommit, CodeBuild, CodeDeploy, CodePipeline), IaC (CloudFormation, CDK), dịch vụ container (ECR/ECS/EKS) và giám sát (CloudWatch, X-Ray).
  * Có thêm các ý tưởng thực tiễn để cải thiện CI/CD và chiến lược giám sát của dự án.

* Đã triển khai chức năng thanh toán bằng thẻ tín dụng an toàn
  * Thiết kế và tích hợp luồng thanh toán sử dụng tokenization (không lưu trữ thẻ thô)
  * Triển khai endpoint backend, tích hợp phía client và xác thực phía server
  * Thêm logging, retry và giám sát cho tỷ lệ thành công/thất bại thanh toán

* Đã kiểm tra luồng thanh toán bằng kiểm thử end-to-end và rà soát bảo mật
  * Kiểm thử kịch bản thành công, từ chối, và lỗi mạng
  * Áp dụng TLS, tokenization và các best practices bảo mật cơ bản

* Đã triển khai tính năng lên staging và viết tài liệu tích hợp
  * Chuẩn bị runbook, cảnh báo giám sát và hướng dẫn tích hợp cho đội
  * Demo tính năng cho đội và thu phản hồi để chuẩn bị rollout production


