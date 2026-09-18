# Level 2 Assessment: GitHub Repository Mastery & Hygiene

---

## 🎯 Assessment Overview
This assessment evaluates your ability to architect clean repositories, eliminate cross-platform line ending corruption, master `.gitignore` mechanics, configure `CODEOWNERS`, and author structured issue forms.

**Passing Score**: 100% on Safety & Normalization items, $\ge 85\%$ overall.

---

## 📝 Section 1: Conceptual & Governance Mastery

### Question 1: `.gitignore` and Tracked Files
A developer commits `config/database.yml` to the repository. Later, they add `config/database.yml` to `.gitignore`. What happens when they edit `config/database.yml`?
- [ ] A) Git ignores the edits automatically.
- [ ] B) Git raises a merge conflict on the next push.
- [ ] C) Git continues tracking modifications to the file because it is already indexed in history.
- [ ] D) Git deletes the local file from disk.

### Question 2: `CODEOWNERS` Rule Evaluation Order
Given the following `.github/CODEOWNERS` file:
```text
/src/billing/ @security-team
* @general-devs
```
Who will be assigned as code reviewer when a PR modifies `/src/billing/invoice.ts`?
- [ ] A) `@security-team` only.
- [ ] B) `@general-devs` only (because last matching rule wins).
- [ ] C) Both `@security-team` and `@general-devs`.
- [ ] D) No one (syntax error).

### Question 3: Line Ending Normalization
Why does setting `* text=auto` in `.gitattributes` prevent cross-platform merge conflicts?
- [ ] A) It forces Windows developers to install Linux.
- [ ] B) It converts all text files to LF in `.git/objects/` while allowing host OS native checkouts.
- [ ] C) It compresses text files with Gzip before pushing.
- [ ] D) It converts tabs to spaces automatically.

---

## 🔍 Section 2: Diagnostics & Scenario Analysis

### Scenario A: The Phantom Monorepo Diff
You open a PR with 1 line of code changed in `server.py`. However, GitHub shows:
`+1,200 lines / -1,200 lines` across 15 files because your IDE converted `LF` to `CRLF`.
1. What command in `.gitattributes` enforces LF line endings on all Python files?
2. What single Git command renormalizes the entire repository index without manual file-by-file editing?

### Scenario B: Untracking Without Data Loss
You accidentally committed 50MB of compiled binaries inside `/build/`. You want to stop Git from tracking `/build/`, add it to `.gitignore`, but keep the compiled files intact on your local drive for testing.
Write the exact 2 Git commands to accomplish this.

---

## 🛠️ Section 3: Practical Verification Task

Run the following test in a sandbox terminal:

```bash
# 1. Create a test repo
mkdir test-repo-eval && cd test-repo-eval && git init

# 2. Author .gitattributes with normalization
echo "* text=auto" > .gitattributes
echo "*.sh text eol=lf" >> .gitattributes

# 3. Verify attributes engine
git check-attr -a .gitattributes
git check-attr text eol deploy.sh
```

Confirm that output matches:
- `.gitattributes: text: auto`
- `deploy.sh: text: set`
- `deploy.sh: eol: lf`

---

## 🏆 Section 4: Level 2 Mastery Criteria Checklist

Sign off each requirement before moving to **Level 3: Branching & Professional Git Workflows**:

- [ ] **Community Standards**: Knows the purpose and placement of all 5 standard community health files.
- [ ] **Ignore Mechanics**: Can write, debug (`check-ignore -v`), and untrack (`rm --cached`) files cleanly.
- [ ] **Line Endings**: Mastered `.gitattributes` and repository renormalization (`git add --renormalize .`).
- [ ] **CODEOWNERS**: Can design multi-team ownership files respecting last-match precedence.
- [ ] **Issue Forms**: Can write valid YAML issue forms with field validations and auto-labeling.

---

## 🔑 Answers & Explanations

<details>
<summary>👉 Click to reveal assessment answers & explanations</summary>

### Section 1 Answers:
1. **C** — `.gitignore` only applies to untracked files. Tracked files remain tracked until removed with `git rm --cached`.
2. **B** — In `CODEOWNERS`, the last matching rule wins. `* @general-devs` at the bottom overrides `/src/billing/`.
3. **B** — `* text=auto` standardizes storage to LF while serving native endings on checkouts.

### Section 2 Explanations:
**Scenario A**:
1. `*.py text eol=lf` in `.gitattributes`.
2. `git add --renormalize .`

**Scenario B**:
```bash
echo "/build/" >> .gitignore
git rm -r --cached build/
git commit -m "chore: untrack build directory"
```
</details>
