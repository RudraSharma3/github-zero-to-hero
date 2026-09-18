# Lab 2: Repository Hygiene, Normalization & Governance (Break/Fix)

---

## 1. Objective
Transform a messy, unstructured codebase into a production-grade GitHub repository by configuring `.gitignore`, eliminating cross-platform line ending corruption via `.gitattributes`, authoring YAML Issue Forms, and diagnosing/recovering from **accidentally committed build artifacts**, **CRLF line-ending phantom diffs**, and **broken `CODEOWNERS` rules**.

---

## 2. Prerequisites
- Terminal environment with Git `2.34+` installed.
- Completed [Level 0](../00-foundations/) and [Level 1](../01-git-fundamentals/).

---

## 3. Setup
Create a dedicated scratch workspace:

```bash
mkdir -p ~/git-mastery-labs/lab-02
cd ~/git-mastery-labs/lab-02
```

---

## 4. Instructions

### Part A: Initialize Repository & Scaffold Structure
```bash
mkdir enterprise-app && cd enterprise-app
git init

# Create directory tree
mkdir -p .github/ISSUE_TEMPLATE src/auth src/billing docs scripts
```

### Part B: Establish `.gitignore` & `.gitattributes`
```bash
# 1. Author .gitignore
cat << 'EOF' > .gitignore
# Build artifacts
/dist/
/build/
*.pyc
node_modules/

# Logs & temporary files
*.log
!audit.log

# Environment variables
.env
.env.*
!.env.example
EOF

# 2. Author .gitattributes
cat << 'EOF' > .gitattributes
# Default text normalization
* text=auto

# Enforce LF for scripts and configs
*.sh text eol=lf
*.py text eol=lf
Dockerfile text eol=lf

# Enforce CRLF for Windows batch files
*.bat text eol=crlf

# Binary file protections
*.png binary
*.jpg binary
*.pdf binary
EOF
```

### Part C: Author Governance Files & Issue Forms
```bash
# 1. Author .github/CODEOWNERS
cat << 'EOF' > .github/CODEOWNERS
# Global default maintainer
* @core-leads

# Documentation
/docs/ @tech-writers

# Billing subsystem requires security and finance approval
/src/billing/ @finance-team @security-lead
EOF

# 2. Author .github/ISSUE_TEMPLATE/config.yml
cat << 'EOF' > .github/ISSUE_TEMPLATE/config.yml
blank_issues_enabled: false
contact_links:
  - name: Community Discussions
    url: https://github.com/octo-org/project/discussions
    about: Ask setup and usage questions.
EOF

# 3. Author .github/ISSUE_TEMPLATE/bug_report.yml
cat << 'EOF' > .github/ISSUE_TEMPLATE/bug_report.yml
name: 🐛 Bug Report
description: File a validated bug report.
title: "[BUG]: "
labels: ["bug", "triage-needed"]
body:
  - type: input
    id: component
    attributes:
      label: Subsystem / Component
      placeholder: e.g. Billing, Auth
    validations:
      required: true
  - type: textarea
    id: reproduction
    attributes:
      label: Reproduction Steps
    validations:
      required: true
EOF

# Commit clean governance baseline
git add .
git commit -m "chore: establish repository architecture, hygiene, and governance"
```

---

## 5. Expected Result
When inspecting the repository:

```bash
git status --short
ls -la .github/
ls -la .github/ISSUE_TEMPLATE/
```

**Expected Terminal Output**:
```text
.github/
├── CODEOWNERS
└── ISSUE_TEMPLATE/
    ├── bug_report.yml
    └── config.yml
.gitattributes
.gitignore
```

---

## 6. Verification
Confirm that line ending rules and ignore definitions are recognized:

```bash
# Check ignore matching
touch test.log && git check-ignore -v test.log
# Output: .gitignore:8:*.log test.log

# Check git attributes
git check-attr -a .gitattributes
git check-attr text eol scripts/deploy.sh
```

---

## 7. Troubleshooting & Deliberate Failure Scenarios (BREAK & FIX)

### 🔴 Failure Scenario 1: Accidentally Committed Build Cache & Tracked `.env`
**The Break**: Simulate an engineer who forced build artifacts and `.env` into history:
```bash
# Simulate accidental commit of build artifact and private credentials
mkdir -p dist
echo "bundle v1" > dist/app.min.js
echo "DB_PASSWORD=secret_password_123" > .env

# Force add and commit
git add -f dist/app.min.js .env
git commit -m "feat: release build with secrets (accident)"
```
*Problem*: Both `dist/app.min.js` and `.env` are now tracked. Even though they are in `.gitignore`, Git tracks every future modification!

**The Fix (Untracking without local deletion)**:
1. Verify why Git still tracks them:
   ```bash
   git ls-files dist/ .env
   # Shows: dist/app.min.js, .env
   ```
2. Remove files from the Index cache while preserving local files on disk:
   ```bash
   git rm --cached -r dist/ .env
   ```
3. Commit the untracking change:
   ```bash
   git commit -m "chore: untrack build artifacts and env credentials"
   ```
4. Verify: `.env` and `dist/app.min.js` still exist on your filesystem, but `git status` shows zero tracked changes!

---

### 🔴 Failure Scenario 2: Cross-Platform CRLF Line Ending Corruption
**The Break**: Simulate corrupted Windows `\r\n` endings committed into Linux shell scripts:
```bash
# Create script with explicit Windows CRLF line endings
printf "#!/bin/bash\r\necho 'Starting server...'\r\nexit 0\r\n" > scripts/deploy.sh
git add -f scripts/deploy.sh
git commit -m "feat: add deploy script"
```
*Problem*: Running this script inside Docker or Linux fails with `/bin/bash^M: bad interpreter`.

**The Fix (Repository Normalization)**:
1. Ensure `.gitattributes` contains `*.sh text eol=lf`.
2. Run Git's re-normalization engine across the entire codebase:
   ```bash
   git add --renormalize .
   ```
3. Inspect the staged diff:
   ```bash
   git diff --staged
   ```
   *Notice*: Git automatically converts all `\r\n` byte sequences to clean Unix `\n` in the index!
4. Commit the normalized state:
   ```bash
   git commit -m "chore: renormalize line endings to LF"
   ```

---

### 🔴 Failure Scenario 3: Broken `CODEOWNERS` Inverted Precedence
**The Break**: A developer edits `CODEOWNERS` and appends `* @junior-dev` to the bottom of the file.
```text
# Broken CODEOWNERS:
/src/billing/ @finance-team @security-lead
* @junior-dev   <-- This overrides all specific rules above it!
```

**The Fix**:
Remember: **Last matching rule wins!** Always place general wildcards (`*`) at the **top** of the file, followed by specific path overrides:
```text
# Fixed CODEOWNERS:
* @junior-dev
/src/billing/ @finance-team @security-lead
```

---

## 8. Cleanup
```bash
cd ~/git-mastery-labs/lab-02
rm -rf enterprise-app
```

---

## 9. Extension Challenge
Add a custom Linguist override in `.gitattributes` so that all files in `docs/` and all files ending in `.generated.ts` are excluded from repository language calculations and automatically collapsed in PR reviews.
