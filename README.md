# About
This is an **unofficial** Arch Linux installation guide. This guide tries to make the Arch Linux installation easier. If you have a question, you can open an [issue](https://github.com/byTheInK/Easier-Arch/issues).

# Setting up

## Installing the ISO file
First of all you have to install the ISO file from the [official Arch Linux website](https://archlinux.org/download/). I would recommend installing this file with qBitTorrent or another Torrent client.

## Writing the ISO image to a USB
After installing you have to write this ISO file in to a USB. I am going to recommend Ventoy for this. You can download this from the [official Ventoy website](https://www.ventoy.net/en/download.html). After opening Ventoy you will see a menu. First select the USB device that you want to write the ISO file. After selecting click install and it will start installing.

> [!CAUTION]
> This will erase the USB disk.

After installing Ventoy you can create a new folder in that disk and you can name it something like Linux. Now you can move the ISO file to that folder.

## Opening Arch Linux
You first have to enter the BIOS. This process depends on your system. If you don't know how you can search how to do it. When you enter the BIOS menu you have to select an option called something like live boot. You have to select that USB disk. A Ventoy menu will pop up. From there select Arch Linux. When you enter, you have to wait few seconds.

# Installation
# Setting the keyboard layout
First of all you are going to load the keyboard layout. The command below will list all of the layouts.
```
localectl list-keymaps
```

To load a layout you can use the command below. You can use the arrow keys to navigate and you can press `q` to exit.
```bash
loadkeys KEYBOARD_LAYOUT
```
Example:
```bash
loadkeys trq
```
## Connecting to the internet
You have to ensure our network interface is defined. Use `ip link` to list them.
```bash
ip link
```
### Ethernet
If you want to connect to Ethernet, you can plug the cable. You can now skip the Wi-Fi part.

### Wi-Fi
To connect to the Wi-Fi you will use `iwctl`. Type the command below.
```bash
iwctl
```
Now you will get a prompt looking like `[iwd]#`. You are now going to look at the devices you can use.
```bash
[iwd] device list
```
This device is probably `wlan0` but this can change. If your device is turned off you can use the command below.
```bash
[iwd] device YOUR_DEVICE set-property Powered on
```
If your adapter is turned off you can use the command below.
```
[iwd] adapter YOUR_ADAPTER set-property Powered on
```
Now you are going to scan and get the networks.

```bash
[iwd] station YOUR_DEVICE scan
[iwd] station YOUR_DEVICE get-networks
```
Now you can connect.
```bash
station YOUR_DEVICE connect YOUR_SSID
```
If your network is hidden
```bash
station YOUR_DEVICE connect-hidden YOUR_SSID
```
Now you can press `Control + D` to exit iwctl.

### Testing

After these steps you should be able to connect to the internet. You can try by using the command below.

```bash
ping google.com
```
You should be able to send packages. You can press `Control + C` to stop.

## Partitioning
This is probably one of the hardest parts. You are going to use `fdisk` to set the partition.
```bash
lsblk
```
With this commands you will see disks. You can find out the disk by looking at the size. Now You are going to use `fdisk`.
```bash
fdisk /dev/YOUR_DISK
```
Example:
```bash
fdisk /dev/nvme0n1
```
Now press the `d` key and delete all of the partitions one by one. If you deleted all of them you can create new ones. Type `n` to create a new partition.

### Boot
1. Type n
2. Press enter
3. Press enter
4. Press enter
5. Type `+1G`

### Swap
1. Type n
2. Press enter
3. Press enter
4. Press enter
5. Type `+8G`

### Root
1. Type n
2. Press enter
3. Press enter
4. Press enter
5. Press enter

### Setting the types
Type `t`, select `1`, type `L` at the end you will see most common types. Near them you will see their code.

1. Uefi
2. Swap
3. Linux

### Write
You can type `w` to save the changes.

## Formatting
You are going to use [ext4](https://wiki.archlinux.org/title/Ext4) for the filesystem.
```bash
lsblk
```
This will show us the partitions.
1. Fat
2. Swap
3. Ext4

Fat
```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

Swap
```bash
mkswap /dev/swap_partition
```

Ext4
```bash
mkfs.ext4 /dev/root_partition
```

## Mounting

Fat
```
mkdir -p /mnt/boot/efi
mount  /dev/efi_system_partition /mnt/boot/efi
```
Swap
```bash
swapon /dev/swap_partition
```
Ext4
```bash
mount /dev/root_partition /mnt
```

## Installing Linux Firmware
To install Linux and other needed dependencies you can type the command below.
```bash
pacstrap -K /mnt base linux linux-firmware micro grub base-devel sudo efibootmgr networkmanager nano os-prober
```

> [!NOTE]
> This can take a while

## Fstab
You can generate an Fstab with this code.

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

## Entering to the system
You are going to change root into the new system.
```bash
arch-chroot /mnt
```
## Localization
You are first going to generate the locales.
```bash
locale-gen
```
Now you can edit the settings. You can do `Control + Q` to exit.
```bash
micro /etc/locale.conf
```
Here type your language and encoding type.
Example:
```
LANG=en_US.UTF-8
```
or
```
LANG=tr_TR.UTF-8
```

Now you can change the keyboard layout.
```
micro /etc/vconsole.conf
```
Example:
```
KEYMAP=trq
```
## Timezone
To set the timezone you are going to make a symbolic link.
```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
```
Example:
```bash
ln -sf /usr/share/zoneinfo/Europe/Istanbul /etc/localtime
```
## Setting the hostname
```bash
micro /etc/hostname
```

## Setting the root password
```bash
passwd
```
## Enabling network manager
```bash
systemctl enable NetworkManager
```
## Adding a user
To add a user you are going to use `useradd`.
```bash
useradd -m -G wheel -s /bin/bash YOUR_USER_NAME
```
Now you can set the user's password.
```bash
passwd YOUR_USER_NAME
```

## Making the user root
```bash
EDITOR=micro
visudo
```
Now you can uncomment the line.

Old:
```bash
# %wheel ALL=(ALL:ALL) ALL
```
New:
```bash
%wheel ALL=(ALL:ALL) ALL
```

## GRUB
GRUB is the most common Bootloader for Linux. You first have to find the boot mode.
```bash
ls /sys/firmware/efi
```
If you see some files and directories you are in Uefi mode but if you don't see, you are in BIOS mode.

Uefi mode:
```bash
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
```

BIOS mode:
```bash
grub-install --target=i386-pc /dev/YOUR_DISK
```

Let's generate the GRUB configuration file.
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

## Finishing
```bash
exit
umount -R /mnt
reboot
```
Your system will reboot and you will see the GRUB menu. Open Arch Linux from there.
```bash
archlinux login: YOUR_USER_NAME
Password: YOUR_PASSWORD
```
First thing you should do is to update the system.
```bash
sudo pacman -Syu
```

There isn't a desktop environment installed.
