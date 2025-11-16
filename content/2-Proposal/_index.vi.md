---
title: "Bản đề xuất"
date: 2025-10-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AWS First Cloud AI Journey – Project Plan

## Website Bán Hàng Trực tuyến: Furious Five Fashion (FFF)

## Giải pháp Website Bán Hàng kết hợp AWS và AI

### 1. Bối cảnh và động lực

#### 1.1 tóm tắt điều hành 

Khách hàng là một doanh nghiệp quy mô nhỏ , chuyên cung cấp các sản phẩm về thời trang cho các bạn trẻ . Doanh nghiệp mong muốn xây dựng website online bán quần áo với AWS và AI,có khả năng mở rộng linh hoạt, đủ sức ph át triển lâu dài và tối ưu chi phí vận hành . 

Về mục tiêu , dự án này là oột bước chuyển đổi tư duy - từ quản lý thủ công trên máy chủ vật lý sang mô hình linh hoạt , thông minh và đặc biệt là phải tối ưu chi phí .AWS cho phép hệ thống có thể mở rộng bất cứ lúc nào , đảm bảo tốc độ truy cập ,và giúp doanh nghiệp tập trung vào phát triển thay vì lo lắng về hạ tầng .

Hệ thống được thiết kế để phục vụ hoạt động bán hàng trực tuyến một cách toàn diện: lưu trữ và phân phối nội dung web, quản lý cơ sở dữ liệu sản phẩm – đơn hàng, hỗ trợ thanh toán, và giám sát hiệu suất vận hành. Mọi thứ đều hướng đến sự ổn định, an toàn và dễ mở rộng trong tương lai.

Đồng hành trong quá trình này là đội ngũ Furious Five sẽ triển khai – những người chịu trách nhiệm tư vấn, thiết kế kiến trúc và cấu hình các dịch vụ quan trọng như Lambda, S3, DynamoDB, CloudFront và Route 53. Ngoài việc dựng hệ thống, họ còn giúp tối ưu chi phí, đảm bảo bảo mật và hướng dẫn đội ngũ nội bộ quản lý hạ tầng hiệu quả.

Dự án này không chỉ là một bản kế hoạch kỹ thuật – nó là một bước khởi đầu cho hành trình trưởng thành của doanh nghiệp trong thế giới số.

#### 1.2 TIÊU CHÍ THÀNH CÔNG CỦA DỰ ÁN

Để dự án Furious Five Fashion thực sự thành công, chúng ta cần xác định rõ những tiêu chí cụ thể, vừa phản ánh được mục tiêu kinh doanh, vừa đánh giá được hiệu quả kỹ thuật sau khi triển khai. Dưới đây là những điểm mấu chốt mà đội dự án cần đạt được:

* Hiệu suất hệ thống:
Trang web phải duy trì thời gian phản hồi dưới 2 giây cho mọi tác vụ người dùng, kể cả trong thời điểm cao điểm truy cập.

* Tính sẵn sàng (Availability):
Hệ thống đạt 99,9% uptime trong suốt quá trình vận hành, được giám sát và báo cáo tự động qua các công cụ AWS như CloudWatch.

* Khả năng mở rộng (Scalability):
Hạ tầng AWS có thể mở rộng quy mô tự động khi lưu lượng truy cập tăng ít nhất gấp 2 lần mà không làm gián đoạn dịch vụ.

* Tối ưu chi phí:
Chi phí vận hành hàng tháng được duy trì dưới 30% ngân sách dự kiến, thông qua cơ chế giám sát và tối ưu tài nguyên (Cost Explorer, Trusted Advisor).

* Bảo mật hệ thống:
Không có sự cố rò rỉ dữ liệu hoặc truy cập trái phép. Tất cả dữ liệu khách hàng được mã hóa bằng các tiêu chuẩn bảo mật của AWS (IAM policies).

