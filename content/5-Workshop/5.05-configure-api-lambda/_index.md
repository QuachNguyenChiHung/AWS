---
title: "Configure API & Lambda"
date: 2025-12-01
weight: 5
chapter: false
pre: " <b> 5.05. </b> "
---

#### Overview

Sau khi deploy infrastructure, bạn cần cấu hình API Gateway routes và Lambda functions để xử lý business logic. Dự án OJT E-commerce sử dụng kiến trúc 2-step deployment: Infrastructure (CDK) và Lambda Code (riêng biệt).

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
    ↓           ↓         ↓         ↓         ↓
RDS SQL Server                              S3 Images
(via Secrets Manager)
```

#### Step 1: Review Project Structure

**Lambda Functions Structure:**

```
OJT_lambda/
├── shared/                  # Shared utilities
│   ├── database.js          # RDS connection helper
│   ├── auth.js              # JWT utilities
│   └── response.js          # API response formatters
├── auth/                    # Authentication (4 functions)
│   ├── login.js             # POST /auth/login
│   ├── signup.js            # POST /auth/signup
│   ├── logout.js            # POST /auth/logout
│   └── me.js                # GET /auth/me
├── products/                # Products (12 functions)
│   ├── getProducts.js       # GET /products
│   ├── getProductDetail.js  # GET /products/detail/{id}
│   ├── createProduct.js     # POST /products
│   ├── updateProduct.js     # PUT /products/{id}
│   ├── deleteProduct.js     # DELETE /products/{id}
│   ├── searchProducts.js    # GET /products/search
│   ├── getBestSelling.js    # GET /products/best-selling
│   ├── getNewest.js         # GET /products/newest
│   └── ...
├── cart/                    # Cart (6 functions)
├── orders/                  # Orders (9 functions)
├── categories/              # Categories (6 functions)
├── brands/                  # Brands (5 functions)
├── banners/                 # Banners (7 functions)
├── ratings/                 # Ratings (3 functions)
├── users/                   # Users (3 functions)
├── images/                  # Images (1 function)
└── product-details/         # Product Details (7 functions)
```

#### Step 2: Configure Lambda Environment Variables

**1. Navigate to Lambda project**

```powershell
cd OJT_lambda
```

**2. Copy environment template**

```powershell
copy .env.example .env
```

**3. Edit .env file**

```bash
# AWS Configuration
AWS_REGION=ap-southeast-1
AWS_ACCOUNT_ID=123456789012

# Database Configuration (from CDK outputs)
DB_HOST=ojt-database.xxx.ap-southeast-1.rds.amazonaws.com
DB_NAME=demoaws
DB_SECRET_ARN=arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:OJT/RDS/Credentials

# JWT Configuration
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d

# S3 Images Bucket (from CDK outputs)
S3_IMAGES_BUCKET=ojt-ecommerce-images-123456789012
```

**4. Get values from CDK outputs**

```powershell
# Get RDS endpoint
aws rds describe-db-instances `
  --db-instance-identifier ojt-database `
  --query 'DBInstances[0].Endpoint.Address' `
  --output text

# Get Secrets Manager ARN
aws secretsmanager list-secrets `
  --query "SecretList[?contains(Name, 'OJT')].ARN" `
  --output text

# Get S3 bucket name
aws s3 ls | Select-String "ojt-ecommerce-images"
```

#### Step 3: Review API Endpoints

**Authentication APIs (4 endpoints):**

| Method | Endpoint | Handler | Description |
|--------|----------|---------|-------------|
| POST | `/auth/login` | login.js | User login |
| POST | `/auth/signup` | signup.js | User registration |
| POST | `/auth/logout` | logout.js | User logout |
| GET | `/auth/me` | me.js | Get current user |

**Products APIs (12 endpoints):**

| Method | Endpoint | Handler | Description |
|--------|----------|---------|-------------|
| GET | `/products` | getProducts.js | List all products |
| GET | `/products/detail/{id}` | getProductDetail.js | Product detail |
| POST | `/products` | createProduct.js | Create product (Admin) |
| PUT | `/products/{id}` | updateProduct.js | Update product (Admin) |
| DELETE | `/products/{id}` | deleteProduct.js | Delete product (Admin) |
| GET | `/products/search` | searchProducts.js | Search products |
| GET | `/products/best-selling` | getBestSelling.js | Best selling products |
| GET | `/products/newest` | getNewest.js | Newest products |
| GET | `/products/category/{id}` | getProductsByCategory.js | Products by category |
| GET | `/products/brand/{id}` | getProductsByBrand.js | Products by brand |
| GET | `/products/price-range` | getProductsByPriceRange.js | Products by price |

**Cart APIs (6 endpoints):**

| Method | Endpoint | Handler | Description |
|--------|----------|---------|-------------|
| POST | `/cart` | addToCart.js | Add to cart |
| GET | `/cart/me` | getMyCart.js | Get user's cart |
| PUT | `/cart/{id}` | updateCartItem.js | Update cart item |
| DELETE | `/cart/{id}` | removeCartItem.js | Remove cart item |
| DELETE | `/cart` | clearCart.js | Clear cart |
| GET | `/cart/count` | getCartCount.js | Get cart count |

**Orders APIs (9 endpoints):**

