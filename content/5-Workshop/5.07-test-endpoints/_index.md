---
title: "Test Endpoints End-to-End"
date: 2025-12-01
weight: 7
chapter: false
pre: " <b> 5.07. </b> "
---

#### Overview

Sau khi deploy backend thành công, bạn cần test tất cả API endpoints để đảm bảo hệ thống hoạt động đúng. Workshop này hướng dẫn chi tiết cách test từng module của hệ thống OJT E-commerce.

#### API Architecture

```
API Gateway (REST API)
    ↓
┌─────────────────────────────────────────────────────────────────┐
│                    11 Lambda Modules (63 APIs)                   │
├─────────┬─────────┬─────────┬─────────┬─────────┬───────────────┤
│  Auth   │Products │  Cart   │ Orders  │Categories│   Brands     │
│(4 APIs) │(12 APIs)│(6 APIs) │(9 APIs) │ (6 APIs) │  (5 APIs)    │
├─────────┼─────────┼─────────┼─────────┼─────────┼───────────────┤
│ Banners │ Ratings │  Users  │ Images  │ Product │               │
│(7 APIs) │(3 APIs) │(3 APIs) │(1 API)  │ Details │               │
│         │         │         │         │(7 APIs) │               │
└─────────┴─────────┴─────────┴─────────┴─────────┴───────────────┘
    ↓
RDS SQL Server (via Secrets Manager)
```

---

### Step 1: Get API Endpoint

**1. Get API URL from CloudFormation Outputs**

```powershell
# Get API endpoint from API Stack
$API_URL = aws cloudformation describe-stacks `
  --stack-name OJT-ApiStack `
  --query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' `
  --output text

Write-Host "API Endpoint: $API_URL"
# Output: https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod
```

**2. Setup Environment Variables**

```powershell
# Set API endpoint for PowerShell session
$API_URL = "https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod"

Write-Host "API Endpoint configured: $API_URL"
```

---

### Step 2: Test Authentication APIs

#### 2.1 Test User Signup

```powershell
# POST /auth/signup
$signupBody = @{
  email = "testuser@example.com"
  password = "Test123!"
  fullName = "Test User"
  phone = "+84901234567"
} | ConvertTo-Json

$response = Invoke-RestMethod `
  -Uri "$API_URL/auth/signup" `
  -Method Post `
  -ContentType "application/json" `
  -Body $signupBody

$response | ConvertTo-Json

```



#### 2.2 Test User Login

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

# Save JWT token for authenticated requests
$TOKEN = $loginResponse.token
Write-Host "JWT Token: $TOKEN"

$loginResponse | ConvertTo-Json
# Expected: { "token": "eyJhbG...", "user": { "userId": 1, "email": "...", "role": "Customer" } }
```

#### 2.3 Test Get Current User

```powershell
# GET /auth/me (requires authentication)
$headers = @{
  "Authorization" = "Bearer $TOKEN"
}

$meResponse = Invoke-RestMethod `
  -Uri "$API_URL/auth/me" `
  -Method Get `
  -Headers $headers

$meResponse | ConvertTo-Json
# Expected: { "user": { "userId": 1, "email": "testuser@example.com", "fullName": "Test User" } }
```

---

### Step 3: Test Products APIs

#### 3.1 Get All Products

```powershell
# GET /products
$products = Invoke-RestMethod `
  -Uri "$API_URL/products?page=1&limit=10" `
  -Method Get

Write-Host "Found $($products.total) products"
$products | ConvertTo-Json -Depth 3
```

#### 3.2 Get Product Detail

```powershell
# GET /products/detail/{id}
$productId = 1
$product = Invoke-RestMethod `
  -Uri "$API_URL/products/detail/$productId" `
  -Method Get

$product | ConvertTo-Json -Depth 3
```

#### 3.3 Search Products

```powershell
# GET /products/search?q=keyword
$searchResults = Invoke-RestMethod `
  -Uri "$API_URL/products/search?q=phone" `
  -Method Get

$searchResults | ConvertTo-Json -Depth 3
```

#### 3.4 Get Best Selling Products

```powershell
# GET /products/best-selling
$bestSelling = Invoke-RestMethod `
  -Uri "$API_URL/products/best-selling?limit=10" `
  -Method Get