* Quy trình triển khai và vận hành:
Hoàn thành việc triển khai hạ tầng AWS trong 4 tuần, với tài liệu kỹ thuật và hướng dẫn vận hành rõ ràng để đội ngũ nội bộ có thể tiếp quản và quản lý hiệu quả.

* Hỗ trợ và đào tạo:
Đội kỹ thuật nội bộ được đào tạo đủ năng lực quản trị, giám sát và bảo trì hệ thống mà không cần phụ thuộc hoàn toàn vào bên thứ ba.

Thành công của Furious Five Fashion không chỉ nằm ở việc website hoạt động được — mà là ở chỗ nó chạy ổn định, an toàn, tiết kiệm chi phí và sẵn sàng phát triển lâu dài.
Một hệ thống thành công là khi cả khách hàng và đội ngũ vận hành đều cảm thấy “an tâm” khi sử dụng nó — và đó chính là mục tiêu chúng ta hướng tới.

#### 1.3 Các giả định  
Khi bắt tay vào dự án FFF, có vài điều mà chúng ta cần thống nhất và cùng tin tưởng để mọi thứ đi đúng hướng.

Trước hết, giả định rằng đội ngũ đều đã có tài khoản AWS và có thể truy cập đầy đủ vào các dịch vụ cần thiết. Mọi người cũng đã có chút nền tảng về AWS — ít nhất là hiểu các dịch vụ như lambda, S3, IAM, và Route 53 hoạt động ra sao. Kết nối Internet ổn định là điều kiện tiên quyết, vì toàn bộ hạ tầng của FFF đều nằm trên đám mây. Và tất nhiên, nhóm cũng cần hiểu rõ các yêu cầu về bảo mật và tuân thủ trước khi triển khai.

Dự án này không đứng một mình — nó phụ thuộc vào nhiều yếu tố. Chúng ta cần AWS hoạt động ổn định ở khu vực đã chọn, nhà cung cấp tên miền và Route 53 phải đảm bảo việc định tuyến mượt mà. Đồng thời, nhóm phát triển web cũng cần phối hợp nhịp nhàng để ứng dụng vận hành tốt trên môi trường cloud. Nói cách khác, thành công của FFF là kết quả của một tập thể biết ăn ý.

Tất nhiên, mọi thứ đều có giới hạn. Dự án này chỉ nằm trong phạm vi thực tập, nên ngân sách không nhiều — ưu tiên sử dụng Free Tier hoặc gói chi phí thấp.Nhóm chưa có quá nhiều kinh nghiệm, vậy nên ta chọn mô hình triển khai đơn giản và dễ quản lý. Thời gian lại ngắn, nên điều quan trọng là giữ mọi thứ thực tế và khả thi.

Về rủi ro, có vài điều đáng để cảnh giác. Sai sót nhỏ trong cấu hình IAM  có thể khiến hệ thống dễ bị tấn công. Nếu quên tắt tài nguyên sau khi thử nghiệm, chi phí có thể tăng lên nhanh chóng. Cũng có lúc AWS gặp sự cố vùng (region outage), và điều đó nằm ngoài tầm kiểm soát của chúng ta. Ngoài ra, đôi khi ứng dụng không tương thích hoàn toàn với dịch vụ AWS, hoặc đơn giản là ta thiếu người có kinh nghiệm xử lý sự cố.

Dẫu vậy, tin tưởng rằng chúng ta đang đi đúng hướng. Mọi thứ đều được xây dựng trên nền tảng giả định rằng khách hàng hiểu rõ đây là phiên bản thử nghiệm (pilot), và chúng ta luôn có giải pháp sao lưu, giám sát và quản lý chi phí cẩn thận. Quan trọng nhất, mỗi rủi ro đều là một bài học quý — giúp ta trưởng thành hơn trong hành trình học cloud và triển khai thật sự.
### 2 KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC

#### 2.1 Sơ đồ Kiến trúc Kỹ thuật

