---
tags:
    - Arch
    - Systemd
---

# Arch Linux Installation - Secure Boot - Systemd Boot

## Enable SSH Agent

Set root password
```bash
passwd
# Then procedd to set the password for root
```
Update Arch repos
```bash
pacman -Sy
```
Enable **SSHD**
```bash
systemctl start sshd
systemctl enable sshd
```
Now, get the **IP** address of the machine you wish to **SSH**
```bash
ip address
```
## Create Partitions For Single Boot Systems (ex. running arch & on a single disk)

Create 2 partitions:
- Partition 1 - Code EF00 - EFI System Partition - 1G
- Partition 2 - Code 8309 - Linux LUKS - Use all of the remaining space on the drive

1. Creating partition using `gdisk`
```bash
# Create a new GUID Partition Table (GPT)
o
Y

# Create the EFI partition
n
[Enter]
[Enter]
+1G
ef00

# Create the LUKS partition
n
[Enter]
[Enter]
[Enter]
8309

# Persist the changes
w
Y
```

## Disk setup
2. Encrypt drive with LUKS **(If installing via SSH)**
```bash
cryptsetup luksFormat \
    --use-random \
    -S 1 \
    -s 512 \
    -h sha512 \
    -i 5000 \
    /dev/nvme0n1p1 
```
*Note: make sure to change the `/dev/<drive>/`*

Type **"YES"** in CAPS
Now, enter your **encryption password**

3. Encrypt drive with LUKS **(If installing directly on device Non-SSH)**
```bash
cryptsetup luksFormat \
    --use-random \
    -S 1 \
    -s 512 \
    -h sha512 \
    -i 5000 \
    /dev/nvme0n1p1 
```
*Note: make sure to change the `/dev/<drive>/

Type **"YES"** in CAPS
Now, enter your **encryption password**

4. Opening **LUKS** encrypted drive
```bash
cryptsetup open /dev/sda2 luks
```
*Note: make sure to pick the right drive (/dev/"yourdrive" and add a string at the end "luks" or something else) *

## Setup Volumes

5. First, create the physical volume, `luks` is use in here, but that can be change when opening the `luks encrypted drive`
```bash
pvcreate /dev/mapper/luks
```

6. Create a volume group
```bash
vgcreate vg /dev/mapper/luks
```

7. Logical volumes creation for `home` `root` and `swap`
*Note: a `swap` partition is needed in order to make `hibernation` to work*

```bash
lvcreate -L 10G vg -n swap #make sure to create at least 2GB above your physical ram
lvcreate -L 100G vg -n root
lvcreate -l 100%FREE vg -n home
```

## Partitioning And Formatting

`EFI` and `Boot/ESP` partitions needs to be formatted as `FAT32` in order to be readable
8. Format the `+2G` partition created earlier in this case it is `/dev/nvme0n1p1`
```bash
mkfs.fat -F 32 /dev/nvme0n1p1
```

9. Active all volumes and enable `dm_mod`
```bash
modprobe dm_mod
# Scan vg volumes
vgscan
# Activate vg volumes
vgchange -a y NameOfVolume(in this cas "vg")
```

10. Format all `lv` volumes as `btrfs`
```bash
mkfs.btrfs -L root /dev/vg/root
mkfs.btrfs -L home /dev/vg/home
# Format swap partition
mkswap /dev/vg/swap
```
*Note: make sure to use the right `vg` label created earlier on [[#Setup Volumes]] Step #6 and #7*

## Mounting partitions

11. Mount everything as follows
```bash
swapon /dev/vg/swap       # Mounts swap
mount /dev/vg/root /mnt   # Mounts root to /mnt