$bestSelling | ConvertTo-Json -Depth 3
```

#### 3.5 Get Newest Products

```powershell
# GET /products/newest
$newest = Invoke-RestMethod `
  -Uri "$API_URL/products/newest?limit=10" `
  -Method Get

$newest | ConvertTo-Json -Depth 3
```

#### 3.6 Get Products by Category

```powershell
# GET /products/category/{id}
$categoryId = 1
$categoryProducts = Invoke-RestMethod `
  -Uri "$API_URL/products/category/$categoryId" `
  -Method Get

$categoryProducts | ConvertTo-Json -Depth 3
```

#### 3.7 Get Products by Brand

```powershell
# GET /products/brand/{id}
$brandId = 1
$brandProducts = Invoke-RestMethod `
  -Uri "$API_URL/products/brand/$brandId" `
  -Method Get

$brandProducts | ConvertTo-Json -Depth 3
```

---

### Step 4: Test Cart APIs

#### 4.1 Add to Cart

```powershell
# POST /cart (requires authentication)
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

#### 4.2 Get My Cart

```powershell
# GET /cart/me
$myCart = Invoke-RestMethod `
  -Uri "$API_URL/cart/me" `
  -Method Get `
  -Headers $headers

$myCart | ConvertTo-Json -Depth 3
```

#### 4.3 Update Cart Item

```powershell
# PUT /cart/{id}
$cartItemId = 1
$updateBody = @{
  quantity = 3
} | ConvertTo-Json

$updateResponse = Invoke-RestMethod `
  -Uri "$API_URL/cart/$cartItemId" `
  -Method Put `
  -Headers $headers `
  -ContentType "application/json" `
  -Body $updateBody

$updateResponse | ConvertTo-Json
```

#### 4.4 Get Cart Count

```powershell
# GET /cart/count
$cartCount = Invoke-RestMethod `
  -Uri "$API_URL/cart/count" `
  -Method Get `
  -Headers $headers

Write-Host "Cart items: $($cartCount.count)"
```

#### 4.5 Remove Cart Item

```powershell
# DELETE /cart/{id}
Invoke-RestMethod `
  -Uri "$API_URL/cart/$cartItemId" `
  -Method Delete `
  -Headers $headers

Write-Host "Cart item removed"
```

---

### Step 5: Test Orders APIs

#### 5.1 Create Order (COD)

```powershell
# POST /orders/create-cod
$orderBody = @{
  shippingAddress = "123 Test Street, District 1, Ho Chi Minh City"
  phone = "+84901234567"
  note = "Please call before delivery"
} | ConvertTo-Json

$orderResponse = Invoke-RestMethod `
  -Uri "$API_URL/orders/create-cod" `
  -Method Post `
  -Headers $headers `
  -ContentType "application/json" `
  -Body $orderBody

$ORDER_ID = $orderResponse.orderId
Write-Host "Order created: $ORDER_ID"
$orderResponse | ConvertTo-Json
```

#### 5.2 Get Order Details

```powershell
# GET /orders/{id}/details
$orderDetails = Invoke-RestMethod `
  -Uri "$API_URL/orders/$ORDER_ID/details" `
  -Method Get `
  -Headers $headers

$orderDetails | ConvertTo-Json -Depth 3
```

#### 5.3 Get User Orders

```powershell
# GET /orders/user/{userId}
$userId = 1
$userOrders = Invoke-RestMethod `
  -Uri "$API_URL/orders/user/$userId" `
  -Method Get `
  -Headers $headers