Dưới đây là kiến trúc kỹ thuật được thiết kế cho dự án FFF, triển khai trên AWS Cloud – Region Singapore (ap-southeast-1). Mục tiêu của kiến trúc này là đảm bảo hệ thống linh hoạt, bảo mật, tự động hóa cao và có khả năng mở rộng dễ dàng trong tương lai, đồng thời vẫn phù hợp với phạm vi dự án thực tập.

Hệ thống được xây dựng theo mô hình đa tầng gồm 6 phần chính :

Frontend & Security Layer:
Người dùng truy cập thông qua Route 53, lưu lượng được bảo vệ bởi AWS WAF và phân phối nhanh bằng CloudFront.
Mã nguồn được quản lý và triển khai tự động qua GitLab và CloudFormation.

API & Compute Layer:
API Gateway tiếp nhận và định tuyến yêu cầu đến AWS Lambda – nơi xử lý logic ứng dụng.
Cognito đảm nhận xác thực người dùng và cấp quyền truy cập API.

Storage Layer:
Gồm hai S3 Bucket: một cho dữ liệu tĩnh (StaticData) và một cho dữ liệu tải lên (Uploads).

Data Layer:
DynamoDB lưu trữ thông tin phi cấu trúc và metadata.
IAM quản lý quyền truy cập an toàn giữa các thành phần.

AI Layer:
Tích hợp Amazon Rekognition và Amazon Bedrock để xử lý hình ảnh và AI tạo sinh cho ứng dụng.

Observability & Security Layer:
CloudWatch, SNS, và SES giám sát, gửi cảnh báo và thông báo khi có sự cố; đảm bảo hệ thống vận hành ổn định.

Dưới đây là sơ đồ luồng dữ liệu :
![E-commerce Website Solution ](/images/2-Proposal/proposal.jpg)

#### 2.2 Kế hoạch kỹ thuật

Trong dự án FFF, nhóm triển khai sẽ phát triển và quản lý hạ tầng bằng các tập lệnh tự động (Infrastructure as Code) sử dụng AWS CloudFormation. Cách làm này giúp việc triển khai hệ thống lên các tài khoản AWS trở nên nhanh chóng, lặp lại và dễ kiểm soát hơn, đồng thời giảm thiểu sai sót thủ công trong quá trình cài đặt.

Các thành phần chính của hạ tầng – bao gồm S3, Lambda, API Gateway, DynamoDB, Cognito, và CloudWatch – sẽ được định nghĩa và khởi tạo thông qua các mẫu CloudFormation. Mọi thay đổi về cấu hình sẽ được lưu lại trong GitLab để đảm bảo tính minh bạch và khả năng phục hồi nếu cần quay lại phiên bản trước.

Một số cấu hình nhạy cảm như quyền truy cập IAM hoặc chính sách bảo mật của WAF có thể yêu cầu phê duyệt riêng trước khi triển khai. Các thay đổi này sẽ tuân thủ quy trình xem xét nội bộ, bao gồm kiểm tra, xác nhận, và phê duyệt bởi người chịu trách nhiệm kỹ thuật.

Tất cả đường dẫn quan trọng (critical paths) trong kiến trúc – từ xác thực người dùng, xử lý API đến lưu trữ dữ liệu và giám sát hệ thống – đều được bao phủ bởi kịch bản kiểm thử tự động và thủ công, đảm bảo tính ổn định, an toàn và khả năng mở rộng của giải pháp.

Kế hoạch kỹ thuật này giúp đội FFF không chỉ triển khai hiệu quả, mà còn học được cách vận hành một hệ thống cloud chuyên nghiệp – từng dòng script đều là một bước tiến tới tư duy kỹ sư thực thụ.

#### 2.3 KẾ HOẠCH DỰ ÁN

Dự án FFF được triển khai theo khung Agile Scrum, trong 3 tháng, chia thành 4 sprint. Phương pháp này giúp nhóm duy trì nhịp độ làm việc ổn định, phản hồi nhanh với thay đổi kỹ thuật và tối ưu hóa kết quả qua từng giai đoạn.

Cấu trúc triển khai:

