---
title: "Kiểm Tra Endpoints Toàn Diện"
date: 2025-12-01
weight: 8
chapter: false
pre: " <b> 5.08. </b> "
---

#### Tổng quan

Sau khi triển khai chương trình phụ trợ thành công, bạn cần kiểm tra tất cả các điểm cuối API để đảm bảo hệ thống hoạt động chính xác. Hội thảo này hướng dẫn chi tiết cách thử nghiệm từng module của hệ thống Thương mại điện tử OJT.

#### Kiến trúc API

```
Cổng API (API REST)
↓
┌─ ...�
│ 11 Mô-đun Lambda (63 API) │
├─────────────────────────────────────────────────────────────────────┤
│ Auth │Sản phẩm │ Giỏ hàng │ Đơn hàng │Danh mục │ Thương hiệu │
│(4 API) │(12 API) │(6 API) │(9 API) │ (6 API) │ (5 API) │
├───────────┼───────────────────────────────────────────────────────┤
│ Biểu ngữ │ Xếp hạng │ Người dùng │ Hình ảnh │ Sản phẩm │ │
│(7 API) │(3 API) │(3 API) │(1 API) │ Chi tiết │ │
│ │ │ │ │(7 API) │ │
└─ ...
↓
RDS SQL Server (thông qua Secrets Manager)
```

---

### Bước 1: Nhận API Điểm cuối

**1. Lấy URL API từ Đầu ra CloudFormation**

```powershell
# Lấy điểm cuối API từ API Stack
$API_URL = aws cloudformation describe-stacks `
--stack-name OJT-ApiStack `
--query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' `
--output text

Write-Host "Điểm cuối API: $API_URL"
# Đầu ra: https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod
```

**2. Thiết lập Biến Môi trường**

```powershell
# Thiết lập điểm cuối API cho phiên PowerShell
$API_URL = "https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod"

Write-Host "Điểm cuối API đã được cấu hình: $API_URL"
```

---

### Bước 2: Kiểm tra API Xác thực

#### 2.1 Đăng ký Người dùng Kiểm tra

```powershell
# POST /auth/signup
$signupBody = @{
email = "testuser@example.com"
password = "Test123!"
fullName = "Người dùng Kiểm tra"
phone = "+84901234567"
} | Chuyển đổi sang JSON

$response = Invoke-RestMethod `
-Uri "$API_URL/auth/signup" `
-Method Post `
-ContentType "application/json" `
-Body $signupBody

$response | Chuyển đổi sang JSON

```

#### 2.2 Kiểm tra đăng nhập người dùng

```powershell
# POST /auth/login
$loginBody = @{
email = "testuser@example.com"
password = "Test123!"
} | ConvertTo-Json

$loginResponse = Invoke-RestMethod `
-Uri "$API_URL/auth/login" `
-Method Post `
-ContentType "application/json" `
-Body $loginBody

# Lưu mã thông báo JWT cho các yêu cầu đã xác thực
$TOKEN = $loginResponse.token
Write-Host "Mã thông báo JWT: $TOKEN"

$loginResponse | ConvertTo-Json
# Dự kiến: { "token": "eyJhbG...", "user": { "userId": 1, "email": "...", "role": "Customer" } }
```

#### 2.3 Kiểm tra Lấy Người dùng Hiện tại

```powershell
# GET /auth/me (yêu cầu xác thực)
$headers = @{
"Authorization" = "Bearer $TOKEN"
}

$meResponse = Invoke-RestMethod `
-Uri "$API_URL/auth/me" `
-Method Get `
-Headers $headers

$meResponse | ConvertTo-Json
# Dự kiến: { "user": { "userId": 1, "email": "testuser@example.com", "fullName": "Người dùng thử nghiệm" } }
```

---

### Bước 3: API sản phẩm thử nghiệm

#### 3.1 Lấy tất cả sản phẩm

```powershell
# GET /products
$products = Invoke-RestMethod `
-Uri "$API_URL/products?page=1&limit=10" `
-Method Get

Write-Host "Đã tìm thấy $($products.total) sản phẩm"
$products | ConvertTo-Json -Depth 3
```

