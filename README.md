# Linux-User-System-Administration-Lab

# Linux User & System Administration Lab

**Environment:** Ubuntu / Kali (VirtualBox home lab)
**Category:** IT Support / System Administration
**Author:** Mafiana

## Overview

This lab covers the full lifecycle of Linux user account management — creation, configuration, privilege escalation, auditing, and safe cleanup — practiced hands-on in a virtualized Ubuntu/Kali environment. The goal was to build practical command-line fluency in tasks a real IT support or SOC analyst role would require: provisioning accounts, managing access, troubleshooting permission errors, and maintaining a clean, auditable system.

## What I Did

- Created and configured multiple test user accounts, including password setup and home directory management
- Practiced switching between users and escalating privileges safely, rather than relying on a persistent root session
- Diagnosed and resolved real errors encountered during account cleanup, including process-ownership conflicts and non-empty directory removal
- Audited multiple user accounts and home directories as an administrator, using root-level access and shell loops to inspect them in batch
- Managed file/directory permissions and ownership across owner, group, and other
- Verified group membership and account configuration after making changes

## Commands Used

| Task | Command |
|---|---|
| Create user (with home dir) | `sudo useradd -m username` |
| Set password | `sudo passwd username` |
| Switch to user | `su - username` |
| Run one command as another user | `sudo -u username command` |
| List processes owned by a user | `ps -u username` |
| Kill a specific process | `sudo kill PID` / `sudo kill -9 PID` |
| Kill all processes for a user | `sudo pkill -u username` |
| Terminate a lingering login session | `sudo loginctl terminate-user username` |
| Delete user (and home dir) | `sudo userdel -r username` |
| Remove a non-empty directory | `sudo rm -r foldername` |
| List all system users | `cut -d: -f1 /etc/passwd` |
| List only human users (UID ≥ 1000) | `awk -F: '$3 >= 1000 {print $1}' /etc/passwd` |
| Change file permissions | `chmod 755 filename` |
| Change file ownership | `sudo chown user:user filename` |
| Add user to a group | `sudo usermod -aG groupname username` |
| Check disk usage per user | `du -sh /home/username` |
| Verify UID/GID/groups | `id username` |

## Key Lessons

- **`useradd -m` matters.** Forgetting the `-m` flag creates a user with no home directory, which causes confusing errors later.
- **`rmdir` vs `rm -r`.** `rmdir` only removes empty directories — trying to delete a populated folder with it fails with `Directory not empty`. Recursive deletion needs `rm -r` (or `rm -rf` to skip prompts).
- **"User is currently used by process" isn't a dead end.** It means a running process still belongs to that user. `ps -u username` identifies it, and `kill`, `pkill -u`, or `loginctl terminate-user` clear it before deletion.
- **`sudo` over root login.** Working as root full-time removes all safety checks and creates no accountability trail. `sudo` logs every elevated action against the real username and limits elevation to the specific command that needs it — the standard expected in professional environments and security audits (e.g. CIS benchmarks).
- **Root can bypass permissions, but that doesn't mean you should skip checking them.** Reviewing ownership (`chown`) and permissions (`chmod`) is still good practice even when admin access lets you route around it.

## Why This Matters

Account provisioning, access control, and audit trails are core responsibilities in both IT support and SOC analyst roles. This lab was a low-stakes way to build muscle memory for the commands and reasoning behind them before working with production systems.