* Sprint Planning: 
   * thiết lập các môi trường AWS cơ bản (S3, Route53,IAm ).
   * cấu hình dịch vụ bảo mật (AWS WAF, CloudFront).
   * Tích hợp các thành phần backend (Lambda, API Gateway, DynamoDB)
   * Kiểm thử hệ thống, tối ưu hiệu năng, và triển khai bản demo cuối.

* Daily Stand-up: Cập nhật tiến độ, xử lý trở ngại kỹ thuật trong 30 phút mỗi ngày.

* Sprint Review: Đánh giá kết quả sprint, trình bày bản demo trên môi trường AWS thật , nếu sai sót sẽ sửa đổi.

* Retrospective: Phân tích rút kinh nghiệm, tối ưu quy trình DevOps và tự động hóa triển khai.

Phân công trách nhiệm:

* Product Owner: Quản lý backlog, định hướng ưu tiên tính năng và mốc kỹ thuật.(Dương Minh Đức)

* Scrum Master: Điều phối sprint, đảm bảo tuân thủ quy trình Agile và loại bỏ trở ngại.(Quách Nguyễn Chí Hùng)

* DevOps/Technical Team: Phát triển, kiểm thử, triển khai hạ tầng qua CloudFormation và quản lý tài nguyên AWS.(Nguyễn Tấn Xuân )

* Mentor / AWS Partner: Giám sát kỹ thuật, đánh giá tuân thủ AWS Well-Architected Framework,kiểm thử AI , hỗ trợ bảo mật và chi phí.(Hải Đăng , Phạm Lê Huy Hoàng)

Nhịp độ giao tiếp:

* Daily Stand-up: Họp ngắn 30 phút mỗi ngày, diễn ra vào khoảng 23:00

* Weekly Sync: Báo cáo tiến độ và rủi ro kỹ thuật.

* End-of-Sprint Review: Tổng kết và xác nhận sản phẩm tạm thời.

Chuyển giao kiến thức (Knowledge Transfer):
Sau sprint cuối, nhóm đối tác kỹ thuật sẽ tổ chức phiên Knowledge Transfer để hướng dẫn vận hành hệ thống, giám sát chi phí (AWS Budgets, CloudWatch), quy trình mở rộng môi trường , sao lưu và khôi phục . Các tài liệu Knowledge Transfer sẽ được bàn giao cho nhóm để đảm bảo nhóm FFF có thể tự tin duy trì ,tối ưu .

#### 2.4 CÁC CÂN NHẮC VỀ BẢO MẬT

1. Quản lý truy cập **(Access Management)**
Nhóm chỉ sử dụng một số tài khoản AWS chính, vì vậy MFA (xác thực đa yếu tố) sẽ được bật cho toàn bộ người dùng có quyền quản trị. Quyền truy cập được phân tách rõ qua IAM User và IAM Role, theo nguyên tắc ít quyền nhất (Least Privilege). Tất cả thao tác quản trị đều được ghi nhận qua CloudTrail để dễ theo dõi và kiểm soát.
2. An ninh cơ sở hạ tầng **(Infrastructure Security)**
Mặc dù không triển khai VPC riêng, các dịch vụ AWS (như S3, Lambda, API Gateway) vẫn được cấu hình để giới hạn truy cập chỉ từ các thành phần nội bộ của hệ thống. Các endpoint công khai đều yêu cầu kết nối qua HTTPS.

