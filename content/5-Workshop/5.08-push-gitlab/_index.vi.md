---
title: "Push lên GitLab"
date: 2025-12-01
weight: 9
chapter: false
pre: " <b> 5.09. </b> "
---

#### Tổng quan

Sau khi kiểm tra thành công, bạn sẽ đẩy mã lên GitLab để kiểm soát phiên bản và chuẩn bị cho CI/CD. Dự án Thương mại điện tử OJT có 3 phần chính cần quản lý: Cơ sở hạ tầng, Lambda và Frontend.

#### Cấu trúc dự án

```
OJT/
├── OJT_infrastructure/ # Cơ sở hạ tầng CDK (TypeScript)
├── OJT_lambda/ # Hàm Lambda (JavaScript)
├── OJT_frontendDev/ # Frontend (React + Vite)
├── cơ sở dữ liệu/ # Tập lệnh cơ sở dữ liệu
└── README.md # Tài liệu dự án
```

---

### Bước 1: Khởi tạo kho lưu trữ Git

**1. Điều hướng đến Project Root**

```powershell
cd D:\AWS\Src\OJT
```

**2. Kiểm tra Trạng thái Git**

```powershell
# Kiểm tra xem Git đã được khởi tạo chưa
git status

# Nếu chưa được khởi tạo:
git init
```

**3. Cấu hình Git**

```powershell
# Đặt tên và email của bạn
git config user.name "Tên của bạn"
git config user.email "your.email@example.com"

# Xác minh cấu hình
git config --list
```

---

### Bước 2: Tạo .gitignore

**1. Tạo tệp .gitignore**

```powershell
# Tạo .gitignore trong thư mục gốc của dự án
@"
# Các tệp phụ thuộc
node_modules/
package-lock.json

# Kết quả bản dựng
dist/
build/
*.js.map
cdk.out/

# Biến môi trường
.env
.env.local
.env.*.local

# AWS
*.pem
*.key

# Gói triển khai
*.zip

# Nhật ký
logs/
*.log
npm-debug.log*

# IDE
.vscode/settings.json
.idea/
*.swp
*.swo

# Hệ điều hành
.DS_Store
Thumbs.db

# CDK
cdk.context.json

# TypeScript
*.tsbuildinfo
*.d.ts
*.js
!vite.config.js
!eslint.config.js

# Lambda build
OJT_lambda/build/
"@ | Out-File -FilePath .gitignore -Encoding UTF8
```

**2. Xác minh .gitignore**

```powershell
Get-Content .gitignore
```

---

### Bước 3: Phân đoạn và Cam kết Tệp

**1. Thêm Tệp vào Phân đoạn**

```powershell
# Thêm tất cả tệp
git add .

# Kiểm tra những tệp sẽ được cam kết
git status
```

**2. Xem lại các tệp cần cam kết**

```powershell
# Liệt kê các tệp cần cam kết
git diff --cached --name-only

# Các tệp dự kiến:
# OJT_infrastructure/
# OJT_lambda/
# OJT_frontendDev/
# database/
# README.md
# .gitignore
```

**3. Tạo cam kết ban đầu**

```powershell
# Cam kết với thông báo
git commit -m "Cam kết ban đầu: Cơ sở hạ tầng và backend thương mại điện tử OJT"

# Xác minh cam kết
git log --oneline

### Bước 4: Tạo Kho lưu trữ GitLab

**1. Đăng nhập vào GitLab**

Truy cập https://gitlab.com/ và đăng nhập bằng tài khoản của bạn.

**2. Tạo Dự án Mới**

1. Nhấp vào **"Dự án Mới"**
2. Chọn **"Tạo Dự án Trống"**
3. Điền thông tin chi tiết:
- Tên dự án: `ojt-ecommerce`
- Slug dự án: `ojt-ecommerce`
- Chế độ hiển thị: **Riêng tư** (khuyến nghị)
- Khởi tạo với README: **Không** (chúng tôi đã có mã)
4. Nhấp vào **"Tạo Dự án"**

**3. Sao chép URL Kho lưu trữ**

```
https://gitlab.com/your-username/ojt-ecommerce.git
```

---

### Bước 5: Thêm Remote và Đẩy

**1. Thêm GitLab Remote**

```powershell
# Thêm remote origin
git remote add origin https://gitlab.com/your-username/ojt-ecommerce.git

# Xác minh remote
git remote -v
```

**2. Đẩy lên GitLab**

```powershell
# Đẩy lên nhánh chính
git push -u origin main

# Nếu nhánh mặc định của bạn là 'master':

git branch -M main
git push -u origin main
```

**3. Nhập thông tin đăng nhập**

Khi được nhắc:
- Tên người dùng: Tên người dùng GitLab của bạn
- Mật khẩu: Mã truy cập cá nhân GitLab của bạn (không phải mật khẩu)

*Ảnh chụp màn hình: Terminal hiển thị thông báo đẩy thành công*

---

### Bước 6: Tạo Mã truy cập cá nhân

Nếu bạn không có Mã truy cập cá nhân:

**1. Vào Cài đặt GitLab**

1. Nhấp vào ảnh đại diện của bạn → **"Tùy chọn"**
2. Vào **"Mã truy cập"** ở thanh bên trái

**2. Tạo Token Mới**

1. Tên Token: `OJT-Ecommerce-Token`
2. Ngày hết hạn: Đặt ngày phù hợp
3. Chọn phạm vi:
- [x] `read_repository`
- [x] `write_repository`
4. Nhấp vào **"Tạo Token Truy cập Cá nhân"**

**3. Lưu Token**

Sao chép và lưu Token một cách an toàn. Bạn sẽ không thể nhìn thấy Token này nữa.

### Bước 7: Xác minh Push trên GitLab

**1. Kiểm tra Kho lưu trữ trên GitLab**

Truy cập kho lưu trữ của bạn: `https://gitlab.com/your-username/ojt-ecommerce`

