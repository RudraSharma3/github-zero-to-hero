# Lesson 01: The Three-Tree Architecture & Git Lifecycles

---

## 1. Learning Objective
Understand and manipulate Git's foundational **Three-Tree Architecture** (Working Tree, Index / Staging Area, and `HEAD` commit history), the four-stage file lifecycle, and atomic staging techniques (`git add -p`).

---

## 2. Why This Matters
Most Git confusion (e.g. "Where did my changes go?", "Why did this commit include debug code?", "Why does `git diff` show nothing when I modified files?") stems from not understanding that Git maintains three concurrent versions of your project. Mastering the Three Trees gives developers precise control over atomic, production-ready commits.

---

## 3. Prerequisites
- Completed [Level 0: Foundation & GitHub Mental Model](../00-foundations/).
- Terminal environment with Git `2.34+` installed.

---

## 4. Concept Explanation

### The Three Trees (The Tripartite Architecture)
Git does not track delta changes between files across time. Instead, it tracks transitions across three structural trees:

1. **The Working Tree (Filesystem)**:
   - The actual directory on your hard drive where you view and edit code with your IDE.
   - Files here can be *Untracked*, *Unmodified*, *Modified*, or *Deleted*.
2. **The Index / Staging Area (`.git/index`)**:
   - A high-performance binary file containing a ordered list of file paths, file permissions (modes), and corresponding blob SHA hashes.
   - It acts as the "staging buffer" or preview for the next snapshot.
3. **The HEAD Commit (Permanent History)**:
   - A pointer to the latest commit object on the currently checked-out branch in `.git/objects/`.
   - Represents the verified, immutable past.

### The File Lifecycle in Git
```
   [Untracked] ────( git add )────> [Staged]
                                       │
   [Modified]  ────( git add )────>    │
        ▲                              │ ( git commit )
        │                              ▼
   [Unmodified] <──────────────── [Committed]
```

### Differentiating `git diff` Commands
- `git diff`: Compares **Working Tree** vs. **Index** (shows unstaged edits).
- `git diff --staged` (or `--cached`): Compares **Index** vs. **HEAD** (shows what will go into the next commit).
- `git diff HEAD`: Compares **Working Tree** vs. **HEAD** (shows all changes since the last commit, regardless of staging).

---

## 5. Mental Model & Architecture Diagrams

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Developer
    participant WT as Working Tree (Disk Files)
    participant Index as Index / Staging (.git/index)
    participant HEAD as HEAD Commit (.git/objects)

    Dev->>WT: 1. Edit code (e.g., app.py)
    Note over WT: Status: Modified (Unstaged)
    Dev->>Index: 2. git add app.py
    Note over Index: Blob written to .git/objects, Index updated
    Note over Index: Status: Staged for Commit
    Dev->>HEAD: 3. git commit -m "feat: update app"
    Note over HEAD: Tree & Commit object created; HEAD pointer moves
    Note over WT,HEAD: All three trees are now identical
```

---

## 6. Real-World Use Case
You are fixing a high-severity production bug in `auth.ts`, but during debugging you added temporary `console.log()` statements and modified `config.ts` for local testing. Using atomic staging (`git add -p auth.ts`), you stage ONLY the surgical bug fix into the **Index**, leaving debug logs in your **Working Tree**. You commit the clean fix to **HEAD**, verify it, and discard the local debug debris.

---

## 7. Official GitHub Documentation
- [GitHub Docs: Git Basics - Recording Changes to the Repository](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)
- [Pro Git Book: Reset Demystified (The Three Trees)](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified)

---

## 8. Recommended High-Quality External Resource
- **Resource**: *Reset Demystified* by Scott Chacon.
- **Why it helps**: The definitive explanation of how `checkout`, `reset`, and `restore` interact with the Working Tree, Index, and HEAD.

---

## 9. Step-by-Step Demonstration

### 1. Initialize a Sandbox Repository
```bash
mkdir three-trees-demo && cd three-trees-demo
git init
```

### 2. Observe the Transitions Across the Three Trees
```bash
# Step 1: Create file in Working Tree
echo "print('v1.0')" > app.py
git status
# Output: Untracked files: app.py

# Step 2: Move snapshot into the Index
git add app.py
git status
# Output: Changes to be committed: new file: app.py

# Step 3: Write Index into permanent HEAD commit
git commit -m "feat: initial app release"
# Output: [main (root-commit)] feat: initial app release
```

### 3. Inspect the Diff Boundaries
```bash
# Modify app.py in Working Tree
echo "print('v1.1')" >> app.py

# Inspect Working Tree vs Index
git diff
# Shows '+print('v1.1')'