1. Bảo vệ dữ liệu **(Data Protection)**
Dữ liệu được lưu trữ trên S3 và DynamoDB, với các tùy chọn mã hóa tích hợp sẵn của dịch vụ. Dữ liệu truyền giữa các thành phần (Lambda ↔ API ↔ Database) luôn đi qua giao thức HTTPS/TLS, đảm bảo an toàn khi trao đổi. Ngoài ra, nhóm thiết lập sao lưu thủ công định kỳ cho dữ liệu quan trọng, giúp khôi phục nhanh nếu có sự cố.
1. Phát hiện và giám sát **(Detection & Monitoring)**
CloudTrail, Config và CloudWatch sẽ luôn ghi lại mọi hành động, giúp ta biết chính xác điều gì đang diễn ra. GuardDuty sẽ liên tục quét, cảnh báo sớm nếu phát hiện hành vi bất thường.
1. Quản lý sự cố **(Incident Response)**
Nhóm sẽ xây dựng quy trình phản ứng sự cố rõ ràng, ghi nhận log, phân tích và khắc phục kịp thời. Quan trọng hơn, chúng ta sẽ diễn tập định kỳ, để khi rủi ro thật sự đến, không ai hoảng loạn – mà hành động đúng, nhanh và chắc.

### 3. CÁC HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO
#### 3.1 Các hoạt động và sản phẩm bàn giao

Bảng dưới đây sẽ tổng hợp các mốc thời gian, hoạt động và sản phẩm bàn giao tương ứng với các mục trong Phạm vi Công việc / Kế hoạch Kỹ thuật của Dự án. Đồng thời, nêu rõ kế hoạch quản lý dự án, quản lý thay đổi, truyền thông và chuyển đổi.

| Giai đoạn dự án| Mốc thời gian                                                                                                                                                                                                   | Hoạt động| Sản phẩm bàn giao/mốc quan trọng |tổng số ngày công                     |
| --------------------------------------------- | ---------- | ----------------------------- | -------------------------------| ------------------------- |
| thiết lập cơ sở hạ tầng   | tuần 1 - 2 |- thu thập và xác nhận yêu cầu doanh nghiệp <br> - Thiết kế kiến trúc kỹ thuật AWS <br> - Cấu hình hạ tầng AWS (S3, CloudFront, API Gateway, Lambda, DynamoDB, Cognito) <br> - Thiết lập GitLab CI/CD pipeline.  |- Kiến trúc kỹ thuật AWS hoàn chỉnh <br> - Hạ tầng cơ sở sẵn sàng <br> - GitLab CI/CD hoạt động.      | 10 ngày
| Thiết lập thành phần 1 (Frontend)  |  Tuần 3 – 5  | - Thiết kế UI/UX <br> - Phát triển giao diện website: trang chủ, danh mục, chi tiết sản phẩm, giỏ hàng, thanh toán <br> - Tích hợp với API demo. | - Giao diện website hoàn thiện (bản dev) <br> - Hệ thống FE kết nối được với API.      | 15 ngày  |
|Thiết lập thành phần 2 (Backend & Database)   |Tuần 6 - 9   |- Xây dựng API bằng AWS Lambda + API Gateway <br> - Thiết lập cơ sở dữ liệu DynamoDB <br> - Xây dựng logic xử lý đơn hàng, người dùng, sản phẩm <br> - Tích hợp bảo mật Cognito và phân quyền IAM.   | - API hoạt động ổn định <br> - Dữ liệu được lưu trữ và truy xuất đúng chuẩn <br> - Backend tích hợp hoàn chỉnh với FE.      | 20 ngày |
|Kiểm thử & Vận hành chính thức   |  Tuần 10 - 11  | - Kiểm thử chức năng, bảo mật, hiệu năng <br> - Ghi nhận lỗi và tối ưu hệ thống <br> - Kiểm thử tích hợp FE – BE – DB trên môi trường AWS. | - Báo cáo kết quả kiểm thử (Test Report) <br> - Phiên bản đạt chuẩn vận hành      | 5 ngày |
|Bàn giao & thuyết trình    | Tuần 12  | - Triển khai production trên AWS <br> - Thiết lập domain & SSL <br> - Đào tạo quản trị hệ thống <br> - Bàn giao mã nguồn và tài liệu kỹ thuật. | - Website FFF hoạt động chính thức <br> - Bộ tài liệu hướng dẫn & bàn giao hoàn chỉnh <br> - Báo cáo bàn giao hệ thống.   | 5 ngày |

