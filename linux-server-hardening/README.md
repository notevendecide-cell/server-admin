# Linux Server Hardening with Ansible

Harden Ubuntu 22.04 and openSUSE Leap servers using Ansible playbooks aligned with CIS/NIST/STIG-inspired best practices. This project configures SSH, password policies, removes insecure services, enables firewalls, automates updates, and configures auditing and logging.

## Features

- SSH hardening and root login disabled
- Password complexity via `pwquality`
- Remove/disable insecure legacy services
- Host firewall (UFW on Ubuntu, firewalld on SUSE)
- Automatic updates (unattended-upgrades / zypper timer)
- Logging: auditd, journald persistence, logrotate
- Compliance check with Lynis

## Quick Start

- Install Ansible on the control node:
  ```bash
  python3 -m venv .venv && . .venv/bin/activate
  pip install -r requirements.txt
  ```
- Define your hosts in `inventory.ini`.
- Run the playbook:
  ```bash
  ansible-playbook -i inventory.ini site.yml -K
  ```

> Tip: Use `-l ubuntu` or `-l opensuse` to target a group.

## Verification

- SSH root login: `sshd_config` shows `PermitRootLogin no`
- Password policy: `/etc/security/pwquality.conf` values set
- Firewall:
  - Ubuntu: `sudo ufw status`
  - SUSE: `sudo firewall-cmd --list-ports`
- Updates:
  - Ubuntu: check `/var/log/unattended-upgrades/unattended-upgrades.log`
  - SUSE: `systemctl status ansible-zypper-update.timer`
- Audit: `sudo auditctl -s`, logs in `/var/log/audit/`
- Compliance: report at `/var/log/compliance/lynis_last.txt`

## Educational Notes

- Config aims to be safe defaults; further tuning recommended per environment and benchmark level.
- Review handlers and `when` conditions for distro-specific behavior.

## License

MIT
