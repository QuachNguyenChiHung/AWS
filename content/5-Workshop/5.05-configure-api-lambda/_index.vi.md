---
title: "Cấu hình API & Lambda"
date: 2025-12-01
weight: 6
chapter: false
pre: " <b> 5.06. </b> "
---

#### Tổng quan

Sau khi triển khai cơ sở hạ tầng, bạn cần cấu hình các tuyến API Gateway và các hàm Lambda để xử lý logic nghiệp vụ. Dự án Thương mại điện tử OJT sử dụng kiến ​​trúc triển khai 2 bước: Cơ sở hạ tầng (CDK) và Mã Lambda (riêng biệt).

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
↓ ↓ ↓ ↓
Hình ảnh RDS SQL Server S3
(qua Secrets Quản lý)
```

#### Bước 1: Xem lại Cấu trúc Dự án

**Cấu trúc Hàm Lambda:**

```
OJT_lambda/
├── shared/ # Tiện ích dùng chung
│ ├── database.js # Trình trợ giúp kết nối RDS
│ ├── auth.js # Tiện ích JWT
│ └── response.js # Trình định dạng phản hồi API
├── auth/ # Xác thực (4 hàm)
│ ├── login.js # POST /auth/login
│ ├── signup.js # POST /auth/signup
│ ├── logout.js # POST /auth/logout
│ └── me.js # GET /auth/me
├── products/ # Sản phẩm (12 hàm)
│ ├── getProducts.js # GET /products
│ ├── getProductDetail.js # GET /products/detail/{id}
│ ├── createProduct.js # POST /products
│ ├── updateProduct.js # PUT /products/{id}
│ ├── deleteProduct.js # DELETE /products/{id}
│ ├── searchProducts.js # GET /products/search
│ ├── getBestSelling.js # GET /products/best-selling
│ ├── getNewest.js # GET /products/newest
│ └── ...
├── cart/ # Cart (6 chức năng)
├── orders/ # Orders (9 chức năng)
├── categories/ # Categories (6 chức năng)
├── brands/ # Brands (5 chức năng)
├── banners/ # Banners (7 chức năng)
├── ratings/ # Ratings (3 chức năng)
├── users/ # Users (3 chức năng)
├── images/ # Images (1 chức năng)
└── product-details/ # Product Details (7 chức năng)
```

#### Bước 2: Cấu hình Biến Môi trường Lambda

**1. Điều hướng đến dự án Lambda**

```powershell
cd OJT_lambda
```

**2. Sao chép mẫu môi trường**

```powershell
sao chép .env.example .env
```

**3. Chỉnh sửa tệp .env**

```bash
# Cấu hình AWS
AWS_REGION=ap-southeast-1
AWS_ACCOUNT_ID=123456789012

# Cấu hình cơ sở dữ liệu (từ đầu ra CDK)
DB_HOST=ojt-database.xxx.ap-southeast-1.rds.amazonaws.com
DB_NAME=demoaws
DB_SECRET_ARN=arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:OJT/RDS/Credentials

# Cấu hình JWT
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d

# Thùng chứa hình ảnh S3 (từ CDK outputs)
S3_IMAGES_BUCKET=ojt-ecommerce-images-123456789012
```

**4. Lấy giá trị từ đầu ra CDK**

```powershell
# Lấy điểm cuối RDS
aws rds describe-db-instances `
--db-instance-identifier ojt-database `
--query 'DBInstances[0].Endpoint.Address' `
--output text

# Lấy ARN của Secrets Manager
aws secretsmanager list-secrets `
--query "SecretList[?contains(Name, 'OJT')].ARN" `
--output text

# Lấy tên bucket S3
aws s3 ls | Select-String "ojt-ecommerce-images"
```

#### Bước 3: Xem lại các điểm cuối API

**API xác thực (4 điểm cuối):**

| Phương thức | Điểm cuối | Trình xử lý | Mô tả |
|--------|----------|---------|-------------|
| POST | `/auth/login` | login.js | Đăng nhập người dùng |
| POST | `/auth/signup` | signup.js | Đăng ký người dùng |
| POST | `/auth/logout` | logout.js | Đăng xuất người dùng |
| GET | `/auth/me` | me.js | Lấy người dùng hiện tại |

**API Sản phẩm (12 điểm cuối):**

