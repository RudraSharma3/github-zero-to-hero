# Level 1 Assessment: Git Fundamentals & Internal Architecture

---

## 🎯 Assessment Overview
This assessment tests your understanding of the Three Trees, the Git object database, DAG graph relationships, and your ability to diagnose and recover from git disasters using plumbing tools and `git reflog`.

**Passing Score**: 100% on Safety & Recovery items, $\ge 85\%$ overall.

---

## 📝 Section 1: Conceptual & Object Model Mastery

### Question 1: What is stored inside a Blob object?
- [ ] A) File contents, filename, and file permissions.
- [ ] B) Pure uncompressed file contents only.
- [ ] C) Pure raw file contents (prefixed by header `blob <size>\0`), compressed with zlib.
- [ ] D) A JSON object mapping timestamps to diff chunks.

### Question 2: How does Git handle identical files across different branches or directories?
- [ ] A) It creates a new blob for each unique filepath.
- [ ] B) It stores one blob object and references its SHA across multiple tree objects.
- [ ] C) It creates hard filesystem links on disk.
- [ ] D) It calculates a cryptographic diff against the root commit.

### Question 3: What is the exact difference between `git diff` and `git diff --staged`?
- [ ] A) `git diff` shows remote changes; `git diff --staged` shows local changes.
- [ ] B) `git diff` compares Working Tree vs. Index; `git diff --staged` compares Index vs. HEAD commit.
- [ ] C) `git diff` compares Index vs. HEAD; `git diff --staged` compares Working Tree vs. Remote.
- [ ] D) They are identical aliases.

### Question 4: What is written inside `.git/HEAD` when you are in a "Detached HEAD" state?
- [ ] A) `ref: refs/heads/main`
- [ ] B) `null`
- [ ] C) A raw 40-character commit SHA hash.
- [ ] D) `detached: true`

---

## 🔍 Section 2: Plumbing & Diagnostic Scenarios

### Scenario A: The Disappearing Commit
A developer ran `git checkout HEAD~3`, made a critical hotfix commit `a1b2c3d`, and then ran `git checkout main`.
1. What state was the developer in when they made the commit?
2. Why does `git log` on `main` not show `a1b2c3d`?
3. What exact command attaches that commit to a new branch called `hotfix-restored`?

### Scenario B: Accidental Force Reset
You accidentally execute `git reset --hard HEAD~5`.
Write the exact 2-step diagnostic and recovery procedure to restore your branch to the exact state before the reset using `git reflog`.

---

## 🛠️ Section 3: Practical Verification Task

Run the following test in a sandbox terminal:

```bash
# 1. Create a test repo
mkdir test-eval && cd test-eval && git init

# 2. Hash a string into a blob
HASH=$(echo "Mastery Test String" | git hash-object -w --stdin)

# 3. Print type and content via plumbing
git cat-file -t $HASH
git cat-file -p $HASH
```

Confirm that output matches:
- Type: `blob`
- Content: `Mastery Test String`

---

## 🏆 Section 4: Level 1 Mastery Criteria Checklist

Sign off each requirement before moving to **Level 2: GitHub Repository Mastery**:

- [ ] **Three Trees**: Can explain Working Tree vs. Index vs. HEAD without ambiguity.
- [ ] **Object Types**: Can name and diagram the 4 Git object types (Blob, Tree, Commit, Tag).
- [ ] **Plumbing Fluency**: Can construct a commit purely via `hash-object`, `write-tree`, and `commit-tree`.
- [ ] **DAG Understanding**: Understands parent pointers, immutable SHA hashes, and references.
- [ ] **Detached HEAD**: Confidently navigates detached HEAD states and rescues dangling commits.
- [ ] **Reflog Mastery**: Can recover from destructive resets (`reset --hard`) in under 30 seconds.

---

## 🔑 Answers & Explanations

<details>
<summary>👉 Click to reveal assessment answers & explanations</summary>

### Section 1 Answers:
1. **C** — Blobs store pure file contents prefixed with `blob <size>\0` compressed with zlib. Filenames and permissions are stored in Tree objects.
2. **B** — Git deduplicates identical content globally; multiple trees point to the same single blob hash.
3. **B** — `git diff` shows unstaged working tree edits; `git diff --staged` shows staged index changes ready to commit.
4. **C** — In detached HEAD, `.git/HEAD` contains the direct commit hash rather than a symbolic `ref: refs/heads/...` string.

### Section 2 Explanations:
**Scenario A**:
1. Detached HEAD state.
2. The commit is dangling because no branch reference points to it.
3. `git branch hotfix-restored a1b2c3d` (or `git branch hotfix-restored HEAD@{1}`).

**Scenario B**:
1. Run `git reflog` to identify the commit before the reset (e.g. `HEAD@{1}`).
2. Run `git reset --hard HEAD@{1}`.
</details>
