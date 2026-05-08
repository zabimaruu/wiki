---
tags:
  - File Manager
---

## Nemo
A fork of *Nautilus* based on Files 3.4

### Installation
```bash
sudo pacman -S nemo
```
[See Arch Wiki Page](https://wiki.archlinux.org/title/Nemo) to add extra functionality to *Nemo*

**Must Have Programs**
- 
## Thunar
Modern file manager from xfce4 built to be fast and lightweight
### Installation
```bash
sudo pacman -S thunar
```

### Extensions & Extra Functionality
```bash
sudo pacman -S gvfs-smb gvfs-mtp gvfs-gphoto2 gvfs-afc
```

- `gvfs-smb:` Adds windows file shares and anything else using `SMB/CFS` protocol
- `gvfs-mtp:` Media players and mobile devices that use `MTP`
- `gvfs-gphoto2:` Digital cameras and mobile devices that use `PTP`
- `gvfs-afc:` Apple mobile devices

### One Liner Installation
This will install all of the above extensions and `thunar` on `Arch-Linux` [[One Liners - Arch]]
```bash
sudo pacman -S thunar gvfs-smb gvfs-mtp gvfs-gphoto2 gvfs-afc
```
