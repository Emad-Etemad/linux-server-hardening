# 04 – SSH Server Installation and Configuration

## Objective

Install the OpenSSH server, inspect how SSH is started on Ubuntu, and configure the SSH daemon to use public-key authentication rather than password-based authentication.

## Install OpenSSH Server

```bash
sudo apt update
sudo apt install openssh-server
```

The correct package name is `openssh-server`.

## Inspect SSH Service and Socket

```bash
systemctl status ssh.socket
systemctl status ssh.service
systemctl is-enabled ssh.socket
ss -tlnp
```

On this Ubuntu installation, systemd socket activation is enabled. The `ssh.socket` unit listens on TCP port 22 and starts `ssh.service` when an incoming SSH connection arrives. Therefore, `ssh.service` may not appear as continuously enabled even though SSH is available through the enabled socket.

## Inspect the Effective SSH Configuration

```bash
sudo sshd -T | grep -E '^(passwordauthentication|pubkeyauthentication|permitrootlogin)'
```

Before hardening, the effective configuration included:

- `PubkeyAuthentication yes`
- `PasswordAuthentication yes`
- `PermitRootLogin prohibit-password`

This showed that public-key authentication was enabled, but password authentication was also permitted.

## Configure SSH Hardening

A configuration drop-in was created:

```bash
sudo nano /etc/ssh/sshd_config.d/99-hardening.conf
```

The following settings were added:

```text
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
```

Meaning:

- `PasswordAuthentication no`: disallows password-based SSH authentication.
- `KbdInteractiveAuthentication no`: disables keyboard-interactive authentication, preventing it from being used as an alternative password prompt.
- `PubkeyAuthentication yes`: enables public-key authentication.

The existing effective root setting remained `PermitRootLogin prohibit-password`. This prevents root password authentication, but it is not the same as setting `PermitRootLogin no`; public-key root login is not categorically disabled by this setting.

## Validate and Apply the Configuration

Before reloading SSH, the configuration syntax was checked:

```bash
sudo sshd -t
```

No output was returned, indicating that the syntax check passed.

The effective settings were then verified:

```bash
sudo sshd -T | grep -E '^(passwordauthentication|pubkeyauthentication|permitrootlogin)'
sudo sshd -T | grep '^kbdinteractiveauthentication'
```

The verified effective values were:

```text
permitrootlogin prohibit-password
pubkeyauthentication yes
passwordauthentication no
kbdinteractiveauthentication no
```

The SSH daemon was reloaded to apply the configuration:

```bash
sudo systemctl reload ssh
```

The reload completed successfully.

## Result

The SSH server was installed and configured to allow public-key authentication while disabling password and keyboard-interactive authentication.

The effective configuration was checked with `sshd -T`, and the daemon configuration was validated with `sshd -t`.