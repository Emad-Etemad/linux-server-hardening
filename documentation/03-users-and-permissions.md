# 03 – Users, Groups and File Permissions

## Objective

Inspect local user accounts and group memberships, understand Linux file permissions, and practise changing ownership and access permissions in a dedicated lab directory.

## User and Group Inspection

### 1. Inspect the Current User

```bash
whoami
id
groups
```

These commands identify the current account, its UID/GID and supplementary group memberships.

The main administrative account used in this lab is `emad` (UID 1000). It belongs to the `sudo` group, which grants administrative privileges through `sudo`.

### 2. Inspect Account and Group Databases

```bash
getent passwd emad
getent group sudo
getent group users
```

The local account database provides information about users and their primary groups. The group database shows group membership.

The `/etc/passwd` file contains account information. Its password field normally contains `x`, indicating that the password hash is stored separately in `/etc/shadow`. Password hashes were not accessed or documented.

### 3. Inspect Sensitive Account File Permissions

```bash
ls -l /etc/passwd /etc/shadow
```

The files have different access permissions because `/etc/shadow` contains sensitive password-hash information. Its contents should not be exposed.

## Linux File Permissions

Linux permissions are defined for three classes:

- User (owner)
- Group
- Others

Each class can have read (`r`), write (`w`) and execute (`x`) permissions.

For regular files, read allows viewing contents, write allows modifying contents, and execute allows running the file as a program.

For directories, read allows listing names, write allows creating or removing directory entries, and execute allows traversing the directory and accessing entries subject to their own permissions.

### 4. Inspect Permissions

```bash
ls -l
ls -ld .
stat textfile.txt
```

`ls -l` displays entries and their permissions. `ls -ld` displays information about the directory itself. `stat` provides detailed metadata for a file.

### 5. Practise Numeric Permissions

```bash
chmod 600 textfile.txt
chmod 644 textfile.txt
```

- `600`: owner can read and write; group and others have no access.
- `644`: owner can read and write; group and others can read.

### 6. Practise Symbolic Permissions

```bash
chmod o-r textfile.txt
chmod u+x textfile.txt
chmod u-x textfile.txt
```

These commands remove read permission from others, add execute permission for the owner, and then remove the owner's execute permission.

### 7. Change Group Ownership

```bash
chown :users textfile.txt
```

This changes the file's group ownership to `users` without changing its owner.

### 8. Inspect and Change Directory Permissions

A test directory was created and its permissions were changed:

```bash
mkdir directory-test
chmod 700 directory-test
chmod 755 directory-test
```

- `700`: only the owner has access.
- `755`: the owner has full access; group and others can list and traverse the directory but cannot create or remove entries.

## Lab Result

A dedicated permissions lab was created under the user's home directory. File ownership and permissions were inspected and modified using `chmod`, `chown`, `ls` and `stat`.

The test file's final recorded permissions were:

```text
-rw-r----- emad users
```

This corresponds to mode `640`: the owner can read and write, the group can read, and others have no permissions.

The users, groups and permissions exercises were completed.