# Stage the change
git add app.py

# Now git diff shows nothing!
git diff

# But git diff --staged shows the staged change
git diff --staged
```

### 4. Atomic Hunk Staging with `git add -p`
```bash
# Add multiple distinct lines
echo "# Configuration settings" >> app.py
echo "DEBUG = True" >> app.py
echo "def run(): pass" >> app.py

# Interactively choose which hunks to stage
git add -p app.py
# (Type 'y' to stage, 'n' to skip, 's' to split hunks)
```

---

## 10. Hands-on Lab
Refer to [`../labs/01-git-plumbing-and-recovery-lab.md`](../labs/01-git-plumbing-and-recovery-lab.md) for practical exercises.

---

## 11. Challenge
**Challenge**: You have modified 3 files in your working tree (`a.txt`, `b.txt`, `c.txt`). You accidentally ran `git add .` staging all three. Without losing any of your written code in the working tree, how do you unstage *only* `b.txt` and `c.txt` so that only `a.txt` is committed?
*(Answer: `git restore --staged b.txt c.txt` or `git reset HEAD b.txt c.txt`)*.

---

## 12. Common Mistakes & Gotchas
- **Mistake 1**: Running `git commit -a` (or `git commit -am`) by habit. (*Why it's bad: Bypasses the Index review stage and accidentally commits untracked files or debug modifications.*)
- **Mistake 2**: Running `git checkout -- <file>` or `git restore <file>` thinking it unstages changes. (*Danger: It discards Working Tree changes permanently.*)
- **Mistake 3**: Assuming `git status` inspects remote branches. (*Reality: `git status` only compares the local 3 trees plus local tracking branches.*)

---

## 13. Professional Practices
- **Atomic Commits**: Every commit should represent a single logical, reversible change that passes tests independently.
- **Review with `git diff --staged` Before Every Commit**: Make it an unbreakable muscle memory habit to run `git diff --staged` before executing `git commit`.
- **Use `git restore` (Git 2.23+)**: Use modern commands (`git restore --staged <file>` for the Index, `git restore <file>` for the Working Tree) instead of overloaded `git reset` / `git checkout`.

---

## 14. Security Considerations
- **Staging Sensitive Environment Files**: Never run `git add .` without checking `git status` or configuring `.gitignore`. Staging an accidental `.env` file writes the blob into `.git/objects/` immediately upon `git add`!

---

## 15. Interview Questions & Progression

1. **[WHAT]**: What are the "Three Trees" in Git?
   - *Answer*: Working Tree (local files), Index/Staging Area (`.git/index`), and HEAD (latest commit object in repository history).
2. **[HOW]**: What is the exact difference between `git diff` and `git diff --staged`?
   - *Answer*: `git diff` displays unstaged modifications between Working Tree and Index; `git diff --staged` displays staged modifications between Index and HEAD.
3. **[WHY]**: Why does Git have a Staging Area (Index) when other VCS like Mercurial or SVN commit directly from the working tree?
   - *Answer*: The Index allows developers to craft granular, atomic commits independently of what files happen to be currently open and modified on disk.
4. **[WHAT IF]**: What happens to files in `.git/objects/` if you stage a file with `git add` and then immediately unstage it?
   - *Answer*: The blob object remains in `.git/objects/` as a loose object until garbage collected by `git gc`, even though the Index reference was removed.
5. **[TRADE-OFFS]**: What are the trade-offs of using `git commit -am` vs explicit `git add`?
   - *Answer*: `-am` saves a few seconds of typing at the severe risk of bundling unintended changes and bypassing code review hygiene.

---

## 16. Real-World Scenario
An engineer working on an enterprise payment gateway accidentally introduces their private sandbox API key into `settings.py` while fixing a payment timeout bug in `gateway.py`. Because they used `git add -p` and reviewed `git diff --staged`, they caught the accidental key staging before creating the commit or pushing to GitHub.

---

## 17. Assessment
Complete the evaluation in [`../assessments/01-git-fundamentals-assessment.md`](../assessments/01-git-fundamentals-assessment.md).

---

## 18. Mastery Criteria
- [ ] Can articulate the role of the Working Tree, Index, and HEAD without hesitation.
- [ ] Fluently navigates `git diff`, `git diff --staged`, and `git diff HEAD`.
- [ ] Confidently uses `git add -p` for atomic patch staging.
- [ ] Uses `git restore` and `git restore --staged` safely without data loss.

---

## 19. Further Exploration
- Inspect the binary index file format using `git ls-files --stage --debug`.
- Learn how `git write-tree` converts the Index directly into a tree object.
