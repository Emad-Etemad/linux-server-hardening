# 07 – Fail2ban SSH Protection

## Objective

Use Fail2ban to monitor SSH authentication failures and temporarily ban source IP addresses that exceed a configured retry threshold.

## Initial Service and Jail Status

Fail2ban was already installed and running on the Ubuntu VM.

Version check:

```bash
fail2ban-client --version
```

Reported version:

```text
Fail2Ban v1.1.0
```

The service was active and enabled:

```bash
sudo systemctl status fail2ban
```

The Fail2ban client reported one active jail:

```bash
sudo fail2ban-client status
```

Output identified the active jail as `sshd`. The SSH jail status was inspected with:

```bash
sudo fail2ban-client status sshd
```

The jail was monitoring SSH events through the systemd journal, matching `ssh.service` and `sshd`.

## Inspect Existing Runtime Settings

The active SSH jail initially reported:

- `maxretry`: 5
- `findtime`: 600 seconds
- `bantime`: 600 seconds
- `ignoreip`: no explicitly ignored addresses

The values were queried using:

```bash
sudo fail2ban-client get sshd maxretry
sudo fail2ban-client get sshd findtime
sudo fail2ban-client get sshd bantime
sudo fail2ban-client get sshd ignoreip
```

## Create a Local SSH Jail Override

A separate local override was created at:

```text
/etc/fail2ban/jail.d/sshd.local
```

Its contents:

```ini
[sshd]
enabled = true
maxretry = 5
findtime = 10m
bantime = 1h
```

This keeps the project's local SSH jail settings separate from the package-provided default configuration.

## Validate and Apply the Configuration

The configuration was tested:

```bash
sudo fail2ban-client -t
```

The test returned:

```text
OK: configuration test is successful
```

Fail2ban was then restarted to load the updated configuration:

```bash
sudo systemctl restart fail2ban
```

The service status confirmed that it was active and running after the restart.

## Verify Runtime Settings

The active runtime values were checked after restarting Fail2ban:

```bash
sudo fail2ban-client get sshd maxretry
sudo fail2ban-client get sshd findtime
sudo fail2ban-client get sshd bantime
sudo fail2ban-client status sshd
```

Verified values:

| Setting | Runtime value | Meaning |
|---|---:|---|
| `maxretry` | 5 | Five failures trigger a ban. |
| `findtime` | 600 seconds | Failures are counted within a 10-minute window. |
| `bantime` | 3600 seconds | A matching source IP is banned for one hour. |

The SSH jail remained active. At the time of verification, there were zero currently failed attempts, zero currently banned addresses, and zero total bans reported by the jail.

## Result and Scope

The SSH Fail2ban jail is enabled and running with a five-attempt threshold, a ten-minute observation window, and a one-hour ban duration.

The configuration and active runtime values were verified. No deliberate failed-login sequence was generated to trigger a ban during this configuration step, so an actual ban event has not been demonstrated.

Fail2ban complements the SSH authentication controls and UFW firewall; it does not replace either of them.
