# homelab-security-hardening
# 01 – SSH Key Authentication

**Date:** 2026-11-01
**Host:** Ubuntu Linux server (Docker host)
**Security+ objectives:** Authentication, access control, secure protocols

## Goal
Replace password-based SSH login with key-based authentication to
reduce exposure to brute-force and credential-guessing attacks.

## Steps
1. Generated an ed25519 key pair on the Windows workstation
   (`ssh-keygen -t ed25519`).
2. Added the public key to `~/.ssh/authorized_keys` on the server
   (manual copy, since `ssh-copy-id` is not available in PowerShell).
3. Verified permissions: `~/.ssh` = 700, `authorized_keys` = 600.
4. Tested key-based login from a second session before changing
   any server settings.
5. Set `PasswordAuthentication no` and `PermitRootLogin no` in
   `/etc/ssh/sshd_config`; checked `/etc/ssh/sshd_config.d/` for overrides.
6. Validated config with `sshd -t`, restarted the service.

## Verification
- Key login succeeds.
- Password-only login is rejected ("Permission denied").

## Troubleshooting notes
- `authorized_keys` was empty because `ssh-copy-id` is not available in
  Windows PowerShell. Fixed by pasting the public key manually.
- Kept an existing session open while testing to avoid lockout.

## Lessons learned
- Always test the new login method in a separate session first.
- Check for config overrides in `sshd_config.d/`.
