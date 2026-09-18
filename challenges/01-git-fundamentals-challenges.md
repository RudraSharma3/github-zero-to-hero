# Level 1 Challenges: Git Fundamentals & Internal Architecture

---

## 🎯 Challenge System Overview
Attempt each tier independently before consulting hints. These challenges build genuine low-level competence across Git's object model and emergency recovery techniques.

---

### 🟢 Tier 1: Basic Usage — Object Hash & Type Verification
**Objective**:
1. Without creating a file on disk, generate a blob object for the text `"Antigravity Git Engine"` and capture its SHA-1 hash.
2. Using only `git cat-file`, print the **size in bytes**, the **object type**, and the **decompressed contents** of that hash.
3. Write a shell one-liner using `shasum` (or `openssl sha1`) that independently hashes `"blob 22\0Antigravity Git Engine"` to prove it yields the exact same 40-character hex string.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Use <code>git hash-object --stdin</code> and <code>printf "blob 22\0Antigravity Git Engine" | shasum</code>.
> </details>

---

### 🟡 Tier 2: Real Project Scenario — Manual Nested Tree Construction
**Objective**:
Using only `git hash-object`, `git update-index`, and `git write-tree`, construct a multi-level directory hierarchy in `.git/objects/`:
```text
config/
  ├── database.yml
  └── redis.yml
src/
  └── server.py
README.md
```
**Requirements**:
1. Create individual blobs for each file.
2. Build the nested tree objects (`config/`, `src/`, and the root tree).
3. Generate a commit with parent `HEAD` pointing to this newly created root tree.
4. Verify the tree structure with `git ls-tree -r <commit-hash>`.

---

### 🟠 Tier 3: Failure / Debugging Scenario — The Force-Pushed Disaster
**Scenario**:
A junior developer was rebasing feature branch `payment-v2` against `main`. Confused by merge conflicts, they ran `git rebase --abort`, followed by an accidental `git reset --hard origin/main`, wiping out 6 commits (3 days of work).

**Task**:
1. Formulate the exact diagnostic commands using `git reflog` to trace the history before the rebase and reset.
2. Identify the commit SHA where `payment-v2` stood before the mistake.
3. Write the single command to recreate `payment-v2` at that exact historical commit without modifying `main`.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Search reflog for <code>payment-v2@{...}</code> or inspect <code>git reflog show payment-v2</code>. Then run <code>git branch -f payment-v2 <target-sha></code>.
> </details>

---

### 🔴 Tier 4: Professional / Enterprise Scenario — Large Binary Packfile Audit
**Objective**:
A developer accidentally committed a 150MB video file (`demo.mp4`), deleted it in the next commit with `git rm demo.mp4`, and pushed. The repository clone size remains over 150MB because the blob object is retained in the history.

**Requirements**:
1. Run `git verify-pack -v .git/objects/pack/*.idx` to sort and identify the top 5 largest objects in the repository by byte size.
2. Map the largest blob SHA hash back to its original file path in Git history using `git rev-list --objects --all`.
3. Explain why `git rm` in a subsequent commit fails to reduce repository clone size.

---

### 🟣 Tier 5: Independent Implementation — The "Mini-Git" Object Store
**Objective**:
Write a standalone script (`minigit.py` or `minigit.sh`) that implements a basic clone of `git hash-object -w` and `git cat-file -p` from scratch using only standard libraries (`hashlib` / `shasum` and `zlib`).

**Specification**:
1. When passed a file path:
   - Reads the raw bytes.
   - Calculates length $L$.
   - Constructs header: `blob {L}\0`.
   - Computes SHA-1 hash.
   - Compresses `(header + content)` with `zlib`.
   - Writes compressed file to `.minigit/objects/{sha[:2]}/{sha[2:]}`.
2. When passed `-p <sha>`:
   - Reads the compressed object from `.minigit/objects/`.
   - Decompresses with `zlib`.
   - Strips the `blob {L}\0` header and prints the pure file contents to stdout.