#### 3.2 NGOÀI PHẠM VI
Các hạng mục dưới đây đã được thảo luận trong giai đoạn xác định yêu cầu, tuy nhiên được xác định là ngoài phạm vi thực hiện của dự án FFF Web quần áo ở giai đoạn hiện tại.

Các mục ngoài phạm vi bao gồm:
* Phát triển ứng dụng di động (Mobile App) cho hệ thống (Android/iOS).
* Tích hợp hệ thống quản lý tồn kho, vận chuyển và logistics thực tế (Giao hàng nhanh, GHN, Viettel Post, v.v.).
* Chức năng quản trị nâng cao như phân quyền nhiều cấp độ, báo cáo doanh thu tự động, biểu đồ thống kê nâng cao.
* Tích hợp CRM (Customer Relationship Management) hoặc ERP (Enterprise Resource Planning) của bên thứ ba.
* Dùng các dịch vụ AWS có tính năng tự động bảo mật cao hơn , mắc tiền hơn.
* Tích hợp cổng thanh toán thực tế (VNPay, Momo, ZaloPay, Stripe, PayPal, v.v.)
* Đa ngôn ngữ (Multilingual) và đa tiền tệ (Multi-currency)
#### 3.3 LỘ TRÌNH ĐƯA VÀO VẬN HÀNH CHÍNH THỨC

Giai đoạn 1 – Làm bản thử nghiệm (POC)

Hoạt động:
Xây dựng phiên bản thử nghiệm FFF Web Bán Hàng với giao diện cơ bản (Home, Danh mục, Chi tiết sản phẩm, Giỏ hàng).
* Kết nối backend qua API Gateway – Lambda – DynamoDB.
* Triển khai website tĩnh trên Amazon S3 + CloudFront.
* Cấu hình tài khoản quản trị và demo quy trình đặt hàng thử.

Kết quả:

* Website hoạt động ổn định trên môi trường AWS thử nghiệm.
* Toàn bộ dữ liệu sản phẩm mẫu được lưu trữ thành công trên DynamoDB.
* Tốc độ tải trang qua CloudFront đạt trung bình < 2 giây.
* Xác minh kết nối API và logic xử lý đặt hàng hoàn chỉnh.
  
Giai đoạn 2 – Hoàn thiện hệ thống và kiểm thử (UAT)

Hoạt động:
* Bổ sung các chức năng người dùng: đăng nhập/đăng ký, xác thực qua AWS Cognito.
* Thêm tính năng thanh toán thử qua sandbox.
* Bổ sung giám sát bằng Amazon CloudWatch và log xử lý lỗi.
* Thực hiện kiểm thử người dùng nội bộ (User Acceptance Test).

Kết quả:

* 100% chức năng cốt lõi hoạt động đúng logic.
* Không phát sinh lỗi nghiêm trọng trong quy trình đặt hàng.
* Hiệu năng trung bình đáp ứng được 100 người dùng đồng thời.
* Giao diện và dữ liệu hiển thị thống nhất giữa frontend và backend.

Giai đoạn 3 – Triển khai vận hành chính thức (Production)

Hoạt động:
* Chuyển toàn bộ hệ thống từ môi trường thử nghiệm sang Production AWS.
* Cấu hình Route53 cho domain chính thức và chứng chỉ SSL qua AWS Certificate Manager.
* Thiết lập bảo mật lớp ngoài bằng AWS WAF.
* Tối ưu dung lượng S3 và cấu trúc CDN trên CloudFront.

Kết quả:

* Website chính thức FFF Web Bán Hàng hoạt động tại domain thật.
* Tỷ lệ uptime đạt 99.98% sau 2 tuần đầu vận hành.
* Độ trễ trung bình giữa client và API dưới 400ms.
* Mọi request được ghi nhận và giám sát theo chuẩn CloudWatch Logs.

Giai đoạn 4 – Ổn định & tối ưu sau triển khai

