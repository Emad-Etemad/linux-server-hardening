# 08 – Security Verification

## Purpose

This stage verifies the security controls configured in the earlier stages and reviews the VM's network exposure and running services.

## SSH security verification

The SSH drop-in configuration at `/etc/ssh/sshd_config.d/99-hardening.conf` contains:

```text
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
```

Validation and effective configuration checks:

- `sudo sshd -t` completed without errors.
- `sudo sshd -T | grep '^permitrootlogin'` returned `permitrootlogin no`.
- After reloading SSH, a new connection from the Mac using the configured Ed25519 key succeeded.
- A password-only authentication attempt from the Mac, with public-key authentication disabled on the client, was rejected with `Permission denied (publickey)`.

These checks confirm that key-based access for the regular account works, password-based SSH authentication is rejected, and direct root SSH login is disabled. The regular account can continue to use `sudo`.

## Listening network sockets

Command used:

```bash
sudo ss -tulpen
```

Observed listeners included:

| Service | Port | Bind address | Assessment |
|---|---:|---|---|
| OpenSSH | TCP 22 | All IPv4 and IPv6 interfaces | Required for remote administration; protected by UFW and SSH hardening. |
| systemd-resolved | TCP/UDP 53 | Loopback addresses | Local DNS resolver; not bound to external interfaces. |
| chrony | UDP 323 | Loopback addresses | Local time synchronisation. |
| Avahi | UDP 5353 | All IPv4 and IPv6 interfaces | Local network discovery (mDNS); reviewed as a possible reduction in network exposure. |
| NetworkManager | UDP 546 | IPv6 link-local interface | Network configuration / DHCPv6 client functionality. |
| CUPS | TCP 631 | Loopback addresses | Local printing service; not bound to external interfaces. |

No services were disabled as part of this review. Services should only be removed or disabled after confirming that they are not required by the VM.

## Running services review

Command used:

```bash
systemctl --type=service --state=running --no-pager
```

The VM had 33 running service units in the captured output. This is an Ubuntu desktop VM, so desktop-related services such as GNOME Display Manager, Accounts Service, CUPS, and power management are expected.

Security-relevant services observed as running included:

- `ssh.service`
- `fail2ban.service`
- `unattended-upgrades.service`
- `ufw` was separately verified as active in the firewall stage.

Avahi and printer-discovery services were identified for possible future review. No services were disabled during this verification.

## Automatic updates

The running-services listing showed `unattended-upgrades.service` as active/running. The Ubuntu login banner also reported 8 available updates, all identified as standard security updates.

The exact APT periodic configuration was not captured in this verification record. Therefore, the presence of the service is not treated as proof that automatic security installation is fully configured. The available updates should be reviewed and installed through the normal APT update process.

## Remaining verification items

- Inspect APT periodic settings to confirm the automatic security-upgrade policy.
- Review SSH and Fail2ban journal entries.
- Recheck the five packages previously held back by phased updates; do not force phased updates.
- Consider whether Avahi and printer-discovery services are needed in this VM.

## Result

SSH hardening was successfully validated, including a successful key-based login and a rejected password-only login. Listening sockets and running services were reviewed. No unnecessary services were disabled without evidence that they could safely be removed.

This stage is partially complete until the remaining update and log checks are recorded.
