# Lab 0: Developer Environment & SSH Authentication (Break/Fix)

---

## 1. Objective
Establish a production-ready developer environment by configuring global Git defaults, generating and registering an Ed25519 SSH keypair, configuring SSH commit signing, and diagnosing/recovering from deliberate authentication failures (`Permission denied (publickey)`) and commit author mismatches.

---

## 2. Prerequisites
- Terminal shell (Bash, Zsh, or PowerShell).
- Git `2.34+` installed (`git --version`).
- A GitHub account with a verified email address and 2FA enabled.

---

## 3. Setup
Create a dedicated scratch directory on your machine to test the configuration safely:

```bash
mkdir -p ~/git-mastery-labs/lab-00
cd ~/git-mastery-labs/lab-00
```

---

## 4. Instructions

### Part A: Configure Professional Global Defaults
Execute the following commands to establish clean baseline settings:

```bash
# 1. Set your full name and verified GitHub email
git config --global user.name "Your Name"
git config --global user.email "your-verified-email@example.com"

# 2. Modern default branch naming
git config --global init.defaultBranch main

# 3. Predictable pull behavior (fast-forward or explicit merge)
git config --global pull.rebase false

# 4. Configure line endings
# On Linux/macOS:
git config --global core.autocrlf input
# On Windows:
# git config --global core.autocrlf true

# 5. Display the full active configuration hierarchy
git config --list --show-origin
```

---

### Part B: Generate and Provision Ed25519 SSH Key
```bash
# 1. Generate Ed25519 keypair with a strong passphrase
ssh-keygen -t ed25519 -C "your-verified-email@example.com" -f ~/.ssh/id_ed25519_lab

# 2. Ensure correct file permissions (Unix/macOS/WSL)
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519_lab
chmod 644 ~/.ssh/id_ed25519_lab.pub

# 3. Start SSH Agent and register key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_lab

# 4. View public key and add to GitHub Profile -> Settings -> SSH and GPG Keys
cat ~/.ssh/id_ed25519_lab.pub
```

> [!TIP]
> If you have GitHub CLI (`gh`) installed and authenticated, you can upload the key directly with:
> `gh ssh-key add ~/.ssh/id_ed25519_lab.pub --title "Lab-00 Key"`

---

### Part C: Configure SSH Commit Signing
```bash
# 1. Configure Git to use SSH format for signing
git config --global gpg.format ssh

# 2. Set the public key as the signing key
git config --global user.signingkey ~/.ssh/id_ed25519_lab.pub

# 3. Enable auto-signing
git config --global commit.gpgsign true
```

---

## 5. Expected Result
When you test connectivity to GitHub:

```bash
ssh -i ~/.ssh/id_ed25519_lab -T git@github.com
```

**Expected Terminal Output**:
```text
Hi <your-username>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 6. Verification
Create a test sandbox repository and verify that your commits are cryptographically signed:

```bash
cd ~/git-mastery-labs/lab-00
git init sandbox-repo
cd sandbox-repo
echo "# Verification Test" > README.md
git add README.md
git commit -m "chore: test cryptographic commit signing"

# Verify signature
git log --show-signature -1
```

**Expected Output**:
```text
commit abc1234... (HEAD -> main)
Good "ssh" signature for <your-verified-email@example.com> with ED25519 key SHA256:...
Author: Your Name <your-verified-email@example.com>
...
```

---

## 7. Troubleshooting & Deliberate Failure Scenarios (BREAK & FIX)

### 🔴 Failure Scenario 1: `Permission denied (publickey)`
**The Break**: Simulate an unregistered key or misconfigured SSH host.
```bash
# Attempt connecting using a non-existent or un-uploaded key
ssh -i /dev/null -T git@github.com
```
*Output*: `git@github.com: Permission denied (publickey).`

**The Fix (Root Cause Analysis)**:
1. Run SSH in verbose debug mode to inspect key negotiation:
   ```bash
   ssh -Tv git@github.com
   ```
2. Look for the `debug1: Authentications that can continue: publickey` and `debug1: Trying private key` lines in the output.
3. Check if your SSH agent has the key loaded:
   ```bash
   ssh-add -l
   ```
4. If missing, re-add the key:
   ```bash
   ssh-add ~/.ssh/id_ed25519_lab
   ```
5. Configure `~/.ssh/config` to always use your key for GitHub:
   ```text
   Host github.com
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_ed25519_lab
       IdentitiesOnly yes
   ```

---

### 🔴 Failure Scenario 2: Commit Author Metadata Mismatch (Grey Avatar)
**The Break**: Simulate committing with an unverified email address:
```bash
git config user.email "fake_developer@unverified-domain-xyz.com"
echo "unverified change" >> README.md
git commit -am "fix: make unverified commit"
git log -1 --pretty=format:"Author: %an <%ae>"
```

**The Fix**:
1. Correct the email back to your verified GitHub email:
   ```bash
   git config --global user.email "your-verified-email@example.com"
   ```
2. Rewrite the last commit author identity cleanly:
   ```bash
   git commit --amend --reset-author --no-edit
   ```
3. Verify the updated author info:
   ```bash
   git log -1 --pretty=format:"Author: %an <%ae>"
   ```

---

## 8. Cleanup
Once verification is complete, clean up the local test repo:

```bash
cd ~/git-mastery-labs/lab-00
rm -rf sandbox-repo
```

*(Note: Keep your generated SSH key in `~/.ssh/` for subsequent levels).*

---

## 9. Extension Challenge
Configure **`~/.ssh/config`** with two distinct host aliases (`Host github.com` and `Host github-personal`) pointing to two different Ed25519 keys, and test authenticating both against GitHub.
