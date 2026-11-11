# 02 – System Updates and Package Management

## Objective

Refresh Ubuntu package information and check for available software updates.

## Package Management

Ubuntu uses APT (Advanced Package Tool) to manage software packages.

## Commands Executed

### 1. Refresh Package Information

```bash
sudo apt update
```

Refreshes package information from configured repositories.

### 2. Check Available Updates

```bash
apt list --upgradable
```

Lists installed packages with available updates.

### 3. Upgrade Installed Packages

```bash
sudo apt upgrade
```

Attempts to upgrade installed packages to newer versions.

## Results

The package manager identified five packages with available updates.

However, Ubuntu did not upgrade these packages because they were subject to phased updates.

The following packages were affected:

- distro-info
- python3-distro-info
- python3-software-properties
- snapd
- software-properties-common

## Security Considerations

Ubuntu uses phased updates to gradually distribute certain package updates.

This helps identify potential problems before updates reach all installations.

The phased updates were not forced during this project.

## Result

Package information was successfully refreshed.

Five available updates remained pending due to Ubuntu's phased update mechanism.
