# Level 0 Assessment: Foundation & GitHub Mental Model

---

## 🎯 Assessment Overview
This assessment evaluates your conceptual clarity, operational diagnostics, and security awareness for Git and GitHub fundamentals.

**Passing Score**: 100% on Safety & Verification items, $\ge 85\%$ overall.

---

## 📝 Section 1: Conceptual & Architectural Mastery

### Question 1: Git Object Storage vs Remote Platform
Which statement accurately describes the relationship between local Git and GitHub?
- [ ] A) Git commits are queued in memory until synced with GitHub over HTTPS.
- [ ] B) Git is an offline content-addressable object database; GitHub is a cloud hosting, automation, and governance platform.
- [ ] C) Deleting a local `.git` folder automatically deletes the corresponding remote repository on GitHub.
- [ ] D) Git commands require an active internet connection to verify user credentials against GitHub IAM.

### Question 2: Commit Author vs Authentication Key
An attacker configures their local machine with `git config user.email "ceo@enterprise.com"` and pushes to a public repo using their personal SSH key. What happens on GitHub?
- [ ] A) GitHub blocks the push because the commit email doesn't match the SSH key owner.
- [ ] B) GitHub displays the commit as authored by `ceo@enterprise.com`, but marks it as **Unverified** unless signed with a trusted cryptographic key.
- [ ] C) The commit is automatically deleted by GitHub Secret Scanning.
- [ ] D) GitHub assigns admin rights to the pusher because of the email domain.

### Question 3: Configuration Precedence Cascade
If `user.name` is configured as `"Global Dev"` in `~/.gitconfig` and `"Local Dev"` in `.git/config`, what name will appear on new commits made inside that repository?
- [ ] A) `"Global Dev"`
- [ ] B) `"Local Dev"`
- [ ] C) `"Global Dev / Local Dev"`
- [ ] D) Git raises a fatal ambiguity error.

---

## 🔍 Section 2: Diagnostic & Command Analysis

### Scenario A: The Broken Push
You run `git push origin main` and get:
```text
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```
List the exact 3 troubleshooting steps you execute in sequence to isolate whether the issue is the SSH Agent, the local key file, or the GitHub account configuration.

### Scenario B: Unsigned Commits Failing Ruleset
Your organization has enabled **Require signed commits** on branch `main`. Your pull request is blocked because your last 3 commits are unsigned.
Write the Git commands to re-sign the last 3 commits without altering code changes.

---

## 🛠️ Section 3: Practical Verification Task

Execute the following live check in your terminal:

```bash
# 1. Test SSH Authentication against GitHub
ssh -T git@github.com

# 2. Check SSH Commit Signing configuration
git config --get gpg.format
git config --get commit.gpgsign
git config --get user.signingkey
```

Attach or verify that output matches:
- `ssh -T`: `Hi <username>! You've successfully authenticated...`
- `gpg.format`: `ssh`
- `commit.gpgsign`: `true`
- `user.signingkey`: Valid path to `~/.ssh/<key>.pub`

---

## 🏆 Section 4: Level 0 Mastery Criteria Checklist

Review and sign off each requirement before moving to **Level 1: Git Fundamentals**:

- [ ] **Mental Model**: Can distinguish local `.git` engine from remote GitHub platform without hesitation.
- [ ] **Environment Defaults**: Configured `init.defaultBranch = main`, `pull.rebase = false`, and appropriate `core.autocrlf`.
- [ ] **Cryptographic Security**: Generated an Ed25519 SSH key with passphrase and registered it with GitHub.
- [ ] **Commit Provenance**: Configured SSH commit signing and verified local commits show "Good signature".
- [ ] **Troubleshooting**: Successfully completed the Break/Fix scenarios in Lab 0 (`Permission denied` and author mismatch).

---

## 🔑 Answers & Explanations

<details>
<summary>👉 Click to reveal assessment answers & explanations</summary>

### Section 1 Answers:
1. **B** — Git is a fully offline content-addressable database. GitHub is the cloud platform.
2. **B** — Commit author metadata is unauthenticated plain text. Without cryptographic commit signing, anyone can write any email address in `user.email`. GitHub awards the "Verified" badge only when the commit signature matches a verified GPG/SSH key on the user's account.
3. **B** — Local repository configuration (`.git/config`) always overrides global configuration (`~/.gitconfig`).

### Section 2 Explanations:
**Scenario A Resolution**:
1. Check if the agent is running and has the key: `ssh-add -l`.
2. Test connection in verbose mode: `ssh -Tv git@github.com`.
3. Verify public key matches what is uploaded to GitHub: `cat ~/.ssh/id_ed25519.pub` vs GitHub Settings.

**Scenario B Resolution**:
Run interactive rebase to re-sign the last 3 commits:
```bash
git rebase --exec 'git commit --amend --no-edit -S' HEAD~3
```
</details>
