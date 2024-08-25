# Void-Setup-guide-t480-xfce-intel i5

## Commands

 **xi pkgs** ...
    — like ‘xbps-install -S’, but take cwd repo and sudo/su into account

**xbps-install flags used**  
-S, --sync
    Synchronize remote repository index files.
    
-y, --yes
    Assume yes to all questions and avoid interactive questions.
    
-u, --update
    Performs a full system upgrade: all installed packages (except those on hold, see --mode hold in xbps-pkgdb(1)) will be updated to the greatest versions that were found in repositories.
    
-v, --verbose
    Enables verbose messages.

**Update System**

sudo xbps-install -Syuv

xcheckrestart

**install repos**

xi void-repo-debug void-repo-multilib void-repo-multilib-nonfree void-repo-nonfree nano

sudo xbps-install -Syuv

xcheckrestart

**Configure intel graphics**

xi linux-firmware-intel mesa-dri vulkan-loader mesa-vulkan-intel intel-video-accel

LIBVA_DRIVER_NAME=i965

sudo nano /etc/default/grub

Add`intel_iommu=igfx_off` to your [kernel cmdline](https://docs.voidlinux.org/config/kernel.html#cmdline).

sudo update-grub

**Intel-firmware**

xi intel-ucode

sudo xbps-reconfigure --force intel-ucode

**Add Flatpak**

xi flatpak

flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

**Enable Bluetooth**

xi bluez libspa-bluetooth dbus

sudo ln -s /etc/sv/dbus /var/service

sudo ln -s /etc/sv/bluetoothd /var/service

sudo reboot

xi blueman rofi kitty
#To use rofi bind 'rofi -show drun' to a keyboard shortcut

blueman-manager

## For me(optional)
**Mount a drive**

lsblk

sudo mkdir /data

sudo chown -R $USER:$USER /data

sudo nano /etc/fstab

/dev/(lsblk output) /data    ext4    defaults        0       0

sudo mount -a

**Final**

flatpak install freetube bitwarden librewolf

FORCE_IPV4 #only use ipv4

sudo ufw enable