**2. Xác minh Tệp**

Kiểm tra xem tất cả các tệp đã được tải lên chưa:
- `OJT_infrastructure/` - Cơ sở hạ tầng CDK
- `OJT_lambda/` - Hàm Lambda
- `OJT_frontendDev/` - Giao diện người dùng
- `database/` - Tập lệnh Cơ sở dữ liệu
- `README.md` - Tài liệu
- `.gitignore` - Quy tắc bỏ qua Git

---

### Bước 8: Tạo Nhánh Phát triển

**1. Tạo Nhánh dev**

```powershell
# Tạo và chuyển sang nhánh dev
git checkout -b dev

# Đẩy nhánh dev lên GitLab
git push -u origin dev
```

**2. Đặt Nhánh Mặc định (Tùy chọn)**

Trên GitLab:
1. Vào **Cài đặt** → **Kho lưu trữ**
2. Trong mục **Mặc định của Nhánh**, đặt `dev` làm nhánh mặc định

---

### Bước 9: Thiết lập Bảo vệ Nhánh (Tùy chọn)

**1. Bảo vệ Nhánh Chính**

Trên GitLab:
1. Vào **Cài đặt** → **Kho lưu trữ** → **Nhánh được bảo vệ**
2. Thêm nhánh `main`:
- Được phép hợp nhất: Người bảo trì
- Được phép đẩy: Không ai
- Yêu cầu phê duyệt: Có

**2. Bảo vệ Nhánh Dev**

Thêm nhánh `dev` với các thiết lập tương tự nhưng cho phép nhà phát triển đẩy.

---

### Tóm tắt Quy trình làm việc Git

```
1. Tạo nhánh tính năng từ dev
git checkout dev
git pull origin dev
git checkout -b feature/new-feature

2. Thực hiện thay đổi và commit
git add .
git commit -m "feat: Thêm tính năng mới"

3. Đẩy lên GitLab
git push -u origin feature/new-feature

4. Tạo Yêu cầu Hợp nhất trên GitLab
feature/new-feature → dev

5. Xem lại và hợp nhất

6. Triển khai lên môi trường dev
cd OJT_lambda
npm run build
npm run deploy

7. Kiểm tra trong môi trường dev

8. Hợp nhất dev → main cho môi trường production
```

---

### Quy ước Thông báo Cam kết

Tuân thủ các cam kết thông thường:

```
feat: Thêm xác thực người dùng
fix: Sửa lỗi xác thực đăng nhập
docs: Cập nhật tài liệu API
style: Định dạng mã đẹp hơn
refactor: Cấu trúc lại truy vấn cơ sở dữ liệu
test: Thêm các bài kiểm tra đơn vị cho xác thực
chore: Cập nhật các phụ thuộc
perf: Tối ưu hóa truy vấn tìm kiếm sản phẩm
```

**Ví dụ:**

```powershell
git commit -m "feat: Thêm chức năng giỏ hàng"
git commit -m "fix: Sửa lỗi tính toán tổng đơn hàng"
git commit -m "docs: Cập nhật README với các bước triển khai"
```

---

### Quy ước đặt tên nhánh

```
feature/add-cart-api
feature/user-authentication
bugfix/fix-login-error
bugfix/cart-quantity-issue
hotfix/critical-security-patch
release/v1.0.0
```

---

### Khắc phục sự cố

#### Sự cố: Xác thực không thành công

```powershell
# Sử dụng Mã truy cập cá nhân thay vì mật khẩu
# Khi được nhắc nhập mật khẩu, hãy nhập mã thông báo của bạn

# Hoặc cấu hình trình trợ giúp xác thực
git config --global credential.helper store
```

#### Sự cố: Tệp lớn bị từ chối

```powershell
# Kiểm tra kích thước tệp
git ls-files -z | ForEach-Object {
$size = (Get-Item $_).Length / 1MB
if ($size -gt 1) { Write-Host "$_ : $size MB" }
}

# Thêm tệp lớn vào .gitignore
echo "*.zip" >> .gitignore
git rm --cached *.zip
git commit -m "Xóa tệp lớn"
```

#### Sự cố: Đẩy bị từ chối (Không chuyển tiếp nhanh)

```powershell
# Kéo các thay đổi mới nhất trước
git pull origin main --rebase

# Sau đó đẩy
git push origin main
```

#### Sự cố: Xung đột hợp nhất

```powershell
# Cập nhật nhánh của bạn
git checkout feature/your-feature
git fetch origin
git merge origin/dev

# Giải quyết xung đột trong tệp
# Sau đó commit
git add .
git commit -m "Giải quyết xung đột hợp nhất"
git push
```

---

### Danh sách kiểm tra kho lưu trữ

- [ ] Git đã được khởi tạo trong thư mục gốc của dự án
- [ ] .gitignore đã được tạo với các loại trừ phù hợp
- [ ] Đã tạo cam kết ban đầu
- [ ] Đã tạo kho lưu trữ GitLab
- [ ] Đã thêm nguồn gốc từ xa
- [ ] Đã đẩy mã lên GitLab
- [ ] Đã tạo Mã truy cập cá nhân (nếu cần)
- [ ] Đã xác minh tệp trên GitLab
- [ ] Đã tạo nhánh phát triển
- [ ] Đã cấu hình bảo vệ nhánh (tùy chọn)

---
### Các bước tiếp theo