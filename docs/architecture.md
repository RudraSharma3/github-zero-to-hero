# Architecture

This document is the durable source of truth for the project's technical architecture, component boundaries, pedagogical data flow, and operational governance.

---

## 1. System Purpose & Scope

**GitHub Mastery for Developers (`github-zero-to-hero`)** is a structured, production-grade developer training system and maintained educational repository. It transforms developers from basic users into proficient platform engineers capable of leveraging GitHub as an end-to-end software delivery and collaboration ecosystem.

### Target Audiences
1. **The Learner**: Developers, DevOps engineers, and students progressing through 16 structured levels (Level 0 Foundation to Level 15 Engineering Capstone) with practical, failure-scenario-driven labs.
2. **The Maintainer**: Course creators and contributors maintaining currency with GitHub's rapid feature releases, API changes, and platform deprecations through automated checks and modular curriculum design.

### Core Value Proposition & Pedagogy
- **Active Competence via `LEARN → DO → BREAK → FIX → EXPLAIN → APPLY`**: Every major concept includes hands-on labs, intentional failure scenarios (merge conflicts, detached HEAD, leaked credentials, broken CI/CD), structured debugging, and real-world synthesis.
- **Living Repository Pattern**: The course repository itself exemplifies enterprise GitHub engineering (Actions, Branch Rulesets, Issue/PR Templates, CODEOWNERS, Dependabot, CodeQL, Semantic Versioning, ADRs).

---

## 2. System Context & Boundaries

```mermaid
graph TB
    subgraph Users [Target Audiences]
        L[Learner Developer]
        M[Course Maintainer / Author]
    end

    subgraph CourseRepo [github-zero-to-hero Platform]
        CoreDoc[Curriculum Modules 00-15]
        LabSys[Interactive Labs & Break/Fix Engine]
        ChalSys[5-Tier Challenge System]
        AssessSys[Assessments & Rubrics]
        RefSys[Cheat Sheets & Official Resources]
        GovSys[Docs, ADRs, AGENTS & AI Context]
    end

    subgraph GitHubPlatform [GitHub Ecosystem Boundary]
        GH_VCS[Git & Repository Engine]
        GH_Collab[Issues, Projects & PRs]
        GH_CI[GitHub Actions & Runners]
        GH_Env[Codespaces & Dev Containers]
        GH_Sec[Secret Scanning, Dependabot, CodeQL]
        GH_Dist[Packages & Releases]
        GH_API[REST & GraphQL APIs, Webhooks]
        GH_AI[GitHub Copilot & Models]
    end

    subgraph Tooling [Developer Local Toolchain]
        GitCLI[Git CLI]
        GHCLI[GitHub CLI gh]
        Docker[Dev Containers / Docker Engine]
    end

    L -->|Clones / Forks / Reads| CourseRepo
    L -->|Executes Labs with| Tooling
    Tooling -->|Interacts with| GitHubPlatform
    M -->|Maintains & Updates via PRs| CourseRepo
    CourseRepo -->|Automated CI Validation| GH_CI
```

### External Boundaries & Interfaces
- **Git Version Control**: Core VCS layer for commits, branches, refs, merges, rebases, and history surgery.
- **GitHub Platform & Cloud Services**: Hosting, permissions, rulesets, pull requests, issue trackers, GitHub Projects v2, GitHub Actions workflow runners, and Codespaces cloud VMs.
- **Developer Local Tools**: `git`, `gh` (GitHub CLI), Docker/Dev Containers, IDEs (VS Code/Cursor/Antigravity).
- **APIs & Webhooks**: GitHub REST API v3, GraphQL API v4, and Webhook payloads for developer automation.

---

## 3. Component Architecture & Module Responsibilities

The codebase is organized into modular curriculum levels, hands-on labs, challenge tiers, and governance subsystems:

