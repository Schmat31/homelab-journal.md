# Enterprise-Style Homelab Build

## Objective
This project is a hands-on homelab built to simulate the foundational layers of an enterprise network, starting from a single Ubuntu VM. The initial phase focuses on hardening remote access through SSH key-based authentication, with planned expansion into firewall configuration, network segmentation, and automated patch management. The goal is to build practical, defensible security controls one layer at a time — mirroring how real enterprise environments are secured — rather than jumping straight to complex topology.

### Skills Learned
- Generating and managing SSH key pairs (Ed25519) for passwordless, more secure authentication
- Understanding the identity/access-first approach to securing a host before layering on network controls
- Diagnosing and resolving cross-platform SSH tooling gaps (Windows PowerShell vs. Linux/Mac conventions)
- Safe change-management practices for remote administration (keeping a fallback session open before applying changes that could lock out access)
- Planning for automated patch management (`unattended-upgrades`) with controlled, non-disruptive configuration
- Documentation habits for reproducible, auditable homelab work

### Tools Used
- **VirtualBox** — VM hypervisor hosting the Ubuntu lab environment
- **Ubuntu Server** — target VM for hardening and configuration
- **OpenSSH** — key generation and secure remote access (client: Windows PowerShell / OpenSSH; server: Ubuntu `sshd`)
- **PowerShell** — command-line environment on the Windows host
- *(Planned)* **UFW / iptables** — host-based firewall configuration
- *(Planned)* **fail2ban** — automated brute-force protection
- *(Planned)* **unattended-upgrades / msmtp** — automated security patching with email notifications

## Steps

*Ref 1: Network Diagram* — [placeholder — add a diagram of the current single-VM setup, e.g. Windows host → VirtualBox NAT/bridged network → Ubuntu VM]

### 1. Generated an SSH key pair on the host machine
Used `ssh-keygen -t ed25519` on the Windows host (via PowerShell) to generate a private/public key pair, protected with a passphrase. Ed25519 was chosen over RSA for its smaller key size and modern security properties.

*Ref 2: Key generation output* — [add screenshot of successful `ssh-keygen` run]

### 2. Copied the public key to the VM
Initially attempted this from inside the VM's own terminal, which failed — the command needs to run from the host machine (the one holding the private key), not from within the target server. Also found that `ssh-copy-id` isn't bundled with Windows OpenSSH, so used the manual fallback (`type ... | ssh ... "cat >> ~/.ssh/authorized_keys"`) instead.

*Ref 3: Failed attempt from inside VM* — [screenshot already captured showing the `$env:USERPROFILE` path error]

*Ref 4: Successful key copy from Windows host* — [screenshot already captured]

### 3. Verified key-based login
Opened a new terminal session (without closing the original) and confirmed SSH login succeeded using the key's passphrase instead of the VM's account password.

### 4. Planned next-phase hardening
Identified remaining steps before considering SSH fully hardened: disabling password authentication in `sshd_config`, disabling root login, restricting login to an explicit user allowlist, and validating config changes with `sshd -t` before restarting the service.

## Next Phase
- [ ] Disable password authentication on the VM (`PasswordAuthentication no`)
- [ ] Disable root login (`PermitRootLogin no`)
- [ ] Install and configure `fail2ban`
- [ ] Begin host-based firewall configuration (UFW/iptables)
- [ ] Configure `unattended-upgrades` for automated security patching with email alerts

---
*See `homelab-journal.md` for the full chronological log, including exact commands and troubleshooting detail behind each step above.*