Hoạt động:
* Theo dõi chi phí AWS thực tế, tối ưu dung lượng lưu trữ và log.
* Điều chỉnh cấu hình Lambda để giảm thời gian cold start.
* Thực hiện backup định kỳ và thử nghiệm khôi phục dữ liệu.
* Cập nhật tài liệu hướng dẫn vận hành cho nhóm quản trị.

Kết quả:

* Giảm chi phí AWS trung bình 20% so với giai đoạn POC.
* Thời gian phản hồi API rút ngắn thêm 15%.
* Hệ thống hoạt động ổn định, không phát sinh lỗi nghiêm trọng.
* Đội ngũ vận hành nội bộ đã có thể tự theo dõi và xử lý sự cố cơ bản.

Tổng kết

Hệ thống FFF Web Bán Hàng đã được triển khai thành công trên nền tảng AWS Serverless với kiến trúc tối ưu chi phí, bảo mật cao và dễ mở rộng.
Các giai đoạn được hoàn thành đúng tiến độ, đảm bảo toàn bộ chức năng được kiểm thử, tinh chỉnh và vận hành ổn định.
Dự án hiện đã sẵn sàng mở rộng người dùng thật và tích hợp thêm các tính năng thương mại điện tử nâng cao.
### 4. CHI TIẾT CHI PHÍ AWS DỰ KIẾN THEO DỊCH VỤ

*Chi phí hạ tầng*
- Chi phí hạ tầng (ước tính theo tháng)
- Route 53 :         $1.00
- AWS WAF :          $5.00
- CloudFront:        $3.90
- S3 (StaticData) :  $0.50
- S3 (Uploads):      $0.75
- AWS Lambda:        $0.25
- API Gateway:       $3.50
- Amazon Bedrock:    $3.00
- DynamoDB:          $1.00
- IAM:               Free
- CloudWatch:        $2.00
- SNS:               $0.10
- SES:               $0.20
- CloudFormation:    Free
- GitLab CI/CD  :    $3.00
- WS Config / Setup & Test migration tools $5.00 (1 lần)
- Tổng chi phí ước tính hàng tháng: ~ $30.00 – $35.00 USD

GIẢ ĐỊNH CHÍNH

Region: ap-southeast-1 (Singapore).
Người dùng truy cập: 500–1000/tháng.
Hệ thống luôn hoạt động 24/7 nhưng tải thấp.
Phần lớn API qua Lambda, không dùng EC2.
Dữ liệu nhỏ (<100GB tổng).
CI/CD thực hiện 1–2 lần deploy mỗi tuần.
Free-tier còn hiệu lực trong 12 tháng đầu.
AI sử dụng ở mức demo, không phải inference quy mô lớn.

TỐI ƯU CHI PHÍ ĐỀ XUẤT

Bật S3 Intelligent-Tiering để tự động chuyển dữ liệu ít truy cập.
Giới hạn CloudWatch Logs giữ lại 14–30 ngày.
Dùng AWS Budgets để cảnh báo nếu vượt $40/tháng.
Nếu triển khai lâu dài → cân nhắc Savings Plan cho Lambda (giảm 30–40%).

### 5. Đội Ngũ

**Nhà tài trợ Điều hành phía Đối tác** <br>

Tên : Nguyen Gia Hung <br>
Chức danh:  Giám đốc chương trình Đào tạo FCJ Việt Nam <br>
Mô tả : Là Nhà tài trợ Điều hành chịu trách nhiệm giám sát tổng thể chương trình thực tập FCJ. Đảm bảo dự án mang lại giá trị học tập, tuân thủ mục tiêu kỹ thuật và định hướng nghề nghiệp của AWS <br>
Email/thông tin liên hệ : hunggia@amazon.com|

**Các bên liên quan của Dự án** <br>
Tên : Van Hoang Kha <br>
Chức danh: Support Teams <br>
Mô tả: là người hỗ trợ Điều hành chịu trách nhiệm giám sát tổng thể chương trình thực tập FCJ<br>
Email/Thông tin liên hệ : Khab9thd@gmail.com

