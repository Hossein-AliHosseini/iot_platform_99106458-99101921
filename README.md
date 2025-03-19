# OS-Lab-1-2-99106458-99101921
Os Lab

# IoT Platform Development Project

This repository is dedicated to the development of an IoT platform with multiple modules, including IoT sensors and a web dashboard. The following document provides an overview of the project setup, branching strategy, commit standards, milestones, and development workflow for Phase 1.

---

## Project Overview

The IoT platform aims to support modular development with simultaneous contributions to different features. During Phase 1, the repository was set up, essential branches were created, commit standards were defined, and milestones were introduced to streamline development.

---

## Repository Setup

### Main Branch
- **Branch Name**: `main` (or `master`)
- This is the **protected branch** containing production-ready and stable code.
- Protection rules:
  - Changes to this branch require a **pull request**.
  - CI checks must pass before merging.
  - At least **1 reviewer approval** is required for pull requests.
  - Direct pushes to this branch are restricted.

### Feature Branches
Each feature is developed in a separate branch to ensure modular development.
- **feature/iot-sensors**: For IoT sensor integration and related functionality.
- **feature/web-dashboard**: For designing and implementing the web dashboard interface.

Feature branches are used to maintain code separation and proper integration workflows.


# Automating Feature Branch Merging

## Objective
Set up a pipeline such that after successful testing, feature branches are automatically merged into the target branch (e.g., `main` or `develop`).

---

## Steps to Implement

### Step 1: Script for Automated Merging
Create a script that automates the process of merging feature branches into the target branch. This script checks the test results, ensures successful execution, and proceeds with merging the branch.

Example Bash Script (`auto-merge.sh`):
```bash
#!/bin/bash

# Variables
FEATURE_BRANCH=$1         
TARGET_BRANCH="develop"   

cd /path/to/repository

git fetch origin

git checkout $TARGET_BRANCH

git pull origin $TARGET_BRANCH

git merge origin/$FEATURE_BRANCH

git push origin $TARGET_BRANCH
```

### Step 2: Configure CI/CD Pipeline
Integrate the automation script into the CI/CD pipeline. This pipeline ensures that merging only occurs if all tests pass successfully.

Example GitHub Actions (.github/workflows/ci-cd.yml):

```yaml
name: CI/CD Pipeline with Auto-Merge

on:
  pull_request:
    branches:
      - main
      - develop

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test
      
  merge:
    needs: test
    if: success() # Proceed only if tests succeed
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Auto-Merge Feature Branch
        run: ./auto-merge.sh feature-branch-name
```

### Step 3: Documentation
Document the automation process and how it is configured. Ensure the documentation covers:

What the automation does:
Merges feature branches into the target branch after successful test execution.

How the automation is triggered:
Pipeline triggers on pull requests or specified branches.

Script details:
Include the bash script used for merging and instructions for its usage.

Example README Section:

