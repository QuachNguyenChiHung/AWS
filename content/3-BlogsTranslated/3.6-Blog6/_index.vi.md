---
title: "Blog 6"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.6. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Từ Linh Hoạt Đến Khung: Thực Thi Thứ Tự Công Cụ Trong MCP Servers

The **Model Context Protocol (MCP)** được tạo ra nhằm mang lại tính nhất quán trong cách các ứng dụng tương tác với các mô hình AI sinh. Thay vì phải chắp vá từng tích hợp riêng lẻ cho mỗi mô hình hoặc môi trường lưu trữ, MCP cung cấp một **lớp giao tiếp chuẩn hóa**.

## Giới Thiệu: Tại Sao MCP Quan Trọng

**Model Context Protocol (MCP)** được tạo ra để mang tính nhất quán vào cách các ứng dụng tương tác với các mô hình Generative AI. Thay vì ghép lại các tích hợp riêng biệt cho mỗi mô hình hoặc môi trường hosting, MCP cung cấp **lớp giao tiếp chuẩn hóa**.

Việc chuẩn hóa này làm cho nó mạnh mẽ cho các ứng dụng AI, đặc biệt là những ứng dụng dựa vào agent sử dụng công cụ bên ngoài. Nhưng với sự linh hoạt này đi kèm một khoảng trống: **MCP không tự nhiên thực thi trình tự mà các công cụ nên được sử dụng**. Trong các tình huống như **Infrastructure as Code (IaC)**, sự thiếu thứ tự này có thể dẫn đến thất bại workflow quan trọng.


---

## Thách Thức: Tại Sao Thứ Tự Công Cụ Quan Trọng

MCP cho phép một LLM (thông qua agent) gọi bất kỳ công cụ có sẵn nào—chẳng hạn như gửi email hoặc lấy dữ liệu thời tiết—mà không có hạn chế về thứ tự. Nhưng trong thực tế, nhiều công cụ có phụ thuộc.

### Các Tình Huống Phụ Thuộc Phổ Biến

* **Chained calls** – Một công cụ phải chạy trước công cụ khác.

  * Ví dụ: `getOrderId()` phải đứng trước `getOrderDetail()`.
  * Ví dụ: `fetch_weather_data()` phải chạy trước `send_email()`.
* **Hành Vi Mặc Định Của MCP** – Tất cả công cụ hoạt động như các hàm độc lập. Framework không biết cái nào nên đến trước.

Điều này đặc biệt có vấn đề trong các quy trình có cấu trúc như **CI/CD pipelines**, nơi mọi giai đoạn phải chạy theo thứ tự nghiêm ngặt:

1. Một pull request kích hoạt pipeline.
2. Linting, unit tests, và security checks được chạy.
3. Một lỗi dừng workflow ngay lập tức.

Thêm vào đó **hành vi không xác định của LLMs**—nơi các prompt giống hệt nhau không luôn tạo ra output giống hệt nhau—và bạn thấy nhu cầu về **một cơ chế để thực thi thứ tự mà không hy sinh sự linh hoạt**.

---

## Hiểu Về MCP Communication

MCP định nghĩa ba giai đoạn lifecycle:

1. **Initialization** – Client và server thỏa thuận phiên bản giao thức và khả năng.
2. **Operation** – Client gọi công cụ và xử lý responses.
3. **Shutdown** – Kết nối đóng một cách graceful.

Trong **initialization**, MCP server chia sẻ các công cụ có sẵn, schema của chúng, và hướng dẫn sử dụng. Dữ liệu schema này cho phép AI agent học không chỉ những công cụ nào tồn tại, mà còn những input và output chúng mong đợi.

Ví dụ, một tool schema có thể yêu cầu một `Result from get_aws_session_info()` hoặc một `security_scan_token`. Bằng cách expose những yêu cầu này sớm, MCP tạo cơ hội để hướng dẫn workflows.

---

## Giải Pháp: Token-Based Orchestration

Vì MCP không cung cấp phụ thuộc trực tiếp giữa các công cụ, **CCAPI MCP server** giới thiệu **mô hình token messenger**.