| Method | Endpoint | Handler | Description |
|--------|----------|---------|-------------|
| GET | `/orders` | getAllOrders.js | All orders (Admin) |
| POST | `/orders` | createOrder.js | Create order |
| POST | `/orders/create-cod` | createOrderCOD.js | Create COD order |
| GET | `/orders/{id}/details` | getOrderDetails.js | Order details |
| GET | `/orders/user/{userId}` | getUserOrders.js | User's orders |
| PATCH | `/orders/{id}/status` | updateOrderStatus.js | Update status |
| DELETE | `/orders/{id}` | cancelOrder.js | Cancel order |

#### Step 4: Install Lambda Dependencies

```powershell
# Install main dependencies
cd OJT_lambda
npm install

# Install all module dependencies
npm run install:all
```

This installs dependencies for:
- `shared/` - Database, auth, response utilities
- `auth/` - bcryptjs, jsonwebtoken
- `products/` - Database queries
- `cart/`, `orders/`, etc.

#### Step 5: Review Shared Utilities

**Database Helper (`shared/database.js`):**

```javascript
const sql = require('mssql');

const config = {
  server: process.env.DB_HOST,
  database: process.env.DB_NAME,
  user: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
  options: {
    encrypt: true,
    trustServerCertificate: true
  }
};

async function query(sqlQuery, params = []) {
  const pool = await sql.connect(config);
  const result = await pool.request();
  // Add parameters
  params.forEach((param, index) => {
    result.input(`param${index}`, param);
  });
  return result.query(sqlQuery);
}

module.exports = { query, sql };
```

**Auth Helper (`shared/auth.js`):**

```javascript
const jwt = require('jsonwebtoken');

function generateToken(user) {
  return jwt.sign(
    { userId: user.UserID, email: user.Email, role: user.Role },
    process.env.JWT_SECRET,
    { expiresIn: process.env.JWT_EXPIRES_IN || '7d' }
  );
}

function verifyToken(token) {
  return jwt.verify(token, process.env.JWT_SECRET);
}

module.exports = { generateToken, verifyToken };
```

**Response Helper (`shared/response.js`):**

```javascript
function success(data, statusCode = 200) {
  return {
    statusCode,
    headers: {
      'Content-Type': 'application/json',
      'Access-Control-Allow-Origin': '*'
    },
    body: JSON.stringify(data)
  };
}

function error(message, statusCode = 500) {
  return {
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

#### Step 6: Verify API Gateway Configuration

**1. Get API Gateway URL from CDK outputs**

```powershell
# Get API Gateway URL
aws cloudformation describe-stacks `
  --stack-name OJT-ApiStack `
  --query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' `
  --output text
```

Expected output:
```
https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod
```

**2. Check API Gateway resources**

```powershell
# Get API ID
$API_ID = aws apigateway get-rest-apis `
  --query "items[?contains(name, 'OJT')].id" `
  --output text

# List resources
aws apigateway get-resources --rest-api-id $API_ID
```

#### Step 7: Verify Lambda Functions

**1. List Lambda functions**

```powershell
aws lambda list-functions `
  --query "Functions[?contains(FunctionName, 'OJT-Ecommerce')].FunctionName" `
  --output table
```

Expected output:
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

**2. Check Lambda environment variables**

```powershell
aws lambda get-function-configuration `
  --function-name OJT-Ecommerce-AuthModule `
  --query 'Environment.Variables'
```

#### Step 8: Test Lambda Functions Locally

**1. Test Auth Login**

```powershell
cd OJT_lambda

# Test login handler
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

**2. Test Products List**

```powershell
# Test get products handler
node -e "
const handler = require('./products/getProducts.js').handler;
const event = {
  queryStringParameters: { page: '1', limit: '10' }
};
handler(event).then(console.log);
"
```

#### Step 9: Build Lambda Packages

```powershell
# Build all Lambda packages
npm run build

# Or build specific module
npm run build:auth
npm run build:products
```

This creates ZIP files in `build/` directory:
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

#### Configuration Checklist

- [ ] Lambda project structure reviewed
- [ ] Environment variables configured in `.env`
- [ ] Database connection details obtained from CDK outputs
- [ ] JWT secret configured
- [ ] S3 bucket name configured
- [ ] Dependencies installed (`npm install` + `npm run install:all`)
- [ ] Shared utilities reviewed (database, auth, response)
- [ ] API Gateway URL obtained
- [ ] Lambda functions listed and verified
- [ ] Local tests passing
- [ ] Lambda packages built (`npm run build`)

#### API Summary

| Module | Functions | Endpoints |
|--------|-----------|-----------|
| Auth | 4 | login, signup, logout, me |
| Products | 12 | CRUD + search, filter, best-selling, newest |
| Product Details | 7 | CRUD + images upload |
| Cart | 6 | add, get, update, remove, clear, count |
| Orders | 9 | CRUD + COD, status, date-range |
| Categories | 6 | CRUD + search |
| Brands | 5 | CRUD |
| Banners | 7 | CRUD + toggle |
| Ratings | 3 | get, stats, create |
| Users | 3 | getAll, getById, updateProfile |
| Images | 1 | upload |
| **Total** | **63** | |

#### Next Steps

Once configuration is complete, proceed to [Deploy Backend] to deploy your Lambda code to AWS.