mkdir -p /mnt/{home,boot} # Creates folders in /mnt
```

12. Mount the remaining partitions
```bash
mount /dev/nvme0n1p1 /mnt/boot  # Mounts `EFI` partition to /mnt/boot
mount /dev/vg/home /mnt/home    # Mounts home to /mnt/home
```
*Note: make sure to mount the right partition on `/mnt/boot` this will be outside of the `LUKS` partition*

## Installing Linux and its fundamental utilities

13. Generate `pacman` key to avoid any package installation issues
```bash
pacman-key --init && pacman-key --populate
```

14. Install Linux and other tools (more packages can be added later on)
```bash
pacstrap -K /mnt archlinux-keyring base base-devel dhcpcd openssh vim lvm2 linux linux-headers linux-lts linux-lts-headers linux-firmware net-tools sudo zsh git
```

15. Generate `fstab`
```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab # Make sure you are able to see all partitions otherwise, a step might have been missed
```

Switch to `chroot` and continue setup
```bash
arch-chroot /mnt
```

16. Set up timezone
```bash
ln -sf /usr/share/zoneinfo/America/Los_Angeles /etc/localtime
hwclock --systohc
```

17. Enable `NTP` client to prevent any clock drift and assure time accuracy
Edit `timesyncd.conf`
```bash
vim /etc/systemd/timesyncd.conf
# Add the following under [Time]

[Time]
NTP=0.arch.pool.ntp.org 1.arch.pool.ntp.org 2.arch.pool.ntp.org 3.arch.pool.ntp.org
FallbackNTP=0.pool.ntp.org 1.pool.ntp.org 0.fr.pool.ntp.org
```

To verify configuration, run
```bash
timedatectl show-timesync --all
```
*For more information visit [NTP timesyncd arch wiki page](https://wiki.archlinux.org/title/Systemd-timesyncd)*

Finally, enable the `systemd`
```bash
systemctl enable systemd-timesyncd.service
```

Localization setup, this is meant to use the correct region and language specific formatting

18. Edit `/etc/locale-gen`
```bash
vim /etc/locale.gen
```

19. Remove "#" from the line containing `en_US.UTF-8 UTF-8`
Then, run
```bash
locale-gen
```

20. Create a `locale.conf` to set `LANG` variable
```bash
echo LANG=en_US.UTF-8 >> /etc/locale.conf
```

21. Create `hostname` to differentiate your device
```bash
echo Arch-Frame13 >> /etc/hostname
```

## Initramfs Configuration

22. Edit `/etc/mkinitcpio.conf` and add the following `Hooks`
*Note: make sure to add these 2 hooks in between `block and filesystems`*
```bash
vim /etc/mkinitcpio.conf
Add "encrypt lvm2" in between "block and filesystems" # HOOKS should be at the bottom of the config
```
Recreate the `initramfs` image
```bash
# Before building create the following file
touch /etc/vconsole.conf

mkinitcpio -P # It will recreate both LTS and Linux
```

23. Get `LUKS` `UUID` 
```bash
blkid | grep crypto_LUKS
```
*Note: Make sure to get the right `UUID` it is case sensitive*

## Bootloader Setup (Systemd-D)

24. Install `UCODE` package
Check CPU type
```bash
lscpu
# If Intel
pacman -S intel-ucode --noconfirm