Thay vì các công cụ truyền thông tin cho nhau, server phát hành **các token bảo mật cryptographic** hoạt động như bằng chứng một phụ thuộc đã được thỏa mãn.

### Cách Nó Hoạt Động

#### 1. Enhanced Functions với `@mcp.tool()`

* Mọi công cụ được wrap với quy tắc validation input và định nghĩa schema.
* Tài liệu làm rõ ràng những gì mỗi công cụ yêu cầu.
* Ví dụ: `generate_infrastructure_code()` sẽ không chạy trừ khi một `session_token` hợp lệ được cung cấp.

#### 2. Dependency Discovery Tại Initialization

* Server publish bản đồ phụ thuộc đầy đủ trong quá trình khởi động.
* AI agent học những tham số (và tokens) nào cần thiết trước khi một công cụ có thể chạy.
* Ví dụ trình tự:

  ```
  get_aws_session_info() → generate_infrastructure_code() → run_checkov() → create_resource()
  ```

#### 3. Server-Side Token Validation

* Tokens được lưu trữ trong memory (`_workflow_store`) và expire sau khi sử dụng.
* Công cụ consume tokens và tạo ra tokens mới, tạo thành một chuỗi.
* Nếu token bị thiếu, hết hạn, hoặc tái sử dụng, hoạt động thất bại ngay lập tức.

Điều này đảm bảo công cụ tuân theo trình tự dự định mà không cần LLM "đoán" thứ tự đúng.

---

## Ví Dụ Workflow

1. `get_aws_session_info()` → tạo ra `session_token`.
2. `generate_infrastructure_code()` → validate `session_token`, consume nó, và tạo `generated_code_token`.
3. `run_checkov()` → yêu cầu `generated_code_token`, sau đó tạo ra `security_scan_token`.
4. `create_resource()` → thực thi chỉ khi `security_scan_token` hợp lệ.

Điều này tạo ra **chuỗi cryptographic của trust** thực thi tính toàn vẹn workflow.

---

## Thách Thức và Hạn Chế

### 1. Quản Lý Session

* Tokens được gắn với sessions và reset khi sessions expire.
* Điều này phản ánh **AWS credential expiration**, phù hợp bảo mật với lifecycle workflow.

### 2. Concurrent Sessions

* Mỗi workflow chạy độc lập, tránh cross-contamination giữa các agent.

### 3. Persistence

* Tokens được gắn với memory vì bảo mật.
* Persistent storage có thể nhưng thường không cần thiết, vì tokens được thiết kế ngắn hạn.

---

## Nhìn Về Phía Trước: Tương Lai Của MCP

Trong khi token orchestration hoạt động ngày nay, giao thức MCP có thể phát triển để hỗ trợ workflows deterministic một cách tự nhiên hơn.

* **Schema-Defined Dependencies**

  ```python
  @mcp.tool(depends_on=["run_checkov"])
  ```
* **Lifecycle Hooks** – Tương tự như hooks của Claude Code, những hooks này sẽ thực thi thứ tự được đảm bảo bên trong framework.

Đối với IaC, CI/CD, và các domain deterministic khác, những cải tiến này sẽ thiết yếu cho việc áp dụng ở quy mô.

---

## Kết Luận

Điểm mạnh của MCP nằm ở sự linh hoạt của nó, nhưng các workflow enterprise phức tạp yêu cầu **tính dự đoán và kiểm soát**.

Bằng cách thêm **token-based orchestration** vào **CCAPI MCP server**:

* Thực thi thứ tự công cụ nghiêm ngặt.
* Bảo mật workflows với validation server-side.
* Giữ kiến trúc linh hoạt của MCP.

Cách tiếp cận này cho thấy MCP có thể chuyển từ **linh hoạt đến framework**—hỗ trợ cả đổi mới và độ tin cậy nghiêm ngặt được yêu cầu cho quản lý cloud infrastructure.

Câu chuyện của MCP vẫn đang diễn ra, nhưng token-based orchestration cung cấp một con đường rõ ràng phía trước: **từ thử nghiệm đến operations enterprise-grade**.