#### 3.2 Lấy Chi tiết Sản phẩm

```powershell
# GET /products/detail/{id}
$productId = 1
$product = Invoke-RestMethod `
-Uri "$API_URL/products/detail/$productId" `
-Method Get

$product | ConvertTo-Json -Depth 3
```

#### 3.3 Tìm kiếm Sản phẩm

```powershell
# GET /products/search?q=keyword
$searchResults = Invoke-RestMethod `
-Uri "$API_URL/products/search?q=phone" `
-Method Get

$searchResults | ConvertTo-Json -Depth 3
```

#### 3.4 Lấy sản phẩm bán chạy nhất

```powershell
# GET /products/best-selling
$bestSelling = Invoke-RestMethod `
-Uri "$API_URL/products/best-selling?limit=10" `
-Method Get

$bestSelling | ConvertTo-Json -Depth 3
```

#### 3.5 Lấy sản phẩm mới nhất

```powershell
# GET /products/newest
$newest = Invoke-RestMethod `
-Uri "$API_URL/products/newest?limit=10" `
-Method Get

$newest | ConvertTo-Json -Depth 3
```

#### 3.6 Lấy sản phẩm theo danh mục

```powershell
# GET /products/category/{id}
$categoryId = 1
$categoryProducts = Invoke-RestMethod `
-Uri "$API_URL/products/category/$categoryId" `
-Method Get

$categoryProducts | ConvertTo-Json -Depth 3
```

#### 3.7 Lấy sản phẩm theo thương hiệu

```powershell
# GET /products/brand/{id}
$brandId = 1
$brandProducts = Invoke-RestMethod `
-Uri "$API_URL/products/brand/$brandId" `
-Method Get

$brandProducts | ConvertTo-Json -Depth 3
```

---

### Bước 4: Kiểm tra API Giỏ hàng

#### 4.1 Thêm vào Giỏ hàng

```powershell
# POST /cart (yêu cầu xác thực)
$cartBody = @{
productDetailId = 1
quantity = 2
} | ConvertTo-Json

$cartResponse = Invoke-RestMethod `
-Uri "$API_URL/cart" `
-Method Post `
-Headers $headers `
-ContentType "application/json" `
-Body $cartBody

$cartResponse | ConvertTo-Json
```

#### 4.2 Lấy Giỏ hàng của tôi

```powershell
# GET /cart/me
$myCart = Invoke-RestMethod`
-Uri "$API_URL/cart/me" `
-Phương thức Lấy `
-Tiêu đề $headers

$myCart | Chuyển đổi sang JSON -Độ sâu 3
```

#### 4.3 Cập nhật mặt hàng trong giỏ hàng

```powershell
# PUT /cart/{id}
$cartItemId = 1
$updateBody = @{
quantity = 3
} | Chuyển đổi sang JSON

$updateResponse = Invoke-RestMethod `
-Uri "$API_URL/cart/$cartItemId" `
-Phương thức Đặt `
-Tiêu đề $headers `
-ContentType "application/json" `
-Body $updateBody

$updateResponse | ConvertTo-Json
```

#### 4.4 Lấy Số lượng Giỏ hàng

```powershell
# GET /cart/count
$cartCount = Invoke-RestMethod `
-Uri "$API_URL/cart/count" `
-Method Get `
-Headers $headers

Write-Host "Số lượng hàng trong giỏ hàng: $($cartCount.count)"
```

#### 4.5 Xóa Hàng trong Giỏ hàng

```powershell
# DELETE /cart/{id}
Invoke-RestMethod `
-Uri "$API_URL/cart/$cartItemId" `
-Method Delete `
-Headers $headers

Write-Host "Số lượng hàng trong giỏ hàng đã bị xóa"
```

---

### Bước 5: Kiểm tra API Đơn hàng

#### 5.1 Tạo Đơn hàng (COD)

```powershell
# POST /orders/create-cod
$orderBody = @{
shippingAddress = "123 Đường Test, Quận 1, Thành phố Hồ Chí Minh"
phone = "+84901234567"
note = "Vui lòng gọi trước khi giao hàng"
} | ConvertTo-Json