$userOrders | ConvertTo-Json -Depth 3
```

#### 5.4 Update Order Status (Admin)

```powershell
# PATCH /orders/{id}/status
$statusBody = @{
  status = "Processing"
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

### Step 6: Test Categories APIs

#### 6.1 Get All Categories

```powershell
# GET /categories
$categories = Invoke-RestMethod `
  -Uri "$API_URL/categories" `
  -Method Get

$categories | ConvertTo-Json -Depth 3
```

#### 6.2 Get Category by ID

```powershell
# GET /categories/{id}
$categoryId = 1
$category = Invoke-RestMethod `
  -Uri "$API_URL/categories/$categoryId" `
  -Method Get

$category | ConvertTo-Json
```

#### 6.3 Search Categories

```powershell
# GET /categories/search?q=keyword
$categorySearch = Invoke-RestMethod `
  -Uri "$API_URL/categories/search?q=phone" `
  -Method Get

$categorySearch | ConvertTo-Json
```

---

### Step 7: Test Brands APIs

#### 7.1 Get All Brands

```powershell
# GET /brands
$brands = Invoke-RestMethod `
  -Uri "$API_URL/brands" `
  -Method Get

$brands | ConvertTo-Json -Depth 3
```

#### 7.2 Get Brand by ID

```powershell
# GET /brands/{id}
$brandId = 1
$brand = Invoke-RestMethod `
  -Uri "$API_URL/brands/$brandId" `
  -Method Get

$brand | ConvertTo-Json
```

---

### Step 8: Test Banners APIs

#### 8.1 Get All Banners

```powershell
# GET /banners
$banners = Invoke-RestMethod `
  -Uri "$API_URL/banners" `
  -Method Get

$banners | ConvertTo-Json -Depth 3
```

#### 8.2 Get Active Banners

```powershell
# GET /banners?active=true
$activeBanners = Invoke-RestMethod `
  -Uri "$API_URL/banners?active=true" `
  -Method Get

$activeBanners | ConvertTo-Json -Depth 3
```

---

### Step 9: Test Ratings APIs

#### 9.1 Get Product Ratings

```powershell
# GET /ratings/product/{id}
$productId = 1
$ratings = Invoke-RestMethod `
  -Uri "$API_URL/ratings/product/$productId" `
  -Method Get

$ratings | ConvertTo-Json -Depth 3
```

#### 9.2 Get Rating Stats

```powershell
# GET /ratings/product/{id}/stats
$ratingStats = Invoke-RestMethod `
  -Uri "$API_URL/ratings/product/$productId/stats" `
  -Method Get

$ratingStats | ConvertTo-Json
# Expected: { "averageRating": 4.5, "totalRatings": 10, "distribution": { "5": 5, "4": 3, "3": 2 } }
```

#### 9.3 Create Rating

```powershell
# POST /ratings
$ratingBody = @{
  productId = 1
  rating = 5
  comment = "Great product! Highly recommended."
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

### Step 10: Test Users APIs (Admin)

#### 10.1 Get All Users

```powershell
# GET /users (Admin only)
$users = Invoke-RestMethod `
  -Uri "$API_URL/users" `
  -Method Get `
  -Headers $headers

$users | ConvertTo-Json -Depth 3
```

#### 10.2 Get User by ID

```powershell
# GET /users/{id}
$userId = 1
$user = Invoke-RestMethod `
  -Uri "$API_URL/users/$userId" `
  -Method Get `
  -Headers $headers

$user | ConvertTo-Json
```

#### 10.3 Update User Profile

```powershell
# PUT /users/profile/{id}
$profileBody = @{
  fullName = "Updated Name"
  phone = "+84909876543"
  address = "456 New Street, District 2"
} | ConvertTo-Json

$profileResponse = Invoke-RestMethod `
  -Uri "$API_URL/users/profile/$userId" `
  -Method Put `
  -Headers $headers `
  -ContentType "application/json" `
  -Body $profileBody

$profileResponse | ConvertTo-Json
```

---

### Step 11: Test Images Upload API

#### 11.1 Upload Image

```powershell
# POST /images/upload
# Note: This requires multipart/form-data

# Using curl for file upload
curl -X POST "$API_URL/images/upload" `
  -H "Authorization: Bearer $TOKEN" `
  -F "file=@D:\test-image.jpg" `
  -F "type=product"


---

### Step 12: Verify CloudWatch Logs

#### 12.1 Check Lambda Logs

```powershell
# Tail Auth Module logs
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --follow

# Tail Products Module logs
aws logs tail /aws/lambda/OJT-Ecommerce-ProductsModule --follow

# View last 10 minutes
aws logs tail /aws/lambda/OJT-Ecommerce-AuthModule --since 10m
```


### Testing Checklist

#### Authentication APIs (4 endpoints)
- [ ] `POST /auth/signup` - User registration
- [ ] `POST /auth/login` - User login, get JWT token
- [ ] `POST /auth/logout` - User logout
- [ ] `GET /auth/me` - Get current user

#### Products APIs (12 endpoints)
- [ ] `GET /products` - List all products
- [ ] `GET /products/detail/{id}` - Get product detail
- [ ] `GET /products/search` - Search products
- [ ] `GET /products/best-selling` - Best selling products
- [ ] `GET /products/newest` - Newest products
- [ ] `GET /products/category/{id}` - Products by category
- [ ] `GET /products/brand/{id}` - Products by brand
- [ ] `GET /products/price-range` - Products by price range
- [ ] `POST /products` - Create product (Admin)
- [ ] `PUT /products/{id}` - Update product (Admin)
- [ ] `DELETE /products/{id}` - Delete product (Admin)

#### Cart APIs (6 endpoints)
- [ ] `POST /cart` - Add to cart
- [ ] `GET /cart/me` - Get my cart
- [ ] `PUT /cart/{id}` - Update cart item
- [ ] `DELETE /cart/{id}` - Remove cart item
- [ ] `DELETE /cart` - Clear cart
- [ ] `GET /cart/count` - Get cart count

#### Orders APIs (9 endpoints)
- [ ] `POST /orders` - Create order
- [ ] `POST /orders/create-cod` - Create COD order
- [ ] `GET /orders/{id}/details` - Get order details
- [ ] `GET /orders/user/{userId}` - Get user orders
- [ ] `GET /orders` - Get all orders (Admin)
- [ ] `PATCH /orders/{id}/status` - Update order status
- [ ] `DELETE /orders/{id}` - Cancel order

#### Categories APIs (6 endpoints)
- [ ] `GET /categories` - List all categories
- [ ] `GET /categories/{id}` - Get category by ID
- [ ] `GET /categories/search` - Search categories
- [ ] `POST /categories` - Create category (Admin)
- [ ] `PUT /categories/{id}` - Update category (Admin)
- [ ] `DELETE /categories/{id}` - Delete category (Admin)

#### Brands APIs (5 endpoints)
- [ ] `GET /brands` - List all brands
- [ ] `GET /brands/{id}` - Get brand by ID
- [ ] `POST /brands` - Create brand (Admin)
- [ ] `PUT /brands/{id}` - Update brand (Admin)
- [ ] `DELETE /brands/{id}` - Delete brand (Admin)

#### Banners APIs (7 endpoints)
- [ ] `GET /banners` - List all banners
- [ ] `GET /banners/{id}` - Get banner by ID
- [ ] `GET /banners?active=true` - Get active banners
- [ ] `POST /banners` - Create banner (Admin)
- [ ] `PUT /banners/{id}` - Update banner (Admin)
- [ ] `DELETE /banners/{id}` - Delete banner (Admin)
- [ ] `PATCH /banners/{id}/toggle` - Toggle banner (Admin)

#### Ratings APIs (3 endpoints)
- [ ] `GET /ratings/product/{id}` - Get product ratings
- [ ] `GET /ratings/product/{id}/stats` - Get rating stats
- [ ] `POST /ratings` - Create rating

#### Users APIs (3 endpoints)
- [ ] `GET /users` - Get all users (Admin)
- [ ] `GET /users/{id}` - Get user by ID
- [ ] `PUT /users/profile/{id}` - Update profile

#### Images API (1 endpoint)
- [ ] `POST /images/upload` - Upload image

---

### Performance Benchmarks

**Expected Response Times:**

| Endpoint | Expected Time | Notes |
|----------|--------------|-------|
| `POST /auth/login` | < 500ms | JWT generation |
| `GET /products` | < 300ms | Database query |
| `GET /products/detail/{id}` | < 200ms | Single record |
| `POST /cart` | < 300ms | Database write |
| `POST /orders/create-cod` | < 500ms | Transaction |
| `GET /categories` | < 200ms | Cached data |
| `POST /images/upload` | < 2s | S3 upload |

---

