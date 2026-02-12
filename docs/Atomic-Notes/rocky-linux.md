---
tags:
    - Rocky
    - Proxmox
    - Server
---

# Rocky Linux - Getting Started

## Proxmox Hosted Must Do

Install `qemu-guest-agent`
```bash
# This will help you get better control over the VM
sudo dnf install qemu-guest-agent

# Check if qemu-guest-agent starts after installation
sudo systemctl --status qemu-guest-agent

# If not active, activate it
sudo systemctl enable --now qemu-guest-agent

# A reboot might be needed for Proxmox to talk to the agent
sudo reboot
```