```markdown
# Automating Feature Branch Merging

## Objective
Automatically merge feature branches into the target branch (e.g., `develop`) once tests pass successfully.

## How It Works
1. CI/CD pipeline runs tests for the feature branch.
2. If tests pass successfully, the `auto-merge.sh` script is invoked to merge the feature branch into the target branch.
3. The merged branch is pushed back to the remote repository.

## Scripts
### `auto-merge.sh`
A bash script to automate the merge process:
```bash
#!/bin/bash
FEATURE_BRANCH=$1
TARGET_BRANCH="develop"
cd /path/to/repository
git fetch origin
git checkout $TARGET_BRANCH
git pull origin $TARGET_BRANCH
git merge origin/$FEATURE_BRANCH
git push origin $TARGET_BRANCH
```

## CI/CD Pipeline Configuration
The automation process is configured in the pipeline file (.github/workflows/ci-cd.yml). Key steps:

- Run tests to validate

---

## Branching Strategy

### Workflow

1. Pull the latest changes from the `main` branch:
   ```bash
   git pull origin main
   ```

2. Create a **feature branch** for new development:
   ```bash
   git checkout -b feature/<branch-name>
   ```

3. Push the feature branch to the remote repository:
   ```bash
   git push --set-upstream origin feature/<branch-name>
   ```

### Branch Usage

- The `main` branch is restricted to production-ready code.
- Feature branches are used to develop, test, and integrate individual modules.

---

## Commit Standards

Clear and descriptive commit messages are essential for tracking changes. Below is the standard format for commit messages:

### Commit Message Format

```
[Type]: [Brief Description]
```

#### **Types**
- `feat`: Adding a new feature.
- `fix`: Fixing a bug.
- `docs`: Updating documentation.
- `refactor`: Code refactoring.
- `test`: Adding or modifying tests.
- `build`: Modifying build configurations.

#### **Examples**
- `feat: Add motion sensor functionality`
- `fix: Resolve API processing error`
- `docs: Add details about branching strategy to README`

---

## Milestones

### Milestone Setup

Milestones are used to track major phases of development.

#### Initial Milestone
- **Title**: **Initial Release**
- **Content**:
  - Repository setup.
  - Creation of protected `main` branch.
  - Feature branch creation.
  - Commitment to descriptive commit standards.
  - Documentation updates.

Future milestones (e.g., module integration or CI/CD setup) will be created as development progresses.

---

## Development Workflow

### Development Steps

Follow the steps below to ensure proper collaboration and integration:

1. Pull the latest changes from `main`:
   ```bash
   git pull origin main
   ```

2. Switch to an appropriate feature branch:
   ```bash
   git checkout -b feature/<branch-name>
   ```

3. Commit changes following commit standards:
   ```bash
   git commit -m "[Type]: [Brief Description]"
   ```

4. Push changes to the remote feature branch:
   ```bash
   git push origin feature/<branch-name>
   ```

5. Open a pull request to merge changes into `main`:
   - Ensure pull requests are reviewed and approved.
   - Wait for CI checks to pass before merging.

---

## Current Progress

### Phase 1 Completed

1. **Repository Setup**
   - Created a GitHub repository with a protected `main` branch.

2. **Branching Strategy**
   - Defined and implemented the branching strategy.
   - Created feature branches (`feature/iot-sensors` and `feature/web-dashboard`).

3. **Commit Standards**
   - Established commit standards with examples for better traceability.

4. **Milestones**
   - Created the **Initial Release** milestone to track repository setup and Phase 1 tasks.

---

## Next Steps
# پایش و گزارش خطا (Monitoring and Error Reporting)

## وظیفه
پیکربندی اعلان‌ها یا گزارش‌دهی خطا در سیستم CI/CD به منظور اطلاع‌رسانی به تیم در موارد ایجاد خطا یا شکست‌های بیلد.

## دستورالعمل‌ها

### 1. تنظیم اعلان‌های ایمیلی
- **انتخاب ابزار ایمیل:** از ابزارهایی مانند SendGrid یا سرویس‌های ایمیلی داخلی استفاده کنید.
- **پیکربندی در CI/CD:**  
  - اگر از GitHub Actions بهره می‌برید، می‌توانید از یک GitHub Action برای ارسال ایمیل استفاده کنید (مثل action-send-mail).
  - در فایل workflow (.github/workflows/ci-cd.yml) یک مرحله اضافه کنید که در صورت شکست بیلد، یک ایمیل به تیم ارسال کند.

**نمونه‌ای از پیکربندی ایمیل در GitHub Actions:**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2
      
      - name: Build and Test
        run: |
          npm install
          npm test

  notify_failure:
    needs: build
    if: failure() # فقط در صورت شکست بیلد اجرا می‌شود
    runs-on: ubuntu-latest
    steps:
      - name: Send Failure Email
        uses: dawidd6/action-send-mail@v3
        with:
          server_address: smtp.example.com
          server_port: 587
          username: ${{ secrets.EMAIL_USERNAME }}
          password: ${{ secrets.EMAIL_PASSWORD }}
          subject: "CI/CD Build Failed: Repository Name"
          body: "سلام تیم،\n\nبیلد آخر به دلیل خطا شکست خورد. لطفاً برای بررسی وارد شوید."
          to: team@example.com
          from: ci@example.com
```

### 2. تنظیم اعلان‌های Slack
- **ایجاد Webhook در Slack:** یک Slack Incoming Webhook برای کانال مورد نظر تنظیم کنید.
- **پیکربندی در CI/CD:**  
  - از یک Action یا اسکریپت استفاده کنید تا در صورت بروز خطا در CI/CD، پیام به کانال Slack ارسال شود.
  - می‌توانید از Action های موجود مانند rtCamp/action-slack-notify استفاده کنید.

**نمونه‌ای از پیکربندی اعلان‌های Slack:**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2
      
      - name: Build and Test
        run: |
          npm install
          npm test

  notify_failure_slack:
    needs: build
    if: failure() # فقط در صورت شکست بیلد اجرا می‌شود
    runs-on: ubuntu-latest
    steps:
      - name: Send Slack Notification
        uses: rtCamp/action-slack-notify@v2
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
          SLACK_MESSAGE: "بیلد در مخزن شما شکست خورد. لطفاً بررسی کنید."
```


## پایش و گزارش خطا

این پروژه از ابزارهای CI/CD برای اجرای بیلد و تست استفاده می‌کند. در صورتی که بیلد شکست بخورد، سیستم اعلان‌های زیر فعالیت می‌کند:
- **ایمیل:** ارسال ایمیل به تیم با توضیحات خطا
- **Slack:** ارسال پیام به کانال مشخص در Slack برای اطلاع‌رسانی سریع

### روند عیب‌یابی

در صورت بروز خطا، ابتدا مراحل زیر را دنبال کنید:
1. بررسی گزارش‌های بیلد در سیستم CI/CD (لاگ‌های اجرای کارها)
2. مشاهده پیام‌های دریافتی در ایمیل یا Slack برای شناسایی خطا
3. مراجعه به مستندات عیب‌یابی در پوشه /docs برای جزئیات بیشتر
4. اقدام بر اساس راهنمایی‌های مستند شده جهت رفع مشکل


---
