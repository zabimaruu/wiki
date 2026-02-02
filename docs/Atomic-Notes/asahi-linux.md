---
tags:
    - Apple Linux
    - Asahi
---

# Asahi Linux

**Asahi linux is evolving everyday!**
All of these applications work well on **Linux Arm**
## Flatpaks
Apps that work being installed from **Discover/Flathub**
- **Armcord:** *Discord client works very well with Wayland*
- **Dolphin:** *File Manager, one of the best ones out there*
- **Mpv:** *Media Player, VLC does not work well as of now 4/19/24*
- **SwayNotificationCenter:** *Awesome notification center*
- **Spot:** *Spotify client, as of 4/19/24 this is the only one working on wayland arm64* [Spot](https://github.com/xou816/spot?tab=readme-ov-file)
	- Note: as of 4/19/24 the app can only play "Liked Songs" 
## Command Line Tools:
- **Backlight Support:** *`sudo tee /sys/class/leds/kbd_backlight/brightness <<<4` value from 0-255*
- **Pamixer:** *Controls audio mute, needed to mute audio when clicking on waybar audio icon*
- **Pavulcontrol:** *Audio control panel*
- **Widevine:** *To install run `sudo widevine-installer` the package/installer already comes with Fedora to know more about it, go to > [Read more](https://docs.fedoraproject.org/en-US/fedora-asahi-remix/faq/#_how_do_i_access_protected_content_in_browsers_widevine_drm)*
	- Note: Without this DRM package playing DRM content such as (Spotify, Netflix) will not work on browsers