```
github-zero-to-hero/
├── README.md                      # Primary entry point, curriculum map, progress tracker
├── AGENTS.md                      # Agent operating rules & self-modifying learning loop
├── LICENSE                        # Open source license (MIT / CC)
├── CONTRIBUTING.md                 # Contribution guidelines & curriculum authoring standards
├── CODE_OF_CONDUCT.md             # Community standards
├── SECURITY.md                    # Vulnerability reporting & secret handling policy
├── CHANGELOG.md                   # Semantic versioning changelog
├── mcp.json                       # Model Context Protocol multi-agent configuration
├── .github/                       # Living GitHub infrastructure
│   ├── workflows/                 # CI validation, link check, markdown lint
│   ├── ISSUE_TEMPLATE/            # Structured issue forms for curriculum & bugs
│   ├── PULL_REQUEST_TEMPLATE.md   # PR checklist and review standards
│   ├── CODEOWNERS                 # Module ownership mapping
│   └── dependabot.yml             # Dependency security automation
├── 00-foundations/                # Level 0: Foundation & GitHub Mental Model
├── 01-git-fundamentals/           # Level 1: Git Mechanics, Plumbing & Porcelain
├── 02-repositories/               # Level 2: Repository Architecture & Hygiene
├── 03-branching-workflows/        # Level 3: Trunk-Based, GitHub Flow, Rebase vs Merge
├── 04-issues-projects/            # Level 4: Project Management, Projects v2, Automation
├── 05-pull-requests/              # Level 5: Code Review, Reviewer Workflows, PR Etiquette
├── 06-github-actions/             # Level 6: CI/CD Pipelines, Custom Actions, Reusable Workflows
├── 07-codespaces/                 # Level 7: Cloud Environments & Dev Containers
├── 08-security/                   # Level 8: Secret Scanning, Dependabot, CodeQL, Rulesets
├── 09-packages-releases/          # Level 9: GitHub Releases, SemVer, GitHub Packages
├── 10-github-cli/                 # Level 10: gh CLI Scripting & Extension Authoring
├── 11-api-automation/             # Level 11: REST, GraphQL, Webhooks & GitHub Apps
├── 12-organizations/              # Level 12: Enterprise, Teams, RBAC & Governance
├── 13-open-source/                # Level 13: Maintainership, RFCs, Triaging & Upstream PRs
├── 14-ai-development/             # Level 14: GitHub Copilot, Agents, Custom Instructions
├── 15-capstone/                   # Level 15: Production-Ready Engineering Capstone
├── labs/                          # Standalone hands-on lab environments & scripts
├── challenges/                    # 5-Tier practical challenges
├── assessments/                   # Level assessments, rubrics & automated validation
├── cheat-sheets/                  # Developer fast-reference cards
├── resources/                     # Curated official links, reading lists & diagrams
└── docs/                          # Architecture, ADRs, conventions, and state
    ├── architecture.md            # Durable system architecture (this document)
    ├── conventions.md             # Style guides, lesson templates & lab specs
    ├── graph.md                   # System dependency graphs & blast radius matrix
    ├── task-state.md              # Active progress & milestone tracker
    ├── lessons.md                 # Verified engineering lessons
    └── decisions/                 # Architectural Decision Records (ADRs)
```

---

## 4. Data Flow & Learning Lifecycle

The pedagogical data flow guides the learner through active reinforcement:

```mermaid
sequenceDiagram
    autonumber
    actor Learner
    participant Module as Level Curriculum (00-15)
    participant Lab as Hands-on Lab Environment
    participant Problem as Failure Scenario (Break Phase)
    participant Fix as Diagnostics & Solution (Fix Phase)
    participant Challenge as 5-Tier Challenge
    participant Assess as Level Assessment

    Learner->>Module: 1. Study Concepts & Mental Models (LEARN)
    Learner->>Lab: 2. Execute Step-by-Step Lab (DO)
    Lab->>Problem: 3. Inject Real-World Error / Conflict (BREAK)
    Note over Problem: Merge conflict, detached HEAD, leaked secret, broken CI
    Learner->>Fix: 4. Diagnose, Resolve & Verify (FIX)
    Fix->>Module: 5. Deepen Architectural Understanding (EXPLAIN)
    Learner->>Challenge: 6. Solve Progressive Challenges (APPLY)
    Learner->>Assess: 7. Complete Assessment & Mastery Rubric
```

---

## 5. Security & Trust Boundaries

1. **Zero Real Secrets Policy**:
   - Demonstration files and tutorials must NEVER use real API keys, SSH private keys, personal access tokens (PATs), or connection strings.
   - All examples must use explicit mock strings (e.g., `ghp_mockTokenForDemoPurposeOnly1234567890`, `AKIAIOSFODNN7EXAMPLE`).
2. **GitHub Actions Workflow Isolation**:
   - Strict `permissions` blocks defined per job in all `.github/workflows/`.
   - Workflows must pin actions to full commit SHAs for supply chain integrity.
3. **Secret Scanning & Pre-Commit Protection**:
   - Pre-commit hooks (`.githooks/pre-commit`) and GitHub push protection to block accidental leaks before reaching the remote repository.
4. **Principle of Least Privilege (PoLP)**:
   - Permissions across repository roles (Read, Triage, Write, Maintain, Admin) and GitHub App tokens are strictly bounded to the minimum necessary scope.

---

## 6. Operational Runbook & Maintenance

1. **Drift Detection & Platform Currency**:
   - Because GitHub UI and features evolve continuously, UI-specific steps are decoupled from stable CLI/API concepts.
   - All modules explicitly track standard GitHub terminology and provide GitHub Docs anchor links.
2. **Automated Documentation CI**:
   - PRs undergo automated markdown linting, formatting checks, and URL link validation.
3. **Semantic Versioning & Changelog**:
   - Major releases (`v1.0.0`, `v2.0.0`) correspond to curriculum milestones.
   - Breaking platform updates or structural revisions are logged in `CHANGELOG.md`.
4. **Architectural Decision Records (ADRs)**:
   - Significant curriculum or structural decisions are recorded in `docs/decisions/NNN-title.md`.