**Đội ngũ Dự án phía Đối tác** (Nhóm Thực tập Furious Five )<br>
Tên : Dương Minh Đức <br>
Chức danh: Trưởng nhóm dự án<br>
Mô tả: Quản lý tiến độ, điều phối công việc giữa nhóm và mentor,Quản lý triển khai hạ tầng AWS ( S3, Lambda, IAM)<br>
Email/Thông tin liên hệ : ducdmse182938@fpt.edu.vn

Tên : Quách Nguyễn Chí Hùng<br>
Chức danh:Thành viên<br>
Mô tả:Phụ trách UI/UX và phần giao diện người dùng<br>
Email/Thông tin liên hệ : bacon3632@gmail.com

Tên :Nguyễn Tấn Xuân<br>
Chức danh:Thành viên<br>
Mô tả: Phụ trách Backend và xử lý logic server<br>
Email/Thông tin liên hệ : xuanntse184074@fpt.edu.vn<br>

Tên :Nguyễn Hải Đăng<br>
Chức danh:Thành viên<br>
Mô tả: Quản lý triển khai hạ tầng AWS ( S3, Lambda, IAM)và tích hợp chat bot AI<br>
Email/Thông tin liên hệ : dangnhse184292@fpt.edu.vn

Tên :Phạm Lê Huy Hoàng<br>
Chức danh:Thành viên<br>
Mô tả: Kiểm thử, đảm bảo chất lượng và tích hợp GitLab CI/CD, và tích hợp chat bot AI<br>
Email/Thông tin liên hệ : hoangplhse182670@fpt.edu.vn

**Liên hệ Khiếu nại / Leo thang Dự án**

Tên :Dương Minh Đức<br>
Chức danh:Trưởng nhóm dự án<br>
Mô tả:Đại diện nhóm thực tập liên hệ trực tiếp với mentor và nhà tài trợ<br>
Email/Thông tin liên hệ : ducdmse182938@fpt.edu.vn


### 6. NGUỒN LỰC & ƯỚC TÍNH CHI PHÍ
#### Nguồn Lực

| Vai trò | trách nhiệm| Mức phí (USD)/Giờ|
| ------------------ | ------------------------------------- | ------------------ |
|Solution Architect(1)|Thiết kế giải pháp tổng thể, bảo đảm tính khả thi kỹ thuật, lựa chọn dịch vụ AWS phù hợp | 35|
|Cloud Engineer(2)|Triển khai hạ tầng AWS, cấu hình dịch vụ ( S3, IAM...), kiểm thử và tối ưu hệ thống| 20 |
|Project Manager (1)|Theo dõi tiến độ, điều phối nhóm, quản lý phạm vi và rủi ro dự án. |15 |
|Support / Documentation (1) |Chuẩn bị tài liệu bàn giao, hướng dẫn sử dụng, và báo cáo tổng kết. | 10| 

#### Ước tính chi phí theo giai đoạn dự án
| Giai đoạn dự án | Kiến trúc sư Giải pháp (giờ)  | 2 Engineers (giờ)     | Project Manager (giờ) | Quản lý dự án / Hỗ trợ (giờ)| Tổng số giờ|
|-----------------------------------------------------|:---------:|:---------:|:----------:|:-------------:|:--------------:|
|Khảo sát & thiết kế giải pháp| 53 | 40 | 13  | 13  |119|
|Triển khai & Kiểm thử|67 |160 | 21|19 |267 | 
|bàn giao & hỗ trợ | 27 |53  |21  | 19 | 120 |
|Tổng số giờ|147 | 253| 55 |51 | 506|
|Tổng số tiền |$ 5145 |$5060 |$ 825|$510 |$11540 |

#### Phân bổ đóng góp chi phí 
| Bên | Mức đóng góp (USD) | % đóng góp |
|-------------------------------------------|:--------:|:--------:|
|Khách hàng |4616 |40% | 
|Đối tác (Furious Five) |2308 |20% | 
| AWS |4616 | 40%| 