| Phương thức | Điểm cuối | Trình xử lý | Mô tả |
|--------|----------|---------|-------------|
| GET | `/products` | getProducts.js | Liệt kê tất cả sản phẩm |
| GET | `/products/detail/{id}` | getProductDetail.js | Chi tiết sản phẩm |
| POST | `/products` | createProduct.js | Tạo sản phẩm (Quản trị viên) |
| PUT | `/products/{id}` | updateProduct.js | Cập nhật sản phẩm (Quản trị) |
| XÓA | `/products/{id}` | deleteProduct.js | Xóa sản phẩm (Quản trị) |
| GET | `/products/search` | searchProducts.js | Tìm kiếm sản phẩm |
| GET | `/products/best-selling` | getBestSelling.js | Sản phẩm bán chạy nhất |
| GET | `/products/newest` | getNewest.js | Sản phẩm mới nhất |
| GET | `/products/category/{id}` | getProductsByCategory.js | Sản phẩm theo danh mục |
| GET | `/products/brand/{id}` | getProductsByBrand.js | Sản phẩm theo thương hiệu |
| GET | `/products/price-range` | getProductsByPriceRange.js | Sản phẩm theo giá |

**Ô tôAPI t (6 điểm cuối):**

| Phương thức | Điểm cuối | Trình xử lý | Mô tả |
|--------|----------|---------|-------------|

| POST | `/cart` | addToCart.js | Thêm vào giỏ hàng |
| GET | `/cart/me` | getMyCart.js | Lấy giỏ hàng của người dùng |
| PUT | `/cart/{id}` | updateCartItem.js | Cập nhật mặt hàng trong giỏ hàng |
| DELETE | `/cart/{id}` | removeCartItem.js | Xóa mặt hàng trong giỏ hàng |
| DELETE | `/cart` | clearCart.js | Xóa giỏ hàng |
| GET | `/cart/count` | getCartCount.js | Lấy số lượng giỏ hàng |

**API Đơn hàng (9 điểm cuối):**

| Phương thức | Điểm cuối | Trình xử lý | Mô tả |
|--------|--------|-----------|-------------|
| GET | `/orders` | getAllOrders.js | Tất cả đơn hàng (Quản trị viên) |
| POST | `/orders` | createOrder.js | Tạo đơn hàng |
| POST | `/orders/create-cod` | createOrderCOD.js | Tạo đơn hàng COD |
| GET | `/orders/{id}/details` | getOrderDetails.js | Chi tiết đơn hàng |
| GET | `/orders/user/{userId}` | getUserOrders.js | Đơn hàng của người dùng |
| PATCH | `/orders/{id}/status` | updateOrderStatus.js | Cập nhật trạng thái |
| DELETE | `/orders/{id}` | cancelOrder.js | Hủy đơn hàng |

#### Bước 4: Cài đặt các phụ thuộc Lambda

```powershell
# Cài đặt các phụ thuộc chính
cd OJT_lambda
npm install

# Cài đặt tất cả các phụ thuộc module
npm run install:all
```

Lệnh này sẽ cài đặt các phụ thuộc cho:
- `shared/` - Cơ sở dữ liệu, xác thực, tiện ích phản hồi
- `auth/` - bcryptjs, jsonwebtoken
- `products/` - Truy vấn cơ sở dữ liệu
- `cart/`, `orders/`, v.v.

#### Bước 5: Xem lại các tiện ích dùng chung

**Trình trợ giúp cơ sở dữ liệu (`shared/database.js`):**

```javascript
const sql = require('mssql');

const config = {
máy chủ: process.env.DB_HOST,
cơ sở dữ liệu: process.env.DB_NAME,
người dùng: process.env.DB_USERNAME,
mật khẩu: process.env.DB_PASSWORD,
tùy chọn: {
mã hóa: đúng,
trustServerCertificate: đúng
}
};

hàm async query(sqlQuery, params = []) {
const pool = await sql.connect(config);
const result = await pool.request();
// Thêm tham số
params.forEach((param, index) => {
result.input(`param${index}`, param);
});
return result.query(sqlQuery);
}

module.exports = {query, sql };
```

**Trợ lý xác thực (`shared/auth.js`):**

```javascript
const jwt = require('jsonwebtoken');

hàm generateToken(người dùng) {
trả về jwt.sign(
{userId: user.UserID, email: user.Email, vai trò: user.Role },
process.env.JWT_SECRET,
{ expiresIn: process.env.JWT_EXPIRES_IN || '7d' }
);
}

hàm verifyToken(mã thông báo) {
trả về jwt.verify(mã thông báo, process.env.JWT_SECRET);
}

module.exports = {generateToken, verifyToken };

```

**Trợ lý phản hồi (`shared/response.js`):**

```javascript
hàm success(data, statusCode = 200) {

trả về {
statusCode,
headers: {
'Content-Type': 'application/json',
'Access-Control-Allow-Origin': '*'
},
body: JSON.stringify(data)
};
}

hàm error(message, statusCode = 500) {

trả về {
statusCode,
headers: {
'Content-Type': 'application/json',
'Access-Control-Allow-Origin': '*'
},
body: JSON.stringify({ error: message })
};
}

module.exports = { success, error };
```

#### Bước 6: Xác minh cấu hình API Gateway

**1. Lấy URL API Gateway từ đầu ra CDK**

