---
title: "Push to GitLab"
date: 2025-12-01
weight: 8
chapter: false
pre: " <b> 5.08. </b> "
---

#### Overview

Sau khi test thành công, bạn sẽ push code lên GitLab để version control và chuẩn bị cho CI/CD. Dự án OJT E-commerce có 3 phần chính cần quản lý: Infrastructure, Lambda, và Frontend.

#### Project Structure

```
OJT/
├── OJT_infrastructure/      # CDK Infrastructure (TypeScript)
├── OJT_lambda/             # Lambda Functions (JavaScript)
├── OJT_frontendDev/        # Frontend (React + Vite)
├── database/               # Database Scripts
└── README.md               # Project documentation
```

---

### Step 1: Initialize Git Repository

**1. Navigate to Project Root**

```powershell
cd D:\AWS\Src\OJT
```

**2. Check Git Status**

```powershell
# Check if Git is already initialized
git status

# If not initialized:
git init
```

**3. Configure Git**

```powershell
# Set your name and email
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Verify configuration
git config --list
```

---

### Step 2: Create .gitignore

**1. Create .gitignore File**

```powershell
# Create .gitignore in project root
@"
# Dependencies
node_modules/
package-lock.json

# Build outputs
dist/
build/
*.js.map
cdk.out/

# Environment variables
.env
.env.local
.env.*.local

# AWS
*.pem
*.key

# Deployment packages
*.zip

# Logs
logs/
*.log
npm-debug.log*

# IDE
.vscode/settings.json
.idea/
*.swp
*.swo

# OS
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

**2. Verify .gitignore**

```powershell
Get-Content .gitignore
```

---

### Step 3: Stage and Commit Files

**1. Add Files to Staging**

```powershell
# Add all files
git add .

# Check what will be committed
git status
```

**2. Review Files to Commit**

```powershell
# List files to be committed
git diff --cached --name-only

# Expected files:
# OJT_infrastructure/
# OJT_lambda/
# OJT_frontendDev/
# database/
# README.md
# .gitignore
```

**3. Create Initial Commit**

```powershell
# Commit with message
git commit -m "Initial commit: OJT E-commerce infrastructure and backend"

# Verify commit
git log --oneline

### Step 4: Create GitLab Repository

**1. Login to GitLab**

Go to https://gitlab.com/ and login with your account.

**2. Create New Project**

1. Click **"New project"**
2. Choose **"Create blank project"**
3. Fill in details:
   - Project name: `ojt-ecommerce`
   - Project slug: `ojt-ecommerce`
   - Visibility: **Private** (recommended)
   - Initialize with README: **No** (we already have code)
4. Click **"Create project"**



**3. Copy Repository URL**

```
https://gitlab.com/your-username/ojt-ecommerce.git
```

---

### Step 5: Add Remote and Push

**1. Add GitLab Remote**

```powershell
# Add remote origin
git remote add origin https://gitlab.com/your-username/ojt-ecommerce.git

# Verify remote
git remote -v
```

**2. Push to GitLab**

```powershell
# Push to main branch
git push -u origin main

# If your default branch is 'master':
git branch -M main
git push -u origin main
```

**3. Enter Credentials**

When prompted:
- Username: Your GitLab username
- Password: Your GitLab Personal Access Token (not password)


*Screenshot: Terminal showing successful push*

---

### Step 6: Create Personal Access Token

If you don't have a Personal Access Token:

**1. Go to GitLab Settings**

1. Click your avatar → **"Preferences"**
2. Go to **"Access Tokens"** in left sidebar

**2. Create New Token**

1. Token name: `OJT-Ecommerce-Token`
2. Expiration date: Set appropriate date
3. Select scopes:
   - [x] `read_repository`
   - [x] `write_repository`
4. Click **"Create personal access token"**

**3. Save Token**

Copy and save the token securely. You won't be able to see it again.


### Step 7: Verify Push on GitLab

**1. Check Repository on GitLab**

Go to your repository: `https://gitlab.com/your-username/ojt-ecommerce`

**2. Verify Files**

Check that all files are uploaded:
- `OJT_infrastructure/` - CDK Infrastructure
- `OJT_lambda/` - Lambda Functions
- `OJT_frontendDev/` - Frontend
- `database/` - Database Scripts
- `README.md` - Documentation
- `.gitignore` - Git ignore rules

---

### Step 8: Create Development Branch

**1. Create dev Branch**

```powershell
# Create and switch to dev branch
git checkout -b dev

# Push dev branch to GitLab
git push -u origin dev
```

**2. Set Default Branch (Optional)**

On GitLab:
1. Go to **Settings** → **Repository**
2. Under **Branch defaults**, set `dev` as default branch

---

### Step 9: Setup Branch Protection (Optional)

**1. Protect Main Branch**

On GitLab:
1. Go to **Settings** → **Repository** → **Protected branches**
2. Add `main` branch:
   - Allowed to merge: Maintainers
   - Allowed to push: No one
   - Require approval: Yes

**2. Protect Dev Branch**

Add `dev` branch with similar settings but allow developers to push.

---

### Git Workflow Summary

```
1. Create feature branch from dev
   git checkout dev
   git pull origin dev
   git checkout -b feature/new-feature

2. Make changes and commit
   git add .
   git commit -m "feat: Add new feature"

3. Push to GitLab
   git push -u origin feature/new-feature

4. Create Merge Request on GitLab
   feature/new-feature → dev

5. Review and merge

6. Deploy to dev environment
   cd OJT_lambda
   npm run build
   npm run deploy

7. Test in dev environment

8. Merge dev → main for production
```

---

### Commit Message Convention

Follow conventional commits:

```
feat: Add user authentication
fix: Fix login validation bug
docs: Update API documentation
style: Format code with prettier
refactor: Refactor database queries
test: Add unit tests for auth
chore: Update dependencies
perf: Optimize product search query
```

**Examples:**

```powershell
git commit -m "feat: Add cart functionality"
git commit -m "fix: Fix order total calculation"
git commit -m "docs: Update README with deployment steps"
```

---

### Branch Naming Convention

```
feature/add-cart-api
feature/user-authentication
bugfix/fix-login-error
bugfix/cart-quantity-issue
hotfix/critical-security-patch
release/v1.0.0
```

---

### Troubleshooting

#### Issue: Authentication Failed

```powershell
# Use Personal Access Token instead of password
# When prompted for password, enter your token

# Or configure credential helper
git config --global credential.helper store
```

#### Issue: Large Files Rejected

```powershell
# Check file sizes
git ls-files -z | ForEach-Object { 
  $size = (Get-Item $_).Length / 1MB
  if ($size -gt 1) { Write-Host "$_ : $size MB" }
}

# Add large files to .gitignore
echo "*.zip" >> .gitignore
git rm --cached *.zip
git commit -m "Remove large files"
```

#### Issue: Push Rejected (Non-Fast-Forward)

```powershell
# Pull latest changes first
git pull origin main --rebase

# Then push
git push origin main
```

#### Issue: Merge Conflicts

```powershell
# Update your branch
git checkout feature/your-feature
git fetch origin
git merge origin/dev

# Resolve conflicts in files
# Then commit
git add .
git commit -m "Resolve merge conflicts"
git push
```

---

### Repository Checklist

- [ ] Git initialized in project root
- [ ] .gitignore created with proper exclusions
- [ ] Initial commit created
- [ ] GitLab repository created
- [ ] Remote origin added
- [ ] Code pushed to GitLab
- [ ] Personal Access Token created (if needed)
- [ ] Files verified on GitLab
- [ ] Dev branch created
- [ ] Branch protection configured (optional)

---

### Next Steps
