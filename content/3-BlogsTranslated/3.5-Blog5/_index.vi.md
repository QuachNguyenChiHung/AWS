---
title: "Blog 5"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.5. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Cộng Tác Multi-Agent với Strands
  
Khi các hệ thống tự động phát triển, sự cộng tác giữa nhiều agent đang chuyển từ lý thuyết thành yếu tố thiết yếu. Khi các agent có được khả năng suy luận nâng cao, khả năng thích ứng và sử dụng công cụ, câu hỏi không còn là *“Một agent có thể giải quyết một nhiệm vụ không?”* mà là *“Làm thế nào để nhiều agent có thể làm việc cùng nhau một cách hiệu quả?”*

## Chuyển Đổi Hướng Tới Hệ Thống Multi-Agent



Mô hình **Supervisor**, được giới thiệu trong công trình trước đây về các AI agent bất đồng bộ với Amazon Bedrock, đã cung cấp bước đầu tiên theo hướng này. Hoạt động như một người điều phối tập trung, Supervisor quản lý các agent loosely coupled bằng cách ủy thác nhiệm vụ, xử lý fallback, và theo dõi trạng thái. Điều này cho phép các tổ chức tiến từ prototype single-agent đến các hệ thống multi-agent sơ khai.

Nhưng khi các hệ thống trở nên năng động hơn, những hạn chế của supervision tĩnh xuất hiện. Workflow thay đổi liên tục, khả năng mới nổi lên, và coordination phải thích ứng theo thời gian thực. Đây là lúc mô hình **Arbiter** xuất hiện—bước tiến hóa tiếp theo của orchestration agentic, được thiết kế cho coordination thích ứng, có thể mở rộng và nhận thức context.

---

## Từ Supervisor Đến Arbiter


Mô hình Supervisor hoạt động tốt với workflow dự đoán được và các agent ổn định. Tuy nhiên, môi trường hiện đại đòi hỏi nhiều hơn: khả năng tạo agent động, match nhiệm vụ theo ngữ nghĩa, và phối hợp thông qua trạng thái chia sẻ.

Mô hình **Arbiter** mở rộng Supervisor với ba đổi mới cốt lõi:

1. **Semantic Capability Matching** – Arbiter suy luận về loại agent cần thiết, ngay cả khi agent đó chưa tồn tại.
2. **Delegated Agent Creation** – Khi không tìm thấy agent phù hợp, Arbiter gọi **Fabricator agent** để tạo agent mới một cách động.
3. **Task Planning với Contextual Memory** – Nhiệm vụ được phân tách thành kế hoạch, theo dõi trong bộ nhớ, thử lại nếu cần, và đánh giá hiệu suất agent.

Điều này chuyển đổi orchestration từ *supervision tĩnh* sang *coordination thích ứng*.

---

## Mô Hình Blackboard Được Xem Xét Lại

Mô hình Arbiter mượn các nguyên lý từ **mô hình blackboard**, một kiến trúc cổ điển từ AI phân tán. Trong cách tiếp cận này, các agent chia sẻ một workspace chung ("blackboard"), đăng các giải pháp một phần hoặc cập nhật. Các agent khác quan sát và phản ứng, thúc đẩy giải quyết vấn đề cộng tác.

Trong triển khai của chúng tôi, blackboard trở thành một **semantic event substrate**:

* Các agent publish và consume các trạng thái liên quan đến nhiệm vụ.
* Arbiter phối hợp thông qua các semantic event này.
* Cộng tác trở thành event-driven và loosely coupled.

Thiết kế này cho phép khả năng thích ứng ở quy mô lớn—nơi các agent không cần API cứng nhắc, chỉ cần khả năng phản ứng với trạng thái đang phát triển.

---

## Cách Thức Hoạt Động Của Arbiter

Arbiter tuân theo một workflow event-driven có cấu trúc:

1. **Interpretation** – Một LLM diễn giải event, trích xuất mục tiêu và các sub-task.
2. **Capability Assessment** – Arbiter đánh giá các agent có sẵn thông qua local index hoặc capability manifest.
3. **Delegation hoặc Generation** –

   * Nếu agent tồn tại, nhiệm vụ được định tuyến trực tiếp.
   * Nếu không có agent nào tồn tại, Arbiter yêu cầu **Fabricator** tạo một agent.
