---
tags:
    - Arch
    - Linux
    - Remote
---

# Remote Serves Must Do

## SSH Essentials
If commands are not recognized on the terminal you using, export the following and try reconnecting
	`export TERM=xterm-256color`
To set this option when connecting via *SSH* add the following to *~/.ssh/config*
	`Host *
	SetEnv TERM=xterm-256color
