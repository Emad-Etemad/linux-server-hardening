# 05 – SSH Key-Based Authentication

## Objective

Create an Ed25519 SSH key pair on the Mac client, install the public key on the Ubuntu VM, and verify that a new SSH connection works without using the Ubuntu account password.

## SSH Key Pair

The key pair was generated on the Mac, which is the SSH client:

```bash
ssh-keygen -t ed25519
```

The generated files are:

- `~/.ssh/id_ed25519`: private key. It remains on the Mac and must never be uploaded or shared.
- `~/.ssh/id_ed25519.pub`: public key. This is installed on the Ubuntu server.

A passphrase was set to protect the private key locally. This passphrase is separate from the Ubuntu account password and is not sent to the server during authentication.

## Install the Public Key on Ubuntu

The public key was added to the Ubuntu account's:

```text
~/.ssh/authorized_keys
```

The SSH directory and authorized-keys file permissions were checked:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

The expected restrictive permissions are:

- `~/.ssh`: mode `700`
- `~/.ssh/authorized_keys`: mode `600`

The authorized-keys file contains public keys permitted to authenticate as that account. Duplicate copies of the same public key were removed so that only one entry remained.

## Verify Key-Based Login

From the Mac, a new SSH connection was opened using the generated private key:

```bash
ssh -o IdentitiesOnly=yes -i ~/.ssh/id_ed25519 emad@<VM-IP>
```

The SSH client requested the private key's passphrase. After it was entered, the connection succeeded without prompting for the Ubuntu account password.

The remote account was verified with:

```bash
whoami
hostname
```

The returned account was `emad`, and the host was the Ubuntu virtual machine.

A second new connection was tested after the SSH hardening configuration had been reloaded. Key-based login continued to work.

## Authentication Concepts

- The private key stays on the client device.
- The public key is stored on the server in `authorized_keys`.
- The private-key passphrase protects the private key at rest on the client.
- The Ubuntu account password is not required for the verified key-based SSH login.
- `IdentitiesOnly=yes` tells the SSH client to use only the explicitly selected identity for that connection; it does not change the server's authentication policy.

## Result

Ed25519 key-based SSH authentication was successfully configured and verified from the Mac to the Ubuntu VM after password and keyboard-interactive authentication had been disabled.

No private key or passphrase is included in this repository.