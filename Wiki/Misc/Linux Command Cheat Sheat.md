---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/os/linux"
status:
  - todo
created-date: 2024-12-15
related: 
summary: Linux frequently used commands
---

# Command Cheat Sheet

## Permission

```bash

# Directory
sudo find Example -type d -exec chmod 755 {} \;

# File
sudo find Example -type f -exec chmod 644 {} \;

# File format
find . -name "*.sh" -exec chmod +x {} \;
```
## Compression

### XZ files

1. `tar -Jxvf <file>.tar.xz`

## Mounting Remote Windows File System

Mounting windows share folder in Linux [link](https://linuxize.com/post/how-to-mount-cifs-windows-share-on-linux/)

```bash
sudo mount -t cifs -o username=<win_share_user>,password=<win_share_password> //WIN_SHARE_IP/<share_name> /mnt/win_share

# for example
sudo mount -t cifs -o username=mshelia,password="rand0m_pa$$w0rd" //crmhyd/builds636/PROD/LA.VENDOR.1.0-73303-WAIPIO.QSSI13.1.CONSOLIDATED-9 kasan_build/
```
## Mounting Remote Linux FS via ssh

```shell
# Step 0 : install the dependencies
sudo apt install sshfs

# Step 1: Create Mount Point
sudo mkdir /mnt/remote_fs_mnt_point

# Step 2: Mount the Remote Filesystem
sshfs username@remote-server:/path/to/remote-directory ~/remote-mount

# Step 3: Unmount the FS
sudo umount /mnt/remote_fs_mnt_point
```

## Misc

1. `dot -Tpng filename.dot -o outfile.png`

![[Pasted image 20220519122851.png]]