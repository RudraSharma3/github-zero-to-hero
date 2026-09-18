# Project Conventions

This document establishes curriculum authoring standards, pedagogical frameworks, coding standards, git workflows, and testing patterns for this repository.

---

## 1. Pedagogical Conventions & Depth Levels

Every topic is explored across three progressive depths:
- **Beginner**: Conceptual understanding, terminology, mental model, and basic purpose.
- **Developer**: Practical execution in a real repository workflow, syntax, and CLI/UI operations.
- **Professional**: Architectural trade-offs, security, automation, scale, failure recovery, and enterprise governance.

---

## 2. Standardized 19-Part Lesson Structure

To ensure consistency across all levels (`00-foundations` to `15-capstone`), major lessons follow this 19-part specification:

1. **Learning Objective**: Precise statement of what the learner will achieve.
2. **Why This Matters**: Real-world relevance and practical developer impact.
3. **Prerequisites**: Required knowledge, tools, and previous modules.
4. **Concept Explanation**: Clear, deep technical explanation (Why before How).
5. **Mental Model**: Intuitive analogy, ASCII/Mermaid visual diagram.
6. **Real-World Use Case**: Enterprise or open-source production scenario.
7. **Official GitHub Documentation**: Canonical GitHub Docs reference links.
8. **Recommended High-Quality External Resource**: Singular, high-value vetted guide or video.
9. **Step-by-Step Demonstration**: Walkthrough of core mechanics with annotated commands.
10. **Hands-on Lab**: Reproducible practical exercise.
11. **Challenge**: Problem-solving prompt without immediate answers.
12. **Common Mistakes**: Anti-patterns, gotchas, and typical learner errors.
13. **Professional Practices**: Production patterns, team conventions, and clean history.
14. **Security Considerations**: Secrets safety, permissions, PoLP, and vulnerability vectors.
15. **Interview Questions**: Progression from What → How → Why → What-If → Trade-Offs.
16. **Real-World Scenario**: Incident simulation or complex architecture dilemma.
17. **Assessment**: Quiz, practical verification checklist, or diagnostic task.
18. **Mastery Criteria**: Explicit checklist defining completion.
19. **Further Exploration**: Advanced GitHub features, RFCs, and roadmap pointers.

---

## 3. Hands-on Lab Standards (9-Part Framework)

All labs in `labs/` or level directories must implement this 9-part structure:
1. **Objective**: Target outcome of the lab.
2. **Prerequisites**: Tools, account setup, environment needs.
3. **Setup**: Sandbox repo initialization commands or starter state.
4. **Instructions**: Clear, step-by-step guidance explaining each command.
5. **Expected Result**: Terminal output or GitHub UI state snippet.
6. **Verification**: Command or check to confirm correct execution.
7. **Troubleshooting**: Solutions for common errors encountered during the lab.
8. **Cleanup**: Instructions to reset local/remote test state cleanly.
9. **Extension Challenge**: Advanced follow-up task.

---

## 4. 5-Tier Challenge System

Challenges follow progressive difficulty tiers:
- **Tier 1 (Basic Usage)**: Syntax execution and basic UI navigation.
- **Tier 2 (Real Project Scenario)**: Applying the concept in a multi-file feature.
- **Tier 3 (Failure / Debugging Scenario)**: Recovering from broken states (merge conflicts, detached HEAD, failed CI).
- **Tier 4 (Professional / Production Scenario)**: Multi-branch, multi-user, or automated enterprise workflows.
- **Tier 5 (Independent Implementation)**: Open-ended problem requiring full end-to-end design.

---

## 5. Markdown & Documentation Style

- **Headings**: Single `#` title, structured `##` and `###` hierarchy.
- **Diagrams**: Use Mermaid (`mermaid`) for workflows, architecture, git trees, and sequence diagrams.
- **Alerts**: Use GitHub alert syntax strategically:
  - `> [!NOTE]` for contextual background.
  - `> [!TIP]` for developer efficiency tricks.
  - `> [!IMPORTANT]` for mandatory prerequisites and requirements.
  - `> [!WARNING]` for common pitfalls or breaking behaviors.
  - `> [!CAUTION]` for high-risk operations (e.g. `git push --force`, hard reset).
- **Links**: Clickable links with correct markdown syntax (`[text](url)` or `[file](file:///...)`).
- **Code Blocks**: Always specify language identifier (e.g. `bash`, `yaml`, `json`, `markdown`).

---

## 6. Git & Version Control Conventions

- **Commit Format**: Conventional Commits: `<type>(<scope>): <short imperative summary>`
  - Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`
  - Example: `docs(foundations): add mental model diagram for git object database`
- **Branch Naming**: `<type>/<short-description>` (e.g., `feat/level-0-foundations`, `docs/conventions-update`).
- **No Emojis**: Keep commit messages and technical docs professional without arbitrary emojis.

---

## 7. Security Conventions

- **No Hardcoded Secrets**: Use mock placeholders (e.g., `ghp_mockToken1234567890`).
- **Pre-commit Hooks**: Enforce secret scanning via `.githooks/pre-commit`.
- **Workflow Permissions**: Explicit `permissions:` stanza on every GitHub Actions workflow.