$orderResponse = Invoke-RestMethod `
-Uri "$API_URL/orders/create-cod" `
-Method Post `
-Headers $headers `
-ContentType "application/json" `
-Body $orderBody

$ORDER_ID = $orderResponse.orderId
Write-Host "Đơn hàng đã tạo: $ORDER_ID"
$orderResponse | Chuyển đổi sang JSON
```

#### 5.2 Lấy thông tin chi tiết đơn hàng

```powershell
# GET /orders/{id}/details
$orderDetails = Invoke-RestMethod `
-Uri "$API_URL/orders/$ORDER_ID/details" `
-Phương thức Lấy `
-Tiêu đề $headers

$orderDetails | Chuyển đổi sang JSON -Độ sâu 3
```

#### 5.3 Lấy thông tin đơn hàng của người dùng

```powershell
# GET /orders/user/{userId}
$userId = 1
$userOrders = Invoke-RestMethod `
-Uri "$API_URL/orders/user/$userId" `
-Phương thức Lấy `
-Tiêu đề $headers

$userOrders | ConvertTo-Json -Depth 3
```

#### 5.4 Cập nhật Trạng thái Đơn hàng (Quản trị viên)

```powershell
# PATCH /orders/{id}/status
$statusBody = @{
status = "Đang xử lý"
} | ConvertTo-Json

$statusResponse = Invoke-RestMethod `
-Uri "$API_URL/orders/$ORDER_ID/status" `
-Method Patch `
-Headers $headers `
-ContentType "application/json" `
-Body $statusBody

$statusResponse | ConvertTo-Json
```

---

### Bước 6: Kiểm tra API Danh mục

#### 6.1 Lấy tất cả Danh mục

```powershell
# GET /categories
$categories = Invoke-RestMethod `
-Uri "$API_URL/categories" `
-Method Get

$categories | ConvertTo-Json -Depth 3
```

#### 6.2 Lấy Danh mục theo ID

```powershell
# GET /categories/{id}
$categoryId = 1
$category = Invoke-RestMethod `
-Uri "$API_URL/categories/$categoryId" `
-Method Get

$category | ConvertTo-Json
```

#### 6.3 Tìm kiếm Danh mục

```powershell
# GET /categories/search?q=keyword
$categorySearch = Invoke-RestMethod `
-Uri "$API_URL/categories/search?q=phone" `
-Method Get

$categorySearch | ConvertTo-Json
```

---

### Bước 7: Kiểm tra API Brands

#### 7.1 Lấy tất cả các Brands

```powershell
# GET /brands
$brands = Invoke-RestMethod `
-Uri "$API_URL/brands" `
-Method Get

$brands | ConvertTo-Json -Depth 3
```

#### 7.2 Lấy thương hiệu theo ID

```powershell
# GET /brands/{id}
$brandId = 1
$brand = Invoke-RestMethod `
-Uri "$API_URL/brands/$brandId" `
-Method Get

$brand | ConvertTo-Json
```

---

### Bước 8: Kiểm tra API Banners

#### 8.1 Lấy tất cả Banners

```powershell
# GET /banners
$banners = Invoke-RestMethod `
-Uri "$API_URL/banners" `
-Method Get

$banners | ConvertTo-Json -Depth 3
```

#### 8.2 Lấy Banner Đang Hoạt Động

```powershell
# GET /banners?active=true
$activeBanners = Invoke-RestMethod `
-Uri "$API_URL/banners?active=true" `
-Method Get

$activeBanners | ConvertTo-Json -Depth 3
```

---

### Bước 9: Kiểm tra API Xếp hạng

#### 9.1 Lấy Xếp hạng Sản phẩm

```powershell
# GET /ratings/product/{id}
$productId = 1
$ratings = Invoke-RestMethod `
-Uri "$API_URL/ratings/product/$productId" `
-Method Get

$ratings | ConvertTo-Json -Depth 3
```

#### 9.2 Lấy Thống kê Xếp hạng

```powershell
# GET /ratings/product/{id}/stats
$ratingStats = Invoke-RestMethod `
-Uri "$API_URL/ratings/product/$productId/stats" `
-Method Get

