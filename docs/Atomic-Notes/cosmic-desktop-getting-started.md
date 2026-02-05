---
tags:
    - Arch
    - Cosmic
    - Linux
---

# Cosmic Desktop - Getting Started

## Installation

## Add-on & Configuration
Missing `caffeine`? install the following to bring this feature back
```bash
# Install all dependencies
sudo pacman cmake just pkg-config cargo

# Clone and Install Caffeine
git clone https://github.com/tropicbliss/cosmic-ext-applet-caffeine
cd cosmic-ext-applet-caffeine
just build-release
sudo just install
```

For more information, and updates visit [GitHub Project Repo](https://github.com/tropicbliss/cosmic-ext-applet-caffeine)

To add `caffeine` to **Cosmic's Desktop** do the following
Open COSMIC settings and navigate to 
	Desktop -> Panel -> Configure panel applets. Caffeine will be available to add from the Add Applet button.
