---
tags:
    - Debian
---

## Installing/Enabling SSH
Installing *OpenSSH*
```bash
# Update package list
sudo apt update

# Install OpenSSH server
sudo apt install openssh-server

# Check the status of SSH service (this should start automaticlly after OpenSSH gets installed)
sudo systemctl status ssh

# Enable SSH service on Boot
sudo systemctl enable ssh
```

### Enabling ufw Rule
```bash
# Install Firewall ufw
sudo apt install ufw

# Allow SSH traffic on default prot 22
sudo ufw allow ssh

# If the step above fails, try this
sudo ufw allow 22/tcp
```

## XRDP X11

This has been tested on **Debian 13**
First, make sure that you have a **Desktop Environment** installed, I have only tested this on **XFCE4**

```bash
sudo pacman -S xrdp
```

Reboot, the server/desktop and now you should be able to access the server/desktop remotely by using a **RDP** client, see [[Awesome - Linux Applications#Remote Tools]] for apps to use in your platform

## Add User To Sudoers
