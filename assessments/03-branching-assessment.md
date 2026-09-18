# Level 3 Assessment: Branching & Professional Git Workflows

---

## 🎯 Assessment Overview
This assessment tests your understanding of branching strategies, merge DAG transformations, interactive rebasing, 3-way conflict resolution, and `git rerere` automation.

**Passing Score**: 100% on Safety & Rebase items, $\ge 85\%$ overall.

---

## 📝 Section 1: Conceptual & DAG Mastery

### Question 1: DAG Properties of a 3-Way Merge Commit
How many parent pointers does a commit created by `git merge --no-ff` have?
- [ ] A) Zero.
- [ ] B) One.
- [ ] C) Two (or more for octopus merges).
- [ ] D) Three.

### Question 2: The Golden Rule of Rebasing
Why should you never rebase commits that have already been pushed and shared with other developers on a public branch?
- [ ] A) Git will delete the repository.
- [ ] B) Rebasing generates new commit SHA hashes, forcing other developers into divergent history conflicts.
- [ ] C) Rebasing converts all files to Windows CRLF line endings.
- [ ] D) GitHub Actions will permanently block the repository.

### Question 3: `zdiff3` vs Standard Conflict Markers
What extra information does `merge.conflictstyle zdiff3` display inside a conflict block?
- [ ] A) The author of each conflicting commit.
- [ ] B) The original code as it existed in the Common Ancestor (Base) commit (`||||||| base`).
- [ ] C) The timestamp of the merge operation.
- [ ] D) The GitHub pull request ID.

### Question 4: `git push --force-with-lease` vs `--force`
What makes `git push --force-with-lease` safer than standard `git push --force`?
- [ ] A) It encrypts the push with SSL.
- [ ] B) It automatically resolves merge conflicts.
- [ ] C) It checks whether the remote branch has been updated by someone else since your last fetch before overwriting.
- [ ] D) It asks for a 2FA verification code.

---

## 🔍 Section 2: Rebase & Conflict Diagnostics

### Scenario A: The Accidental Skip
During a 5-commit interactive rebase, you encounter a merge conflict. In a hurry, you run `git rebase --skip`.
1. What did `git rebase --skip` actually do to the conflicting commit?
2. How do you abort the entire rebase immediately to prevent data loss?

### Scenario B: Auto-Resolving with `git rerere`
You frequently rebase a long-lived feature branch against `main`.
1. What Git configuration enables recording and reusing conflict resolutions?
2. Where does Git store recorded conflict resolutions on disk?

---

## 🛠️ Section 3: Practical Verification Task

Run the following test in a sandbox terminal:

```bash
# 1. Create a test repo
mkdir test-rebase-eval && cd test-rebase-eval && git init

# 2. Configure zdiff3 and rerere
git config merge.conflictstyle zdiff3
git config rerere.enabled true

# 3. Verify settings
git config --get merge.conflictstyle
git config --get rerere.enabled
```

Confirm that output matches:
- `merge.conflictstyle`: `zdiff3`
- `rerere.enabled`: `true`

---

## 🏆 Section 4: Level 3 Mastery Criteria Checklist

Sign off each requirement before moving to **Level 4: Issues, Projects & Collaboration**:

- [ ] **Branching Strategy**: Can explain Trunk-Based Development vs. GitHub Flow and the role of feature flags.
- [ ] **Merge Strategies**: Understands Fast-Forward, Merge Commits, Squash, and Rebase DAG graphs.
- [ ] **Interactive Rebasing**: Can squash, fixup, reword, edit, split, and drop commits using `git rebase -i`.
- [ ] **Safe Force Pushing**: Enforces `git push --force-with-lease`.
- [ ] **Conflict Surgery**: Mastered `zdiff3` conflict resolution.
- [ ] **Rerere Automation**: Enabled and understands `git rerere` conflict reuse.

---

## 🔑 Answers & Explanations

<details>
<summary>👉 Click to reveal assessment answers & explanations</summary>

### Section 1 Answers:
1. **C** — 3-Way merge commits have 2 parent pointers (or more for octopus merges).
2. **B** — Rebasing replays commits onto a new base, generating completely new SHA hashes and rewriting history.
3. **B** — `zdiff3` displays the common ancestor base code between `||||||| base` and `=======`.
4. **C** — `--force-with-lease` ensures you don't overwrite unexpected remote updates pushed by teammates.

### Section 2 Explanations:
**Scenario A**:
1. `git rebase --skip` discards the entire commit and all of its unique code changes!
2. `git rebase --abort`.

**Scenario B**:
1. `git config --global rerere.enabled true` (and `rerere.autoupdate true`).
2. `.git/rr-cache/`.
</details>
