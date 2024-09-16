# Void Linux Setup Guide for T480 with Xfce and Intel i5

This guide provides step-by-step instructions to set up Void Linux with the Xfce desktop environment on an Intel-based ThinkPad T480 laptop.

## Prerequisites
- Ensure you have a working installation of Void Linux.
- Make sure your system is connected to the internet.

## Commands Overview

The following commands are used throughout this setup guide:

### xi
This command is similar to `xbps-install -S`, but it takes into account the current directory and sudo/su considerations. Usage example:
```bash
xi package-name
```

### xbps-install Flags
- `-S` or `--sync`: Synchronize remote repository index files.
- `-y` or `--yes`: Assume yes to all questions and avoid interactive questions.
- `-u` or `--update`: Update all installed packages.
- `-v` or `--verbose`: Enable verbose messages.

## System Preparation

```bash
sudo xbps-install -Syuv
```
```bash
xcheckrestart
```

### Install additional repositories and essential packages
```bash
xi void-repo-debug void-repo-multilib void-repo-multilib-nonfree void-repo-nonfree nano
sudo xbps-install -Syuv
xcheckrestart
```

## Graphics Configuration

1. **Configure Intel graphics**:
    ```bash
    xi linux-firmware-intel mesa-dri vulkan-loader mesa-vulkan-intel intel-video-accel
    ```
    - Set the `LIBVA_DRIVER_NAME` environment variable to `i965`.
        ```bash
        export LIBVA_DRIVER_NAME=i965
        ```

2. **Edit GRUB**:
    - Add `intel_iommu=igfx_off` to your kernel command line:
        ```bash
        sudo nano /etc/default/grub
        ```
    - Update GRUB after making changes:
        ```bash
        sudo update-grub
        ```

## Firmware and Additional Software

1. **Install Intel microcode**:
    ```bash
    xi intel-ucode
    sudo xbps-reconfigure --force example "linux-6.6.51_1"
    ```

2. **Add Flatpak support**:
    ```bash
    xi flatpak
    flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
    ```

3. **Enable Bluetooth**:
    - Install necessary packages:
        ```bash
        xi bluez libspa-bluetooth dbus
        ```
    - Enable the `dbus` and `bluetoothd` services:
        ```bash
        sudo ln -s /etc/sv/dbus /var/service
        sudo ln -s /etc/sv/bluetoothd /var/service
        ```
    - Restart your computer:
        ```bash
        sudo reboot
        ```

4. **Install additional software**:
    ```bash
    xi blueman rofi kitty
    ```

5. **Configure Rofi shortcut** (optional):
    Bind `rofi -show drun` to a keyboard shortcut for easier access.

6. **Manage Bluetooth devices**:
    ```bash
    blueman-manager
    ```

## Network Configuration

1. **Install and configure network services**:
    ```bash
    xi ufw wireguard NetworkManager-pptp NetworkManager-l2tp NetworkManager-devel NetworkManager-l2tp NetworkManager-openvpn dnscrypt-proxy
    ```
   
2. **Configure UFW** (optional):
    - Open the UFW configuration file:
        ```bash
        sudo nano /etc/default/ufw
        ```
        - IPv6=no
    - Disable and re-enable UFW to apply changes:
        ```bash
        sudo ufw disable
        sudo ufw enable
        ```
        
    3.**Enable Dnscrypt-proxy**:
     ```bash
    sudo ln -s /etc/sv/dnscrypt_proxy /var/service
    ```

    4.**Final reboot**:
     ```bash
    sudo reboot
    ```

## Optional: Mount a Drive

1. **Mount a drive** (if applicable):
    - List block devices:
        ```bash
        lsblk
        ```
    - Create a mount point:
        ```bash
        sudo mkdir /data
        sudo chown -R $USER:$USER /data
        ```
    - Add your device to `/etc/fstab` for automatic mounting:
        ```bash
        sudo nano /etc/fstab
        ```
    - Mount the drive manually:
        ```bash
        sudo mount -a
        ```

## Flatpaks

1. **Install final applications**:
    ```bash
    flatpak install freetube bitwarden librewolf
    ```

