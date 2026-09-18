# Level 3 Challenges: Branching & Professional Git Workflows

---

## 🎯 Challenge System Overview
Attempt each tier independently. These challenges develop high-level competence in history crafting, interactive rebasing, merge conflict resolution, and automated workflow hygiene.

---

### 🟢 Tier 1: Basic Usage — History Sculpting with `git rebase -i`
**Objective**:
You have a feature branch with the following 5 draft commits:
```text
c1: feat(auth): add login endpoint
c2: fix: typo in login endpoint
c3: test: add unit test for login
c4: wip: temporary console logs
c5: docs: update api documentation
```
**Requirements**:
1. Use `git rebase -i` to combine `c2` into `c1` using `fixup`.
2. Drop `c4` completely.
3. Reorder `c5` (documentation) so that it comes immediately after `c1`.
4. End up with exactly 3 clean, atomic commits: `feat(auth)`, `docs(auth)`, `test(auth)`.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Reorder the lines in the rebase todo list and change commands from <code>pick</code> to <code>fixup</code> or <code>drop</code>.
> </details>

---

### 🟡 Tier 2: Real Project Scenario — Splitting an Accidental Monolithic Commit
**Objective**:
A developer accidentally committed both an urgent security patch in `auth.py` and a large cosmetic redesign of `dashboard.tsx` in a single commit titled `fix: auth and redesign`.

**Requirements**:
1. Use `git rebase -i` and the `edit` verb to pause at that commit.
2. Unstage the commit using `git reset HEAD~1`.
3. Create Commit 1: `fix(auth): sanitize user input in authentication handler`.
4. Create Commit 2: `feat(ui): update dashboard visual theme`.
5. Run `git rebase --continue` to finish with a clean linear history.

---

### 🟠 Tier 3: Failure / Debugging Scenario — The Bad Rebase & Lost Release
**Scenario**:
A developer ran `git rebase -i main` on a release candidate branch. Midway through resolving conflicts, they accidentally ran `git rebase --skip` multiple times, effectively deleting 4 critical bugfix commits from the branch!

**Task**:
1. Formulate the exact commands using `git reflog` to find the commit SHA where the branch stood immediately before the rebase started.
2. Restore the branch pointer to that exact commit using `git reset --hard`.
3. Explain why `git rebase --skip` should almost never be used during conflict resolution.

---

### 🔴 Tier 4: Professional / Enterprise Scenario — Long-Lived Branching with `git rerere`
**Objective**:
You are maintaining a 6-month feature branch (`v2-engine`) that undergoes weekly rebases against `main`.
1. Configure your local repository to record and automatically reuse conflict resolutions.
2. Trigger a 3-way merge conflict on `database.py`.
3. Resolve the conflict and record the resolution.
4. Abort the merge and perform a rebase to demonstrate that `git rerere` auto-resolves the conflict with zero manual intervention.

---

### 🟣 Tier 5: Independent Implementation — The "Clean PR History" Git Pre-Push Hook
**Objective**:
Author a Git pre-push hook script (`.githooks/pre-push` or `.git/hooks/pre-push`) that inspects all outgoing commits on a feature branch before allowing `git push`.

**The Hook Must Block Push If**:
1. Any commit message begins with `wip:`, `fix typo`, `temp`, or `test`.
2. Any commit in the push series contains un-resolved merge conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
3. Any commit is empty (`git diff-tree` shows zero file changes).
4. Outputs a descriptive error message guiding the developer to run `git rebase -i` if validation fails.
