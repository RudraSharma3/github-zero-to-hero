# Lab 1: Git Plumbing & Dangling Commit Recovery (Break/Fix)

---

## 1. Objective
Gain deep architectural mastery of Git by manually constructing a complete commit using only low-level plumbing commands (`hash-object`, `write-tree`, `commit-tree`, `update-ref`), and intentionally inducing and recovering from dangerous **Detached HEAD dangling commits** and **accidental `git reset --hard` catastrophes** using `git reflog` and `git fsck`.

---

## 2. Prerequisites
- Terminal environment with Git `2.34+` installed.
- Completed [Level 0](../00-foundations/) and [Level 1 Lessons](../01-git-fundamentals/).

---

## 3. Setup
Create an isolated scratch directory on your local machine:

```bash
mkdir -p ~/git-mastery-labs/lab-01
cd ~/git-mastery-labs/lab-01
```

---

## 4. Instructions

### Part A: Manual Plumbing Construction (Zero Porcelain)
We will build a valid Git repository containing a directory with two files and an initial commit without touching `git add` or `git commit`.

```bash
# 1. Initialize empty Git database
mkdir plumbing-sandbox && cd plumbing-sandbox
git init

# 2. Create raw Blobs directly in .git/objects/
BLOB1_SHA=$(printf "# Microservices Core\nVersion 1.0\n" | git hash-object -w --stdin)
BLOB2_SHA=$(printf "def health_check(): return {'status': 'healthy'}\n" | git hash-object -w --stdin)
echo "Blob 1: $BLOB1_SHA"
echo "Blob 2: $BLOB2_SHA"

# 3. Stage Blobs into the Index with explicit paths and file permissions
# 100644 = Regular non-executable file
git update-index --add --cacheinfo 100644 $BLOB1_SHA "README.md"
git update-index --add --cacheinfo 100644 $BLOB2_SHA "src/health.py"

# 4. Convert Index into an immutable Tree Object
TREE_SHA=$(git write-tree)
echo "Root Tree SHA: $TREE_SHA"

# 5. Create Commit Object pointing to Root Tree
COMMIT_SHA=$(git commit-tree $TREE_SHA -m "feat: initial commit created entirely with plumbing")
echo "Commit SHA: $COMMIT_SHA"

# 6. Point main branch reference to the new commit
git update-ref refs/heads/main $COMMIT_SHA

# 7. Check out the working tree files to disk
git checkout main
```

---

### Part B: Inspecting the Internal Database
```bash
# Inspect the Commit object
git cat-file -p $COMMIT_SHA

# Inspect the Tree object
git cat-file -p $TREE_SHA

# Inspect the raw Blob
git cat-file -p $BLOB1_SHA
```

---

## 5. Expected Result
When you run `git log` and `ls -la`:

```bash
git log --stat
ls -la
ls -la src/
```

**Expected Terminal Output**:
```text
commit <commit-sha> (HEAD -> main)
Author: Your Name <your-email>
Date:   ...

    feat: initial commit created entirely with plumbing

 README.md     | 2 ++
 src/health.py | 1 +
 2 files changed, 3 insertions(+)

- README.md
- src/health.py
```

---

## 6. Verification
Confirm database integrity with `git fsck`:

```bash
git fsck --full
```
**Expected Output**:
`Checking object directories: 100% (256/256), done.` (Zero dangling or corrupt objects).

---

## 7. Troubleshooting & Deliberate Failure Scenarios (BREAK & FIX)

### 🔴 Failure Scenario 1: The Detached HEAD Disappearing Commit
**The Break**:
```bash
# Check out initial commit directly to enter Detached HEAD
git checkout $COMMIT_SHA

# Make an experimental feature commit while detached
echo "def payment_processor(): pass" >> src/health.py
git commit -am "feat: detached payment processor"
DETACHED_COMMIT_SHA=$(git rev-parse HEAD)
echo "Detached Commit Hash: $DETACHED_COMMIT_SHA"

# Switch back to main branch (Leaving the commit behind!)
git checkout main
```
*Observation*: `git log` on `main` does **NOT** show `feat: detached payment processor`! The commit appears lost!

**The Fix (Reflog Rescue)**:
1. Query the reflog history:
   ```bash
   git reflog
   ```
2. Locate the orphaned entry: `HEAD@{1}: commit: feat: detached payment processor`.
3. Resurrect the commit into a new permanent feature branch:
   ```bash
   git branch feature-payment HEAD@{1}
   ```
4. Verify the rescued branch:
   ```bash
   git checkout feature-payment
   git log -1 --stat
   ```

---

### 🔴 Failure Scenario 2: Accidental `git reset --hard` Catastrophe
**The Break**:
```bash
# Make 2 consecutive commits on main
echo "audit log v1" >> README.md && git commit -am "feat: audit log"
echo "metric log v1" >> README.md && git commit -am "feat: metrics"

# Disastrous command: Hard reset destroys Working Tree, Index, and HEAD pointer
git reset --hard HEAD~2
```
*Observation*: The last 2 commits are completely gone from `git log` and `README.md`!

**The Fix**:
1. Run `git reflog` to identify the state before the reset:
   ```bash
   git reflog -n 5
   ```
2. You will see: `HEAD@{0}: reset: moving to HEAD~2` and `HEAD@{1}: commit: feat: metrics`.
3. Move `main` back to `HEAD@{1}` instantly:
   ```bash
   git reset --hard HEAD@{1}
   ```
4. Inspect `git log`—all commits and files are 100% restored.

---

### 🔴 Failure Scenario 3: Identifying Dangling Objects with `git fsck`
**The Break**: Delete a temporary branch without merging it:
```bash
git branch temp-scratch
git branch -D temp-scratch
```

**The Fix**:
Find all unreferenced objects across the repository:
```bash
git fsck --lost-found
```
Inspect any dangling commit found in `.git/lost-found/commit/` using `git cat-file -p <hash>`.

---

## 8. Cleanup
```bash
cd ~/git-mastery-labs/lab-01
rm -rf plumbing-sandbox
```

---

## 9. Extension Challenge
Use `git mktree` to create a nested subdirectory tree structure (`lib/utils/math.py`) entirely via plumbing without touching the filesystem.
