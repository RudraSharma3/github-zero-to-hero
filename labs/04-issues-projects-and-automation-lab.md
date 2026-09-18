# Lab 4: Issues, Agile Projects & Automation Engineering (Break/Fix)

---

## 1. Objective
Master agile collaboration and project management on GitHub by building multi-level Epic Issue tasklists, creating sprint Milestones, establishing namespaced label systems, and diagnosing/recovering from **broken tasklist links**, **misfiring auto-closing keywords**, and **project workflow routing failures**.

---

## 2. Prerequisites
- GitHub CLI (`gh`) installed and authenticated (`gh auth status`).
- Completed [Level 0](../00-foundations/) through [Level 3](../03-branching-workflows/).

---

## 3. Setup
Create a dedicated scratch workspace and ensure GitHub CLI is authenticated:

```bash
mkdir -p ~/git-mastery-labs/lab-04
cd ~/git-mastery-labs/lab-04
```

---

## 4. Instructions

### Part A: Create Namespaced Labels System
Execute the following commands to provision a standardized label schema:

```bash
# Provision type labels
gh label create "type: bug" --color "d73a4a" --description "Defect or unexpected error" --force
gh label create "type: feature" --color "a2eeef" --description "New functionality" --force

# Provision priority labels
gh label create "priority: p0-blocker" --color "b60205" --description "Production outage or blocker" --force
gh label create "priority: p1-high" --color "d93f0b" --description "High priority sprint work" --force

# Provision status labels
gh label create "status: triage-needed" --color "ededed" --description "Awaiting triage" --force
gh label create "status: in-progress" --color "fbca04" --description "Active development" --force
```

---

### Part B: Create Sprint Milestone
```bash
# Create Sprint Milestone
gh milestone create \
  --title "Sprint 1: User Onboarding Engine" \
  --description "Complete user registration, JWT authentication, and profile onboarding." \
  --due-date "2026-11-30"
```

---

### Part C: Author Sub-Issues & Parent Epic Issue
```bash
# 1. Create Sub-Issue 1: Database Schema
SUB1_URL=$(gh issue create \
  --title "feat(db): create user and session tables" \
  --body "Define PostgreSQL schema migrations for users and sessions." \
  --label "type: feature,priority: p1-high" \
  --milestone "Sprint 1: User Onboarding Engine")

# 2. Create Sub-Issue 2: Auth Service
SUB2_URL=$(gh issue create \
  --title "feat(auth): implement JWT token generation" \
  --body "Implement HMAC-SHA256 JWT signing and verification service." \
  --label "type: feature,priority: p1-high" \
  --milestone "Sprint 1: User Onboarding Engine")

# Extract Issue Numbers
SUB1_NUM=$(echo $SUB1_URL | grep -oE '[0-9]+$')
SUB2_NUM=$(echo $SUB2_URL | grep -oE '[0-9]+$')

# 3. Create Parent Epic Issue referencing Sub-Issues
gh issue create \
  --title "EPIC: User Onboarding & Authentication Architecture" \
  --body "### Overview\nMaster tracking issue for the User Onboarding Engine.\n\n### Deliverables\n- [ ] #$SUB1_NUM\n- [ ] #$SUB2_NUM\n- [ ] Write integration test suite" \
  --label "type: feature,priority: p0-blocker" \
  --milestone "Sprint 1: User Onboarding Engine"
```

---

## 5. Expected Result
Navigating to your repository on GitHub displays:
- A standardized, color-coded label list.
- An active Milestone with a due date and $0\%$ completion.
- A master Epic issue rendering an interactive tasklist (`0 of 3 tasks completed`) with active links to the sub-issues.

---

## 6. Verification
Inspect the created issues and milestone status via CLI:

```bash
gh issue list --milestone "Sprint 1: User Onboarding Engine"
gh milestone list
```

---

## 7. Troubleshooting & Deliberate Failure Scenarios (BREAK & FIX)

### 🔴 Failure Scenario 1: Misfiring Multi-Issue Auto-Close Keywords
**The Break**: A developer writes a PR description attempting to close both Sub-Issue 1 and Sub-Issue 2:
```text
This PR completes the onboarding database and auth service.
Fixes #1, #2 and closes #3
```
*Problem*: When the PR is merged into `main`, Issue `#1` and `#3` close automatically, but **Issue `#2` stays open!**

**The Fix (Root Cause & Solution)**:
1. **Root Cause**: GitHub's keyword parser requires an explicit closing keyword before **EVERY** issue reference. `, #2` is treated as a regular text mention without closing intent.
2. **Correct Syntax**:
   ```text
   Fixes #1, fixes #2, and closes #3
   ```
3. Update PR descriptions to ensure every target issue is preceded by a recognized keyword (`fixes`, `closes`, `resolves`).

---

### 🔴 Failure Scenario 2: PR Merged into Non-Default Branch Fails to Close Issue
**The Break**: A developer creates a PR with description `Fixes #10`. The PR targets branch `develop` (or a feature branch) instead of `main`.
*Problem*: When the PR merges into `develop`, Issue `#10` remains Open! The developer assumes GitHub automation is broken.

**The Fix (Understanding the Default Branch Rule)**:
1. GitHub intentionally **only** auto-closes issues when changes reach the repository's **default branch (`main`)**.
2. When the `develop` branch is eventually merged into `main` in a subsequent PR, GitHub evaluates the merged commit history and **automatically closes Issue `#10`** at that moment!
3. To close immediately for non-default branches, the issue must be closed manually or through custom GitHub Actions.

---

### 🔴 Failure Scenario 3: Broken Tasklist Markdown Syntax
**The Break**: A developer writes a tasklist with improper spacing:
```markdown
-[] #1
-[x]#2
* [ ]#3
```
*Problem*: GitHub renders the text as raw characters; no interactive checkboxes or progress bars appear at the top of the issue.

**The Fix**:
Ensure strict GFM spacing syntax:
```markdown
- [ ] #1
- [x] #2
- [ ] #3
```
*(Dash, space, bracket, space/x, bracket, space, reference).*

---

## 8. Cleanup
```bash
cd ~/git-mastery-labs/lab-04
```
*(Note: Close the test issues and milestone using `gh issue close <num>` and `gh milestone delete <num>` if testing in a live repository).*

---

## 9. Extension Challenge
Create a Project v2 board via the GitHub web UI or GraphQL CLI, configure a custom Single-Select field `Component` (`Auth`, `Billing`, `UI`), and build a Table View grouped by `Component`.
