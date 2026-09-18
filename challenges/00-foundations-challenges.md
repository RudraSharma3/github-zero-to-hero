# Level 0 Challenges: Foundations & Environment Engineering

---

## 🎯 Challenge System Overview
The challenges in this repository progress across 5 difficulty tiers. Do not skip straight to hints or answers—attempt each scenario independently to develop genuine operational muscle memory.

---

### 🟢 Tier 1: Basic Usage — Configuration & Identity Audit
**Objective**: Perform a comprehensive audit of your active Git and SSH environment.
1. Run a single command to list all resolved Git configuration keys, values, and the exact configuration file paths where each key is defined.
2. Check the algorithm and fingerprint of all currently loaded keys in your running `ssh-agent`.
3. Confirm that `init.defaultBranch` is set to `main` and `gpg.format` is set to `ssh`.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Explore <code>git config --list --show-origin</code> and <code>ssh-add -l -E sha256</code>.
> </details>

---

### 🟡 Tier 2: Real Project Scenario — Multi-Account Profile Isolation (`includeIf`)
**Objective**: Configure your workstation to handle two separate identities seamlessly without manual switching:
- Repositories cloned inside `~/work/` must automatically use author `work-user@company.com` and SSH key `~/.ssh/id_ed25519_work`.
- Repositories cloned inside `~/personal/` must automatically use author `personal-user@gmail.com` and SSH key `~/.ssh/id_ed25519_personal`.

**Requirements**:
1. Use Git's `includeIf "gitdir:~/work/"` directive inside `~/.gitconfig`.
2. Ensure commits made in `~/work/project-a` automatically sign with the work key.
3. Verify that creating a repo in `~/personal/project-b` defaults to the personal identity.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> Create <code>~/.gitconfig-work</code> and reference it using <code>[includeIf "gitdir:~/work/"] path = ~/.gitconfig-work</code> in your root <code>~/.gitconfig</code>.
> </details>

---

### 🟠 Tier 3: Failure / Debugging Scenario — The Locked Out Engineer
**Objective**: Diagnose and repair a broken SSH setup under realistic failure constraints.

**Scenario**:
A teammate runs `git push origin main` and receives:
```text
sign_and_send_pubkey: signing failed for ED25519 "/home/dev/.ssh/id_ed25519": agent refused operation
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Task**:
1. Explain the three potential root causes of `agent refused operation`.
2. Write the diagnostic commands to inspect the SSH agent socket environment variable (`SSH_AUTH_SOCK`).
3. Formulate the exact recovery sequence without regenerating or deleting the existing SSH keypair.

> [!NOTE]
> <details>
> <summary>💡 Hint</summary>
> This error often occurs when the SSH agent process is orphaned, permissions on <code>~/.ssh</code> are too open (e.g. 777), or the key passphrase was not cached. Check <code>eval $(ssh-agent -s) && ssh-add</code> and file permissions.
> </details>

---

### 🔴 Tier 4: Professional / Enterprise Scenario — Corporate Proxy & Port 443 Fallback
**Objective**: Configure SSH to route over HTTPS port 443 in environments where standard outbound port 22 (SSH) is blocked by corporate firewalls.

**Requirements**:
1. Update `~/.ssh/config` to connect to GitHub's SSH service via `ssh.github.com` on port `443`.
2. Test and verify connection over port 443 using `ssh -T -p 443 git@ssh.github.com`.
3. Configure Git to automatically use this alternate port for all `git@github.com:...` remote URLs.

---

### 🟣 Tier 5: Independent Implementation — The "Git Doctor" Script
**Objective**: Build an automated diagnostic script (`git-doctor.sh` or `git-doctor.ps1`) that inspects a developer's workstation and outputs a green/yellow/red compliance report.

**Script Must Check**:
1. Git CLI installed and version $\ge 2.34$.
2. `user.name` and `user.email` are non-empty and formatted correctly.
3. `init.defaultBranch` is explicitly set to `main`.
4. SSH Ed25519 key exists in `~/.ssh/` with strict permissions (`600`/`700`).
5. SSH agent is actively running with at least one loaded key.
6. Successful handshake with `ssh -T git@github.com`.
7. SSH commit signing enabled and verified with a temporary test commit.