```powershell
# Lấy URL API Gateway
aws cloudformation describe-stacks `
--stack-name OJT-ApiStack `
--query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' `
--output text
```

Đầu ra dự kiến:
```
https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod
```

**2. Kiểm tra tài nguyên API Gateway**

```powershell
# Lấy ID API
$API_ID = aws apigateway get-rest-apis `
--query "items[?contains(name, 'OJT')].id" `
--output text

# Liệt kê tài nguyên
aws apigateway get-resources --rest-api-id $API_ID
```

#### Bước 7: Xác minh các hàm Lambda

**1. Liệt kê các hàm Lambda**

```powershell
aws lambda list-functions `
--query "Functions[?contains(FunctionName, 'OJT-Ecommerce')].FunctionName" `
--output table
```

Đầu ra dự kiến:
```
OJT-Ecommerce-AuthModule
OJT-Ecommerce-ProductsModule
OJT-Ecommerce-ProductDetailsModule
OJT-Ecommerce-CartModule
OJT-Ecommerce-OrdersModule
OJT-Ecommerce-CategoriesModule
OJT-Ecommerce-BrandsModule
OJT-Ecommerce-BannersModule
OJT-Ecommerce-RatingsModule
OJT-Ecommerce-UsersModule
OJT-Ecommerce-ImagesModule
```

**2. Kiểm tra biến môi trường Lambda**

```powershell
aws lambda get-function-configuration `
--function-name OJT-Ecommerce-AuthModule `
--query 'Environment.Variables'
```

#### Bước 8: Kiểm tra các hàm Lambda cục bộ

**1. Kiểm tra Đăng nhập Xác thực**

```powershell
cd OJT_lambda

# Trình xử lý đăng nhập kiểm tra
node -e "
const handler = require('./auth/login.js').handler;
const event = {
body: JSON.stringify({
email: 'test@test.com',
password: 'Test123!'
})
};
handler(event).then(console.log);
"
```

**2. Kiểm tra Danh sách Sản phẩm**

```powershell
# Kiểm tra trình xử lý lấy sản phẩm
node -e "
const handler = require('./products/getProducts.js').handler;
const event = {
queryStringParameters: { page: '1', limit: '10' }
};
handler(event).then(console.log);
"
```

#### Bước 9: Xây dựng các Gói Lambda

```powershell
# Xây dựng tất cả Lambdmột gói
npm run build

# Hoặc xây dựng mô-đun cụ thể
npm run build:auth
npm run build:products
```

Lệnh này tạo các tệp ZIP trong thư mục `build/`:
```
build/
├── auth.zip
├── products.zip
├── product-details.zip
├── cart.zip
├── orders.zip
├── categories.zip
├── brands.zip
├── banners.zip
├── ratings.zip
├── users.zip
└── images.zip
```

#### Danh sách kiểm tra cấu hình

- [ ] Xem xét cấu trúc dự án Lambda
- [ ] Biến môi trường được cấu hình trong `.env`
- [ ] Chi tiết kết nối cơ sở dữ liệu được lấy từ đầu ra CDK
- [ ] Cấu hình bí mật JWT
- [ ] Cấu hình tên bucket S3
- [ ] Các dependency đã được cài đặt (`npm install` + `npm run install:all`)
- [ ] Các tiện ích dùng chung đã được xem xét (cơ sở dữ liệu, xác thực, phản hồi)
- [ ] Đã lấy URL API Gateway
- [ ] Liệt kê và xác minh các hàm Lambda
- [ ] Các bài kiểm tra cục bộ đã đạt yêu cầu
- [ ] Các gói Lambda đã được xây dựng (`npm run build`)

#### Tóm tắt API

| Mô-đun | Hàm | Điểm cuối |
|--------|-----------|-----------|
| Xác thực | 4 | đăng nhập, đăng ký, đăng xuất, tôi |
| Sản phẩm | 12 | CRUD + tìm kiếm, lọc, bán chạy nhất, mới nhất |
| Chi tiết sản phẩm | 7 | CRUD + tải lên hình ảnh |
| Giỏ hàng | 6 | thêm, lấy, cập nhật, xóa, xóa, đếm |
| Đơn hàng | 9 | CRUD + COD, trạng thái, khoảng thời gian |
| Danh mục | 6 | CRUD + tìm kiếm |
| Thương hiệu | 5 | CRUD |
| Biểu ngữ | 7 | CRUD + chuyển đổi |
| Xếp hạng | 3 | lấy, thống kê, tạo |
| Người dùng | 3 | lấyTất cả, lấyTheoId, cập nhậtHồ sơ |
| Hình ảnh | 1 | tải lên |
| **Tổng cộng** | **63** | |

#### Các bước tiếp theo

Sau khi cấu hình hoàn tất, hãy chuyển đến [Triển khai Backend] để triển khai mã Lambda của bạn lên AWS.