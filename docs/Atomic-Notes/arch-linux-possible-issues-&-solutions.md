---
tags:
    - Arch
    - Troubleshooting Linux
---

# Arch Linux Possible Issues & Solutions

## Network Related

### Wireguard Tools
Follow the **tldr** solution below
**Issue:** 
`resolv: command not fount`
**Solution:** 
`sudo pacman -S systemd-resolvconf && sudo systemctl enable --now systemd-resolved`

First you need to install the actual package `sudo pacman -S wireguard-tools`
The issue resides when trying to connect, you might get an error that says `resolv: command not found` 
Fix: install `systemd-resolv` package 
```bash
sudo pacman -S systemd-resolvconf

#Enable the systemd-resolved service
sudo systemctl enable --now systemd-resolved
```
### Fonts No Loading/Showing
**Issue:**  
fonts are not showing up when running `fc-list | grep "Font_Name"`
**Solution:**
Run `fc-cache --force` if `fc-cache` does not work

### Tuta Email Client/Apps Storing Keys
**Issue:**
Tuta is unable to store encryption key on **Arch** systems that do not have **Gnome-Keyring** installed
**Solution:**
Install [**Gnome-Keyring**](https://archlinux.org/packages/?sort=&q=gnome-keyring&maintainer=&flagged=) `sudo pacman -S gnome-keyring`

### Neovim Stylua Failing to install
**Issue:**
Installing Neovim Kickoff, Stylua fails to install
**Solution:**
Install **Unzip** `sudo pacman -S unzip`