# Else
pacman -S amd-ucode --noconfirm
```

25. Run `systemd-d` install command (this will install loader within `/boot/`)
```bash
bootctl install
```

26. Navigate to `/boot/loader/entries/` and create 2 files *(Since I always install 2 Kernels would need 2 .conf files)*

Arch Linux (Linux) Conf File
```bash
echo "title Arch Linux(Linux)" >> arch.conf
echo "linux /vmlinuz-linux" >> arch.conf
echo "initrd /<YOUR-UCODE-TYPE>.img" >> arch.conf
echo "initrd /initramfs-linux.img" >> arch.conf
echo "options cryptdevice=UUID=<UUID-OF-ROOT-PARTITION>:luks root=/dev/mapper/vg-root rw resume=/dev/mapper/vg-swap rw" >> arch.conf
```

Arch Linux (Linux LTS) Conf File
```bash
echo "title Arch Linux(Linux-LTS)" >> arch-lts.conf
echo "linux /vmlinuz-linux-lts" >> arch-lts.conf
echo "initrd /<YOUR-UCODE-TYPE>.img" >> arch-lts.conf
echo "initrd /initramfs-linux-lts.img" >> arch-lts.conf
echo "options cryptdevice=UUID=<UUID-OF-ROOT-PARTITION>:luks root=/dev/mapper/vg-root rw resume=/dev/mapper/vg-swap rw" >> arch-lts.conf
```
*Note: Make sure to add your `UCODE-TYPE` `Intel` or `AMD` and also, make sure to add your `cryptdevice UUID` which can be acquired on [[#Initramfs Configuration]] Step #23*

Examples to follow
```bash
echo "options cryptdevice=a121212c-1212-1212-1212-a6ec764cdcf7:luks root=/dev/mapper/root rw resume=/dev/mapper/swap rw" >> arch.conf
```

```bash
echo "initrd /intel-ucode.img" >> arch.conf
```

27. Navigate to `/boot/loader/` and create a file named `loader.conf` (This file should be there since we ran `bootctl install` ealier)
```bash
vim /boot/loader/loader.conf
# Add the following options
default  arch.conf
timeout  5
console-mode max
editor   no
# Save and exit `:wq`
```

## User Account Creation

28. Change `root passwd` (This will change the `passwd` we set earlier to access this machine via `ssh`)
```bash
passwd # To change root password
```

29. Create `user` account and add it to `/bin/zsh`
```bash
useradd -mG wheel -s /bin/zsh <user-name>
passwd toniiz # Set password for new user
```

30. Give `wheel` group users access to `root` so, we can install application without becoming `root`
```bash
visudo

# Uncomment the following line
wheel ALL=(ALL:ALL) ALL
# Exit and save :wq
```

## Desktop Environment

31. Install `greetd` this is a super minimal login manager & `tuigreet` which is the graphical console to work with `greetd`
```bash
pacman -S greetd && pacman -S greetd-tuigreet
```

Edit `/etc/greetd/config.toml` and the `command` variable to use `tuigreet`
```bash
vim /etc/greetd/config.toml

# Default will be set to "agreety" change this to "tuigreet"
# Also, change "sway" to a different window manager such as "hyprland"

[terminal]
vt = 1

[default_session]
command = "tuigreet --cmd hyprland"
user = "greeter"

# Enable greetd, so it will start when you boot your system
systemctl enable greetd.service
```

32. Hyprland Installation (Using Pacman Official Repo/Non-AUR)
```bash
pacman -S hyprland hyprpaper hyprlock hypridle hyprpicker xdg-desktop-portal-hyprland hyprsunset hyprpolkitagent hyprland-qt-support
```

Hyprland extra utilities, such as `terminals` (All from Pacman Official Repo/Non-AUR)
```bash
pacman -S alacritty kitty ghostty neovim ranger qt5-wayland qt6-wayland waybar pipewire wireplumber rofi-wayland cliphist nemo yazi superfile networkmanager
```
*Note: Make sure to enable `NetworkManager`*
```bash
systemctl enable NetworkManager
```

Hyprland extra utilities, using `AUR` (Some tools are not available in Pacman Official Repo)
Before all of this, install `yay` (Make sure to make the script executable `chmod +x yay.sh`)
*NOTE: this script will not room while being in `chroot` you will first need to log in as a user!*

```bash
#!/bin/bash

# Update system and install necessary dependencies
sudo pacman -Syu --noconfirm
sudo pacman -S --needed --noconfirm base-devel git

# Clone the yay-bin repository for a pre-compiled version
git clone https://aur.archlinux.org/yay-bin.git

# Navigate into the cloned directory
cd yay-bin

# Build and install yay
makepkg -si --noconfirm

# Clean up the cloned directory
cd ..
rm -rf yay-bin

echo "Yay has been successfully installed."
```

```bash
# One Liner Yay
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

AUR Utilities
```bash
yay xwaylandvideobridge-git
```

## Sanity Checks

33. Make sure that `/etc/fstab` has the following permission for `fmask` and `dmask`
```bash
vim /etc/fstab

# Change default values from "0022" for both fmask and dmask to
fmask=0077,dmask=0077

# quit and save, :wq
```

34. Recreate the `initramfs` images, one last time!
```bash
mkinitcpio -P # It will recreate both LTS and Linux
```

