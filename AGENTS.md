# Project Instructions

Read this entire file before starting any task on this project.

---

## 1. Project Purpose

**GitHub Mastery for Developers (`github-zero-to-hero`)** is an interactive, hands-on developer training system and maintained educational repository designed to teach GitHub and Git from fundamental mental models to enterprise-grade engineering workflows.

### Core Audiences:
1. **Learner**: A developer, engineer, or student seeking complete practical competence across version control, CI/CD, security, automation, and collaborative platform operations.
2. **Maintainer**: The course author and maintainers who continuously evolve, validate, and update the curriculum alongside GitHub platform updates.

### Core Value Proposition & Pedagogy:
- **`LEARN → DO → BREAK → FIX → EXPLAIN → APPLY`**: Replaces passive reading with deliberate hands-on labs, failure scenarios (merge conflicts, detached HEAD, leaked secrets, broken CI), multi-tier challenges, and comprehensive capstone workflows.
- **Living Example Repository**: The repository itself demonstrates the enterprise patterns it teaches (GitHub Actions, Branch Rulesets, Issue/PR Templates, CODEOWNERS, Dependabot, CodeQL, Semantic Versioning).

---

## 2. Required Reading

Before taking action, review:
- `docs/architecture.md` (System context, components, data flows)
- `docs/graph.md` (Component hierarchy, route call paths, blast radius matrix)
- `docs/conventions.md` (Code style, git workflows, test patterns)
- `docs/task-state.md` (Active task progress, goals, blockers)
- Relevant records in `docs/decisions/` (Accepted architectural choices)
- `docs/lessons.md` (Verified project lessons)

---

## 3. Environment & Commands

<!-- Exact development and validation commands for this repository -->
- **Install / Setup**: `npm install` (or local markdown/linter toolchain)
- **Lint / Format**: `npx prettier --check .` / `npx markdownlint-cli2 "**/*.md"`
- **Test / Verify Labs**: `npm test` (or validation test runners in `labs/` and `assessments/`)
- **Link Check**: `npx markdown-link-check **/*.md`
- **Build / Doc Gen**: `npm run build`

---

## 4. Engineering Rules

1. Make the smallest surgical change that satisfies the acceptance criteria.
2. Follow established patterns in `docs/conventions.md` and `docs/architecture.md`.
3. Never modify unrelated files or alter working behavior without justification.
4. Always add or update tests for functional changes.
5. Record material architectural trade-offs as ADRs in `docs/decisions/`.
6. Never commit secrets, credentials, API keys, or private tokens to version control.
7. Automatically optimize brief or messy user prompts into full engineering specifications (architecture alignment, edge cases, error handling, security) in `docs/task-state.md` before executing.
8. Practice Proportional Effort: Use Solo Model execution for routine tasks; reserve Multi-Agent MCP orchestration and Consensus sampling strictly for high-complexity architectural decisions.

---

## 5. Self-Modifying Learning Loop

When the user gives a direct correction or states a recurring preference:
1. **Append Rule**: Immediately assign the next sequential number and append it to `## 6. Learned Rules` below.
   - Format: `N. [CATEGORY] Rule description — rationale.`
   - Allowed Categories: `[STYLE]`, `[CODE]`, `[ARCH]`, `[TOOL]`, `[PROCESS]`, `[DATA]`, `[UX]`, `[SECURITY]`, `[TEST]`, `[OTHER]`.
2. **Supersede Obsolete Rules**: If a new preference replaces an older rule, mark the older rule with `- SUPERSEDED BY RULE N.` (never silently delete).
3. **Generalize Secrets**: Never store raw credentials or API keys; generalize into security practices.
4. **Notify User**: Explicitly state in the response: `🧠 Learned Rule Added to AGENTS.md: N. [CATEGORY] Rule — rationale.`

For unverified internal errors or complex edge cases, log a candidate first in `.ai/mistakes-candidates.md`.

---

## 6. Learned Rules

<!--
Validated project-specific rules are appended below sequentially.
Format: N. [CATEGORY] Rule description — rationale.
-->

1. [PROCESS] Read `docs/task-state.md` and `docs/architecture.md` before writing code — ensures alignment with current project context and architecture.
2. [SECURITY] Never hardcode secrets or private keys — load credentials through environment variables or configuration managers.
3. [TEST] Run project test suite and verify changes before declaring task completion — ensures code quality and prevents regressions.
