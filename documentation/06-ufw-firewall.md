# 06 – UFW Firewall Configuration

## Objective

Configure the Uncomplicated Firewall (UFW) on the Ubuntu VM to deny unsolicited incoming connections while allowing outgoing connections and preserving SSH access from the Mac client.

## Initial Status

The firewall was initially inactive:

```bash
sudo ufw status verbose
sudo ufw status numbered
```

Both commands reported:

```text
Status: inactive
```

## Allow SSH Before Enabling the Firewall

An allow rule was added before enabling UFW to avoid blocking the administrative SSH connection:

```bash
sudo ufw allow ssh
```

UFW confirmed that the rule was added for IPv4 and IPv6. The resulting rule was:

```text
ufw allow 22/tcp
```

## Configure Default Policies

Incoming connections were denied by default:

```bash
sudo ufw default deny incoming
```

Outgoing connections were allowed by default:

```bash
sudo ufw default allow outgoing
```

## Enable and Verify UFW

The firewall was enabled:

```bash
sudo ufw enable
```

The resulting status was checked with:

```bash
sudo ufw status verbose
```

The verified configuration was:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
```

## Test SSH Connectivity

After enabling UFW, a new SSH connection was opened from the Mac to the Ubuntu VM using the Ed25519 key:

```bash
ssh -o IdentitiesOnly=yes -i ~/.ssh/id_ed25519 emad@<VM-IP>
```

The connection succeeded. This verified that the firewall was active and that the SSH allow rule preserved remote administration.

## Result

UFW is active with the following policy:

- Incoming connections: denied by default.
- Outgoing connections: allowed by default.
- SSH: allowed on TCP port 22 for IPv4 and IPv6.
- Routed traffic: disabled by default.
- Logging: enabled at low level.

The SSH rule currently allows connections from any source address. Restricting SSH to a trusted source address may be appropriate in a production environment, depending on the server's network and administrative access requirements.

This configuration was applied and verified in the project VM. No claim is made that all possible network exposure has been eliminated.
