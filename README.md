# About
This is an **unoffical** Arch Linux installation guide. This guide tries to make the Arch Linux installation easier. If you have a question how can open an [issue](https://github.com/byTheInK/Easier-Arch/issues).

# Setting up

## Installing the ISO file
First of all we have to install the ISO file from the [offical Arch Linux website](https://archlinux.org/download/). I would recommend installing this file with qBitTorrent or another Torrent client.

## Writing the ISO image to a USB
After installing we have to write this ISO file in to a USB. I am going to recommend Ventoy for this. You can download this from the [offical Ventoy website](https://www.ventoy.net/en/download.html). After opening Ventoy you will see a menu. First select the USB device that you want to write the ISO file. After selecting click install and it will start installing.

> [!CAUTION]
> This will erase the USB disk.

After installing Ventoy you can create a new folder in that disk and you can name it something like Linux. Now we can move the ISO file to that folder.

## Opening Arch Linux
You first have to enter to the BIOS. This process depends on your system. If you don't know how you can search how to do it. When you enter to the BIOS menu you have to select an option called something like live boot. You have to select that USB disk. A Ventoy menu will pop up. From there select Arch Linux. When you enter, you have to wait few seconds.

# Installation
# Setting the keyboard layout
First of all we are going to load the keyboard layout. The command below will list all of the layouts.
```
localectl list-keymaps
```

To load a layout we can use the command below.
```bash
loadkeys KEYBOARD_LAYOUT
```
For example we are going to load the Turkish layout.
```bash
loadkeys trq
```
## Connecting to the internet