$ratingStats | ConvertTo-Json
# Dự kiến: { "averageRating": 4.5, "totalRatings": 10, "distribution": { "5": 5, "4": 3, "3": 2 } }
```

#### 9.3 Tạo Xếp hạng

```powershell
# POST /ratings
$ratingBody = @{
productId = 1
rating = 5
comment = "Sản phẩm tuyệt vời! Rất đáng dùng."
} | ConvertTo-Json

$ratingResponse = Invoke-RestMethod `
-Uri "$API_URL/ratings" `
-Method Post `
-Headers $headers `
-ContentType "application/json" `
-Body $ratingBody

$ratingResponse | ConvertTo-Json
```

---

### Bước 10: Kiểm tra API người dùng (Quản trị viên)

#### 10.1 Lấy tất cả người dùng

```powershell
# GET /users (Chỉ dành cho Quản trị viên)
$users = Invoke-RestMethod `
-Uri "$API_URL/users" `
-Method Get `
-Headers $headers

$users | ConvertTo-Json -Depth 3
```

#### 10.2 Lấy người dùng theo ID

```powershell
# GET /users/{id}
$userId = 1
$user = Invoke-RestMethod `
-Uri "$API_URL/users/$userId" `
-Method Get `
-Headers $headers

$người dùng | Chuyển đổi sang JSON
```

#### 10.3 Cập nhật Hồ sơ Người dùng

```powershell
# PUT /users/profile/{id}
$profileBody = @{
fullName = "Tên đã cập nhật"
phone = "+84909876543"
address = "456 New Street, District 2"
} | Chuyển đổi sang JSON

$profileResponse = Invoke-RestMethod `
-Uri "$API_URL/users/profile/$userId" `
-Method Put `
-Headers $headers `
-ContentType "application/json" `
-Body $profileBody

$profileResponse | ConvertTo-Json
```

---

### Bước 11: Kiểm tra API Tải lên Hình ảnh

#### 11.1 Tải lên Hình ảnh

```powershell
# POST /images/upload
# Lưu ý: Thao tác này yêu cầu multipart/form-data

# Sử dụng curl để tải lên tệp
curl -X POST "$API_URL/images/upload" `

-H "Authorization: Bearer $TOKEN" `

-F "file=@D:\test-image.jpg" `

-F "type=product"

---

### Bước 12: Xác minh Nhật ký CloudWatch

#### 12.1 Kiểm tra Nhật ký Lambda

```powershell
# Nhật ký Mô-đun Tail Auth
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --follow

# Nhật ký Mô-đun Tail Products
aws logs tail /aws/lambda/OJT-Ecommerce-ProductsModule --follow

