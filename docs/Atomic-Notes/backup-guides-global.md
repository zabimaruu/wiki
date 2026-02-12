---
tags:
    - Backup
    - Syncthing
---


# File Syncthing Applications
## Syncthing
### Debian Installation Headless
[Installation Source](https://apt.syncthing.net/)
*Note: Syncthing GUI is only accessible from within the server, to change this start syncthing with the following parameters*
`syncthing --gui-address=<address+port>` ex. `syncthing --gui-address=0.0.0.0:8384`

#### Arch Installation
```bash
sudo pacman -S syncthing
```

Enable Systemd Service
```bash
systemctl --user enable syncthing.service
systemctl --user start syncthing.service
```
*Note: run these commands as `non-sudo` user level services should never be run as `sudo`*
