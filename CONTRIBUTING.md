# Contributing to GitHub Mastery for Developers

Thank you for your interest in contributing to **GitHub Mastery for Developers (`github-zero-to-hero`)**!

This project is a maintained developer education system. To ensure high pedagogical quality, accuracy, and longevity, all contributions must follow the standards outlined below.

---

## 🎯 Dual Contributor Roles

### 1. The Learner Contributor
- Submitting clarifications, typos, broken link fixes, or reporting outdated platform UI flows.
- Submitting additional diagnostic tips or edge-case troubleshooting in the **Common Mistakes** or **Troubleshooting** sections of existing labs.

### 2. The Curriculum Maintainer Contributor
- Proposing new lessons, labs, challenges, or architectural updates.
- Updating workflows, action versions, or security checks in response to GitHub platform updates.

---

## 📐 Curriculum Authoring Standards

### 1. The 19-Part Lesson Specification
All major lessons added to `00-foundations/` through `15-capstone/` must implement all 19 standardized sections:
1. `Learning Objective`
2. `Why This Matters`
3. `Prerequisites`
4. `Concept Explanation` (Why before How)
5. `Mental Model` (Includes Mermaid or ASCII diagrams)
6. `Real-World Use Case`
7. `Official GitHub Documentation` (Canonical links)
8. `Recommended High-Quality External Resource`
9. `Step-by-Step Demonstration`
10. `Hands-on Lab`
11. `Challenge`
12. `Common Mistakes & Gotchas`
13. `Professional Practices`
14. `Security Considerations`
15. `Interview Questions` (What $\to$ How $\to$ Why $\to$ What-If $\to$ Trade-Offs)
16. `Real-World Scenario`
17. `Assessment`
18. `Mastery Criteria`
19. `Further Exploration`

### 2. The 9-Part Lab Framework
Every hands-on lab in `labs/` must contain:
1. `Objective`
2. `Prerequisites`
3. `Setup`
4. `Instructions`
5. `Expected Result`
6. `Verification`
7. `Troubleshooting`
8. `Cleanup`
9. `Extension Challenge`

### 3. Deliberate Failure Scenarios (Break/Fix)
Every major module must feature an intentional failure simulation (merge conflict, bad rebase, broken workflow, permission error, secret leak) with clear diagnostic and recovery steps.

---

## 🔒 Security & Code Standards

- **Zero Real Secrets**: NEVER commit real PATs, private keys, AWS keys, or connection strings. Always use mock placeholders (e.g. `ghp_mockToken1234567890`, `AKIAIOSFODNN7EXAMPLE`).
- **Markdown Style**: Use standard GitHub Flavored Markdown (GFM). Use callout alerts (`[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, `[!CAUTION]`) sparingly and meaningfully.
- **Diagrams**: All architecture and workflow diagrams must use Mermaid (`mermaid` code blocks).

---

## 🔄 Pull Request Process

1. **Create an Issue**: Before submitting substantial changes or new lessons, open an issue using the appropriate template.
2. **Branch Naming**: Use descriptive branches: `feat/<level>-<topic>`, `fix/<issue-number>-<description>`, `docs/<topic>`.
3. **Commit Messages**: Follow Conventional Commits:
   ```text
   docs(foundations): add mental model diagram for git object database
   feat(actions): add matrix build lab with failure recovery
   fix(labs): update deprecated gh cli command syntax in lab 04
   ```
4. **Validation**: Ensure all internal markdown links and diagrams render without errors.
5. **Review**: All PRs require review from a module CODEOWNER before merging.
