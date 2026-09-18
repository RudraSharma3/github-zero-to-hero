# Level 4 Challenges: Agile Issues, Projects & Automation

---

## 🎯 Challenge System Overview
Attempt each tier independently. These challenges develop enterprise-grade mastery across GitHub agile project management, sprint planning, label engineering, and workflow automation.

---

### 🟢 Tier 1: Basic Usage — Automated Label Provisioning Script
**Objective**:
Author a shell script (`setup-labels.sh`) using GitHub CLI (`gh label`) that:
1. Deletes the default GitHub labels (`bug`, `enhancement`, `wontfix`, `duplicate`, `question`, `invalid`).
2. Provisions a comprehensive 12-label namespaced taxonomy covering `type:*`, `priority:*`, `status:*`, and `area:*` with hex color palettes and descriptions.
3. Successfully runs against any target repository idempotently (`--force`).

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Use <code>gh label delete "&lt;name&gt;" --yes</code> followed by <code>gh label create "&lt;name&gt;" --color "&lt;hex&gt;" --description "&lt;desc&gt;" --force</code>.
> </details>

---

### 🟡 Tier 2: Real Project Scenario — Sprint Milestone Planning
**Objective**:
Using GitHub CLI (`gh`), establish a complete sprint delivery structure for a 2-week sprint:
1. Create a Milestone titled `Sprint 25: Payment Architecture` with a due date set to 14 days from today.
2. Create 3 discrete user story issues with titles, markdown descriptions, `type: feature` labels, and assign them to the milestone.
3. Create 1 parent Epic issue that embeds a markdown tasklist referencing the 3 story issue numbers.
4. Verify via CLI that the Milestone lists all 4 items and tracks progress.

---

### 🟠 Tier 3: Failure / Debugging Scenario — The Unclosed Production Bug
**Scenario**:
A developer submitted a PR intended to resolve 3 critical bugs: `#101` (Internal DB crash), `#102` (UI overflow), and `external-org/sdk#45` (SDK timeout).
The PR description read:
`Resolved issue #101, #102 and external-org/sdk#45.`
When the PR merged into `main`, `#101` closed, but `#102` and the external SDK issue remained open.

**Task**:
1. Explain the exact syntax flaws in the PR description.
2. Rewrite the description with correct closing keyword syntax for all 3 issues.
3. Explain what permissions are required on `external-org/sdk` for the external issue to close automatically.

---

### 🔴 Tier 4: Professional / Enterprise Scenario — Multi-Repo Organization Portfolio Board
**Objective**:
Design the complete architecture for an enterprise-level GitHub Project v2 board tracking 4 distinct microservice repositories (`auth-api`, `billing-api`, `web-frontend`, `mobile-app`):
1. **Views**: Configure 3 dedicated views:
   - `Sprint Backlog`: Table view grouped by `Iteration`, filtered by `status:-Done`.
   - `Team Kanban`: Board view grouped by `Status` (`Backlog`, `In Progress`, `In Review`, `Done`).
   - `Quarterly Timeline`: Roadmap view tracking items by `Start Date` and `Target Date`.
2. **Automations**: Define the workflow rules to automatically add issues labeled `sprint-candidate` and auto-archive items in `Done` after 14 days.

---

### 🟣 Tier 5: Independent Implementation — The "Issue Triage Bot"
**Objective**:
Author a Node.js or Python CLI script (`issue-triage-bot.py` / `issue-triage-bot.js`) that uses the GitHub CLI or REST API to perform automated daily issue hygiene:

**The Script Must**:
1. Query all open issues labeled `status: triage-needed`.
2. Check if the issue description is less than 50 characters:
   - If true, add a comment requesting reproduction steps and apply label `status: awaiting-info`.
3. Check if the issue title begins with `[BUG]` or `[FEAT]`:
   - Auto-apply `type: bug` or `type: feature` accordingly.
4. If the issue has no assignee, assign the designated on-call maintainer.
5. Remove `status: triage-needed` once processed.
