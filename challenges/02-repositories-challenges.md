# Level 2 Challenges: Repository Architecture & Hygiene

---

## 🎯 Challenge System Overview
Attempt each tier independently. These challenges develop enterprise-grade competence in repository governance, ignore patterns, line ending normalization, and automated review routing.

---

### 🟢 Tier 1: Basic Usage — Complex `.gitignore` Glob Engine
**Objective**: Author a `.gitignore` file that satisfies all of the following rules simultaneously:
1. Ignore all `.log` files anywhere in the project.
2. Re-include only `/var/log/audit.log`.
3. Ignore all subdirectories named `build` or `dist` at any directory depth.
4. Ignore all files ending with `.tmp` inside `/src/`, but NOT inside `/tests/`.
5. Ignore all `.env` files except `.env.example` and `.env.test`.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Use <code>git check-ignore -v &lt;path&gt;</code> to test each condition against sample files.
> </details>

---

### 🟡 Tier 2: Real Project Scenario — Multi-Team `CODEOWNERS` Matrix
**Objective**: Design a comprehensive `.github/CODEOWNERS` file for a large enterprise organization with the following governance policies:
1. Default catch-all maintainers: `@enterprise/core-infra`.
2. All Python (`*.py`) and Go (`*.go`) files: `@enterprise/backend-guild`.
3. All TypeScript (`*.ts`, `*.tsx`) and CSS files: `@enterprise/frontend-guild`.
4. The entire `/infrastructure/terraform/` directory: `@enterprise/sre-team`.
5. The `/services/payments/` directory: Must require sign-off from BOTH `@enterprise/payments-team` AND `@security-auditors`.
6. Documentation files in `/docs/` and all `*.md` files: `@enterprise/technical-writers`.

---

### 🟠 Tier 3: Failure / Debugging Scenario — Monorepo CRLF Disaster
**Scenario**:
A team member on Windows checked out a monorepo containing 1,200 shell scripts, Python files, and React components. When they made a 1-line change to `README.md` and pushed, their PR showed 850 files modified with full-file deletion/addition diffs due to global `CRLF` replacement.

**Task**:
1. Formulate the exact `.gitattributes` configuration required to permanently prevent this issue across all operating systems.
2. Write the sequence of Git commands to normalize the working directory and index without losing the developer's legitimate 1-line change.
3. Verify that `git diff --staged` shows only the 1-line change in `README.md`.

---

### 🔴 Tier 4: Professional / Enterprise Scenario — Incident Bug Report Issue Form
**Objective**:
Author a production-grade YAML Issue Form (`.github/ISSUE_TEMPLATE/incident_report.yml`) for production incidents with the following schema:
1. Form Name: `🚨 Production Incident Report`
2. Auto-applied labels: `incident`, `p0-urgent`, `needs-postmortem`
3. Dropdown for `Impacted Region` (`US-East`, `EU-Central`, `AP-South`, `Global`).
4. Required input for `Incident Commander / On-Call Lead`.
5. Dropdown for `Severity Level` (`SEV-1: Outage`, `SEV-2: Degraded`, `SEV-3: Minor`).
6. Textarea with `render: shell` for `Error Logs / Metrics Dashboard Links`.
7. Required checkboxes confirming immediate mitigation steps were taken.

---

### 🟣 Tier 5: Independent Implementation — The "Repo Linter" Script
**Objective**:
Build an automated verification script (`repo-linter.sh` or `repo-linter.py`) that audits any Git repository for compliance with enterprise governance and hygiene standards.

**The Script Must Verify**:
1. `README.md`, `LICENSE`, `SECURITY.md`, and `CONTRIBUTING.md` exist and are non-empty.
2. `.gitignore` exists and explicitly blocks `.env` and `*.log`.
3. `.gitattributes` exists and contains `* text=auto`.
4. `.github/CODEOWNERS` exists and has valid syntax (every line has a pattern and at least one owner starting with `@` or a valid email).
5. `.github/ISSUE_TEMPLATE/config.yml` exists with `blank_issues_enabled: false`.
6. No tracked files exist in the repository that match `.gitignore` rules.
7. Outputs an exit code of `0` on success, or `1` with a formatted checklist of failing rules.
