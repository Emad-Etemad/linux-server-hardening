# 01 – System Baseline Inspection

## Objective

Inspect the initial Ubuntu installation before making any security modifications.

The purpose is to establish a baseline that can be compared against the final hardened configuration.

## Environment Information

- Distribution: Ubuntu 26.04.1 LTS
- Architecture: ARM64 (aarch64)
- Kernel: Linux 7.0.0-43-generic
- Virtualization: UTM
- Username: emad

## Commands Executed

### 1. Operating System Information

```bash
cat /etc/os-release
```

Identifies the Linux distribution, version and release information.

### 2. Kernel Information

```bash
uname -a
```

Displays kernel version, architecture and system information.

### 3. Current User

```bash
whoami
```

Identifies the currently logged-in user.

### 4. User Identity and Groups

```bash
id
```

Displays user ID, group ID and group memberships.

The account belongs to the `sudo` group, indicating administrative privileges.

### 5. Network Interfaces

```bash
ip addr
```

Displays network interfaces and assigned IP addresses.

The primary network interface is `enp0s1`.

### 6. Disk Space

```bash
df -h
```

Displays available and used filesystem space.

The root filesystem initially had approximately 14 GB available.

## Initial Observations

- Ubuntu was freshly installed.
- The default user had administrative privileges through sudo.
- The VM had an active network connection.
- Sufficient disk space was available for the project.

## Result

The initial system inspection was completed successfully.

No security configurations were modified during this stage.
