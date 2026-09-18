# Lab 3: Branching Workflows, Conflict Surgery & `git rerere` (Break/Fix)

---

## 1. Objective
Master real-world Git conflict resolution and history sculpting by resolving complex multi-file 3-way merge conflicts using `zdiff3`, recovering from broken interactive rebase states with `git rebase --abort` and `git reflog`, and automating repeating conflict resolution with `git rerere`.

---

## 2. Prerequisites
- Terminal environment with Git `2.34+` installed.
- Completed [Level 0](../00-foundations/), [Level 1](../01-git-fundamentals/), and [Level 2](../02-repositories/).

---

## 3. Setup
Create a dedicated scratch workspace:

```bash
mkdir -p ~/git-mastery-labs/lab-03
cd ~/git-mastery-labs/lab-03
```

---

## 4. Instructions

### Part A: Initialize Repository & Configure Global Superpowers
```bash
mkdir conflict-surgery-lab && cd conflict-surgery-lab
git init

# Enable zdiff3 (3-way conflict view with common ancestor base)
git config merge.conflictstyle zdiff3

# Enable git rerere (Reuse Recorded Resolution)
git config rerere.enabled true
git config rerere.autoupdate true

# Create initial project baseline
cat << 'EOF' > api.py
def get_user_profile(user_id):
    # Base user lookup
    query = f"SELECT * FROM users WHERE id = {user_id}"
    timeout = 10
    return {"user_id": user_id, "status": "active", "timeout": timeout}
EOF

cat << 'EOF' > config.json
{
  "environment": "production",
  "port": 8080,
  "max_retries": 3
}
EOF

git add api.py config.json
git commit -m "c1: base api and configuration"
```

---

### Part B: Create Divergent Feature Branch
```bash
git checkout -b feat/security-hardening

# 1. Update api.py with SQL injection fix on feature branch
cat << 'EOF' > api.py
def get_user_profile(user_id):
    # Parametrized query to prevent SQL injection
    query = "SELECT * FROM users WHERE id = :user_id"
    timeout = 15  # Extended for security auth check
    return {"user_id": user_id, "status": "active", "timeout": timeout}
EOF

# 2. Update config.json on feature branch
cat << 'EOF' > config.json
{
  "environment": "production",
  "port": 8080,
  "max_retries": 5,
  "strict_ssl": true
}
EOF

git commit -am "feat: add sql parametrization and strict ssl"
```

---

### Part C: Create Conflicting Commits on `main`
```bash
git checkout main

# 1. Update api.py with caching optimization on main
cat << 'EOF' > api.py
def get_user_profile(user_id):
    # Cached user lookup
    query = f"SELECT * FROM users WHERE id = {user_id}"
    timeout = 5   # Reduced timeout for cache performance
    return {"user_id": user_id, "status": "active", "cached": True, "timeout": timeout}
EOF

# 2. Update config.json on main
cat << 'EOF' > config.json
{
  "environment": "production",
  "port": 9090,
  "max_retries": 3,
  "cache_enabled": true
}
EOF

git commit -am "feat: add caching layer and change default port"
```

---

## 5. Expected Result
Both branches have divergent, conflicting changes to both `api.py` and `config.json`.

---

## 6. Verification
Inspect the divergent graph:
```bash
git log --graph --oneline --all
```

---

## 7. Troubleshooting & Deliberate Failure Scenarios (BREAK & FIX)

### 🔴 Failure Scenario 1: Multi-File Merge Conflict Surgery with `zdiff3`
**The Break**: Attempt to merge `feat/security-hardening` into `main`:
```bash
git merge feat/security-hardening
```
*Output*:
```text
Auto-merging api.py
CONFLICT (content): Merge conflict in api.py
Auto-merging config.json
CONFLICT (content): Merge conflict in config.json
Automatic merge failed; fix conflicts and then commit the result.
Recorded preimage for 'api.py'
Recorded preimage for 'config.json'
```

**The Fix (Surgical Resolution)**:
1. Inspect `api.py` with `zdiff3`:
   ```bash
   cat api.py
   ```
   *Observe*:
   - `HEAD`: Added `cached: True` and set `timeout = 5`.
   - `base`: Original unparameterized query and `timeout = 10`.
   - `feat/security-hardening`: Added SQL parametrization and `timeout = 15`.
2. Surgically combine the best of both (Parameterized query + caching + safety timeout of 10):
   ```bash
   cat << 'EOF' > api.py
   def get_user_profile(user_id):
       # Parametrized query (security) + Caching layer
       query = "SELECT * FROM users WHERE id = :user_id"
       timeout = 10
       return {"user_id": user_id, "status": "active", "cached": True, "timeout": timeout}
   EOF
   ```
3. Surgically resolve `config.json`:
   ```bash
   cat << 'EOF' > config.json
   {
     "environment": "production",
     "port": 9090,
     "max_retries": 5,
     "strict_ssl": true,
     "cache_enabled": true
   }
   EOF
   ```
4. Stage resolved files and complete the merge:
   ```bash
   git add api.py config.json
   git commit -m "merge: resolve api security and config port conflicts"
   ```
   *Notice*: Git outputs `Recorded resolution for 'api.py'` and `Recorded resolution for 'config.json'`.

---

### 🔴 Failure Scenario 2: Stuck Mid-Rebase Conflict Loop & Safe Abort
**The Break**: Simulate an engineer who tries rebasing against an older branch, gets lost in conflicts, and wants to escape safely:
```bash
# Create experimental branch with 2 commits
git checkout -b exp-feature HEAD~2
echo "exp 1" >> api.py && git commit -am "exp: 1"
echo "exp 2" >> api.py && git commit -am "exp: 2"

# Start rebase on main (Triggers conflicts)
git rebase main
```
*Problem*: Terminal shows `(main|REBASE 1/2)`. The developer feels overwhelmed and needs to abort with 100% safety.

**The Fix (Clean Rebase Abort)**:
```bash
# Return to clean pre-rebase state instantly
git rebase --abort

# Verify working tree is clean
git status
```

---

### 🔴 Failure Scenario 3: Automated Conflict Replay with `git rerere`
**The Break**: You need to merge `feat/security-hardening` into a testing branch, and later rebase it.
Because `git rerere` is enabled, watch Git auto-resolve the conflict!

```bash
# Create a staging branch from original base
git checkout -b staging HEAD~2

# Merge the exact same feature branch again
git merge feat/security-hardening
```
*Output*:
```text
Auto-merging api.py
CONFLICT (content): Merge conflict in api.py
Auto-merging config.json
CONFLICT (content): Merge conflict in config.json
Resolved 'api.py' using previous resolution.
Resolved 'config.json' using previous resolution.
```
*Magic*: Git used your previous resolution automatically! Running `git diff` shows the files are already resolved!

---

## 8. Cleanup
```bash
cd ~/git-mastery-labs/lab-03
rm -rf conflict-surgery-lab
```

---

## 9. Extension Challenge
Inspect the hidden `.git/rr-cache/` directory to see how Git stores conflict SHA fingerprints and resolution diffs on disk.