# Xem 10 phút gần nhất
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --since 10m
```

### Danh sách kiểm tra thử nghiệm

#### API xác thực (4 điểm cuối)
- [ ] `POST /auth/signup` - Đăng ký người dùng
- [ ] `POST /auth/login` - Đăng nhập người dùng, nhận mã thông báo JWT
- [ ] `POST /auth/logout` - Đăng xuất người dùng
- [ ] `GET /auth/me` - Nhận người dùng hiện tại

#### API sản phẩm (12 điểm cuối)
- [ ] `GET /products` - Liệt kê tất cả sản phẩm
- [ ] `GET /products/detail/{id}` - Nhận chi tiết sản phẩm
- [ ] `GET /products/search` - Tìm kiếm sản phẩm
- [ ] `GET /products/best-selling` - Sản phẩm bán chạy nhất
- [ ] `GET /products/newest` - Sản phẩm mới nhất
- [ ] `GET /products/category/{id}` - Sản phẩm theo danh mục
- [ ] `GET /products/brand/{id}` - Sản phẩm theo thương hiệu
- [ ] `GET /products/price-range` - Sản phẩm theo khoảng giá
- [ ] `POST /products` - Tạo sản phẩm (Quản trị viên)
- [ ] `PUT /products/{id}` - Cập nhật sản phẩm (Quản trị viên)
- [ ] `DELETE /products/{id}` - Xóa sản phẩm (Quản trị viên)

#### API Giỏ hàng (6 điểm cuối)
- [ ] `POST /cart` - Thêm vào giỏ hàng
- [ ] `GET /cart/me` - Lấy giỏ hàng của tôi
- [ ] `PUT /cart/{id}` - Cập nhật giỏ hàng mục
- [ ] `DELETE /cart/{id}` - Xóa mục trong giỏ hàng
- [ ] `DELETE /cart` - Xóa giỏ hàng
- [ ] `GET /cart/count` - Lấy số lượng giỏ hàng

#### API Đơn hàng (9 điểm cuối)
- [ ] `POST /orders` - Tạo đơn hàng
- [ ] `POST /orders/create-cod` - Tạo đơn hàng COD
- [ ] `GET /orders/{id}/details` - Lấy thông tin chi tiết đơn hàng
- [ ] `GET /orders/user/{userId}` - Lấy đơn hàng của người dùng
- [ ] `GET /orders` - Lấy tất cả đơn hàng (Quản trị viên)
- [ ] `PATCH /orders/{id}/status` - Cập nhật trạng thái đơn hàng
- [ ] `DELETE /orders/{id}` - Hủy đơn hàng

#### API Danh mục (6 điểm cuối)
- [ ] `GET /categories` - Liệt kê tất cả các danh mục
- [ ] `GET /categories/{id}` - Lấy danh mục theo ID
- [ ] `GET /categories/search` - Tìm kiếm danh mục
- [ ] `POST /categories` - Tạo danh mục (Quản trị viên)
- [ ] `PUT /categories/{id}` - Cập nhật danh mục (Quản trị viên)
- [ ] `DELETE /categories/{id}` - Xóa danh mục (Quản trị viên)

#### API Thương hiệu (5 điểm cuối)
- [ ] `GET /brands` - Liệt kê tất cả các thương hiệu
- [ ] `GET /brands/{id}` - Lấy thương hiệu theo ID
- [ ] `POST /brands` - Tạo thương hiệu (Quản trị viên)
- [ ] `PUT /brands/{id}` - Cập nhật thương hiệu (Quản trị viên)
- [ ] `DELETE /brands/{id}` - Xóa thương hiệu (Quản trị viên)

#### API Banners (7 điểm cuối)
- [ ] `GET /banners` - Liệt kê tất cả banners
- [ ] `GET /banners/{id}` - Lấy banners theo ID
- [ ] `GET /banners?active=true` - Lấy banners đang hoạt động
- [ ] `POST /banners` - Tạo banners (Quản trị viên)
- [ ] `PUT /banners/{id}` - Cập nhật banners (Quản trị viên)
- [ ] `DELETE /banners/{id}` - Xóa banners (Quản trị viên)
- [ ] `PATCH /banners/{id}/toggle` - Bật/tắt banners (Quản trị viên)

#### API Xếp hạng (3 điểm cuối)
- [ ] `GET /ratings/product/{id}` - Lấy xếp hạng sản phẩm
- [ ] `GET /ratings/product/{id}/stats` - Lấy số liệu thống kê xếp hạng
- [ ] `POST /ratings` - Tạo xếp hạng

#### API Người dùng (3 điểm cuối)
- [ ] `GET /users` - Lấy tất cả người dùng (Quản trị viên)
- [ ] `GET /users/{id}` - Lấy người dùng theo ID
- [ ] `PUT /users/profile/{id}` - Cập nhật hồ sơ

#### API Hình ảnh (1 điểm cuối)
- [ ] `POST /images/upload` - Tải hình ảnh lên

---

### Điểm chuẩn Hiệu suất

**Thời gian Phản hồi Dự kiến:**

| Điểm cuối | Thời gian Dự kiến ​​| Ghi chú |
|----------|--------------|-------|
| `POST /auth/login` | < 500ms | Tạo JWT |
| `GET /products` | < 300ms | Truy vấn Cơ sở dữ liệu |
| `GET /products/detail/{id}` | < 200ms | Bản ghi đơn lẻ |
| `POST /cart` | < 300ms | Ghi cơ sở dữ liệu |
| `POST /orders/create-cod` | < 500ms | Giao dịch |
| `GET /categories` | < 200ms | Dữ liệu được lưu trong bộ nhớ đệm |
| `POST /images/upload` | < 2 giây | Tải lên S3 |

---