# Linux Server Hardening

## Overview

This project focuses on practical Linux system administration and security hardening using Ubuntu Linux in a virtual machine running on UTM.

The objective is to develop hands-on experience with Linux administration, user management, SSH security, firewall configuration, intrusion prevention and system security verification.

## Environment

| Component | Details |
|---|---|
| Operating System | Ubuntu 26.04.1 LTS |
| Architecture | ARM64 (aarch64) |
| Virtualization | UTM |
| Kernel | Linux 7.0.0-43-generic |
| Shell | Bash |

## Project Objectives

- Understand Linux users, groups and permissions.
- Configure secure SSH access.
- Implement SSH key-based authentication.
- Further restrict root SSH access.
- Configure UFW firewall.
- Install and configure Fail2ban.
- Apply security updates.
- Verify and document security configurations.

## Project Progress

| Stage | Status |
|---|---|
| System inspection | Completed |
| System updates | Completed (phased updates pending) |
| Users and permissions | Completed |
| SSH configuration | Completed |
| SSH key authentication | Completed |
| Firewall configuration | Completed |
| Fail2ban SSH protection | Completed |
| Security verification | In progress |

## Documentation

Detailed implementation notes, commands, configuration changes and verification results are maintained in the `documentation/` directory.

- [01 – System baseline inspection](documentation/01-system-baseline.md)
- [02 – System updates](documentation/02-system-updates.md)
- [03 – Users and permissions](documentation/03-users-and-permissions.md)
- [04 – SSH configuration](documentation/04-ssh-configuration.md)
- [05 – SSH key authentication](documentation/05-ssh-key-authentication.md)
- [06 – UFW firewall configuration](documentation/06-ufw-firewall.md)
- [07 – Fail2ban SSH protection](documentation/07-fail2ban-ssh-protection.md)

## Disclaimer

This project is conducted in an isolated virtual machine for educational purposes. Configuration changes and security tests are performed on systems under my control.