4. **Blackboard Coordination** – Tất cả các agent tham gia đọc/ghi vào blackboard chia sẻ.
5. **Reflection và Adaptation** – Hiệu suất được ghi log và sử dụng để tinh chỉnh coordination tương lai hoặc kích hoạt tạo agent mới.

### Arbiter vs Supervisor

* **Supervisor:** Orchestration dựa vào danh sách cấu hình tĩnh.
* **Arbiter:** Coordination thích ứng động thông qua blackboard ngữ nghĩa chia sẻ.

Điều này cho phép điều chỉnh giữa nhiệm vụ, cộng tác phong phú hơn, và học tập liên tục.

---

## Fabricator Agent: Tạo Khả Năng Theo Yêu Cầu

**Fabricator** mở rộng khả năng thích ứng của Arbiter bằng cách tạo ra các agent mới khi những agent hiện có không thể xử lý một nhiệm vụ.

### Cách Thức Hoạt Động

* Nhận yêu cầu khả năng từ Arbiter.
* Tạo mã worker agent mới sử dụng Strands.
* Lưu trữ agent trong S3 để sử dụng runtime.
* Đăng ký khả năng trong DynamoDB để có sẵn ngay lập tức.
* Publish agent mới vào hệ thống để orchestration.

Cách tiếp cận này chuyển đổi hệ thống từ *được lập trình trước* thành *tự mở rộng*.

---

## Generic Wrapper: Runtime Thực Thi Động

Để chạy các agent mới này mà không cần cung cấp thêm infrastructure, **Generic Wrapper** cho phép hot-loading:

* Các agent được thực thi từ mã được lưu trữ trong S3.
* Một wrapper duy nhất dynamically load và thực thi mã agent.
* Kết quả được publish lại qua EventBridge để Arbiter theo dõi.

Điều này tách biệt **tăng trưởng agent** khỏi **mở rộng infrastructure**, cho phép hàng trăm agent tồn tại mà không có bottleneck vận hành.

### Lợi Ích Của Hot-Loading

* **Có thể mở rộng:** Hỗ trợ tạo agent không giới hạn.
* **Hiệu quả:** Tránh infrastructure mới cho mỗi agent.
* **Chuẩn hóa:** Tất cả agent giao tiếp thông qua cấu trúc event nhất quán.
* **Có khả năng phục hồi:** Lỗi được cô lập và xử lý một cách graceful.

---

## Workflow End-to-End

1. **Event nhận được** → Arbiter diễn giải và phân tách nhiệm vụ.
2. **Kiểm tra khả năng** → Tìm agent phù hợp hoặc yêu cầu Fabricator.
3. **Fabricator được gọi** → Tạo agent mới nếu cần, đăng ký nó.
4. **Generic Wrapper thực thi** → Hot-load và chạy mã agent.
5. **Blackboard chia sẻ được cập nhật** → Các agent cộng tác qua semantic state.
6. **Reflection loop** → Arbiter ghi log kết quả, thích ứng workflow tương lai.

---

## Khả Năng Chính Của Hệ Thống Arbiter

* **Xử Lý Bất Đồng Bộ** – Phân phối nhiệm vụ dựa trên SQS.
* **Quản Lý Trạng Thái Bền Vững** – Theo dõi workflow DynamoDB.
* **Khả Năng Mở Rộng** – Kiến trúc hot-loading hỗ trợ tăng trưởng agent vô tận.
* **Orchestration Thông Minh** – LLM phân tách nhiệm vụ và sắp xếp workflow.
* **Khả Năng Tự Mở Rộng** – Tạo agent dựa trên Strands theo yêu cầu.
* **Giao Tiếp Chuẩn Hóa** – Giao thức event-driven đảm bảo độ tin cậy.

---

## Kết Luận: Từ Supervision Đến Adaptation

Mô hình Arbiter đại diện cho một bước nhảy vọt từ orchestration tĩnh hướng tới **coordination thích ứng, generative và có khả năng phục hồi**. Bằng cách kết hợp semantic reasoning, tạo agent động, và cộng tác dựa trên blackboard, nó chuyển đổi các hệ sinh thái agent thành **hệ thống tự phát triển**.

Nơi Supervisor mang lại cho chúng ta trật tự, Arbiter mang lại khả năng thích ứng—mở đường cho các hệ thống multi-agent phi tập trung, thông minh có thể **học hỏi, thích ứng và cộng tác ở quy mô lớn**.
