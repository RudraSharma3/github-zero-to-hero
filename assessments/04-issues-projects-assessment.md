# Level 4 Assessment: Issues, Projects & Agile Collaboration

---

## 🎯 Assessment Overview
This assessment evaluates your practical ability to design label systems, manage sprint milestones, structure tasklists, configure GitHub Projects v2, and debug automated issue closing workflows.

**Passing Score**: 100% on Safety & Workflow items, $\ge 85\%$ overall.

---

## 📝 Section 1: Conceptual & Workflow Mastery

### Question 1: Closing Keywords Multi-Issue Syntax
Which of the following PR descriptions will successfully auto-close both Issue #10 and Issue #20 when merged to `main`?
- [ ] A) `Closes #10 and #20`
- [ ] B) `Closes #10, closes #20`
- [ ] C) `Fix issue 10 & 20`
- [ ] D) `Resolved: 10, 20`

### Question 2: The Default Branch Auto-Close Rule
A developer opens a PR targeting branch `feature/auth-v2` with description `Fixes #50`. When the PR merges into `feature/auth-v2`, what happens to Issue #50?
- [ ] A) Issue #50 closes immediately.
- [ ] B) Issue #50 remains Open until `feature/auth-v2` is merged into the default branch (`main`).
- [ ] C) Issue #50 is permanently locked.
- [ ] D) Issue #50 is deleted from the repository.

### Question 3: Discussions vs. Issues
Which scenario is best suited for a **GitHub Discussion** rather than a **GitHub Issue**?
- [ ] A) "Database query on /users endpoint times out after 30 seconds."
- [ ] B) "RFC: Exploring architectural trade-offs between REST and GraphQL for our v3 mobile API."
- [ ] C) "Add a 'Remember Me' checkbox to the login form."
- [ ] D) "Fix CSS alignment on the checkout button in Safari."

### Question 4: Projects v2 Built-in Automations
What is the primary benefit of enabling the **Auto-archive** workflow in a GitHub Project v2?
- [ ] A) It permanently deletes old issues from the repository to save disk space.
- [ ] B) It moves completed cards out of the active board view after a specified number of days, keeping boards fast and focused.
- [ ] C) It archives the entire Git repository to a tarball.
- [ ] D) It sends an automated email report to the team lead.

---

## 🔍 Section 2: Diagnostics & Scenario Analysis

### Scenario A: The Broken Tasklist
An engineer creates an issue description:
```markdown
### Deliverables
-[x] #12
- [ ]#13
* [x] #14
```
Explain why GitHub fails to render the first two lines as interactive checkboxes.

### Scenario B: The Cross-Repository Reference
You are working in repository `company/mobile-app`. You want to link to a bug report in `company/backend-api` (Issue #88) and ensure that when your PR in `mobile-app` merges to `main`, the backend issue is automatically closed.
Write the exact keyword line to include in your PR description.

---

## 🛠️ Section 3: Practical Verification Task

Run the following test in your terminal using GitHub CLI (`gh`):

```bash
# 1. Verify gh auth status
gh auth status

# 2. View active labels in current repo
gh label list --limit 10
```

Confirm that output shows successful authentication and lists existing repository labels.

---

## 🏆 Section 4: Level 4 Mastery Criteria Checklist

Sign off each requirement before moving to **Level 5: Pull Requests & Code Review**:

- [ ] **Issues & Tasklists**: Builds nested tasklists and understands sub-issue parent-child graphs.
- [ ] **Closing Keywords**: Flawlessly writes multi-issue closing keywords (`Closes #A, closes #B`).
- [ ] **Default Branch Mechanics**: Explains why auto-closing only triggers on default branch merges.
- [ ] **Label Taxonomies**: Understands namespaced label hierarchies (`type:`, `priority:`, `area:`).
- [ ] **Milestones**: Configures sprint milestones with due dates and tracks burndown progress.
- [ ] **Projects v2**: Configures Table, Board, and Roadmap views with custom Iteration and Number fields.
- [ ] **Automated Workflows**: Enables native project automations (Auto-Add, Auto-Status, Auto-Archive).
- [ ] **Discussions vs Issues**: Can articulate when to use Discussions for ideation vs Issues for execution.

---

## 🔑 Answers & Explanations

<details>
<summary>👉 Click to reveal assessment answers & explanations</summary>

### Section 1 Answers:
1. **B** — GitHub requires repeating the keyword before each issue number (`Closes #10, closes #20`).
2. **B** — Auto-closing only executes when code reaches the default branch (`main`). Merging into intermediate feature branches only creates a reference link.
3. **B** — RFCs, brainstorming, and open-ended design discussions belong in GitHub Discussions.
4. **B** — Auto-archive hides closed cards after a set duration (e.g. 14 days) to maintain board cleanliness without deleting data.

### Section 2 Explanations:
**Scenario A**:
- Line 1 missing space after dash (`- [x]`).
- Line 2 missing space after bracket (`- [ ] #13`).

**Scenario B**:
`Closes company/backend-api#88` (or `Fixes company/backend-api#88`).
</details>
