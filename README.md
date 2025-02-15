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

To load a layout we can use the command below. You can use the arrow keys to navigate and you can press `q` to exit.
```bash
loadkeys KEYBOARD_LAYOUT
```
For example we are going to load the Turkish layout.
```bash
loadkeys trq
```
## Connecting to the internet
We first have to ensure our network interface is defined. Use `ip link` to list them.
```bash
ip link
```
### Ethernet
If you want to connect to Ethernet, you can plug the cable. You can now skip the Wi-Fi part.

### Wi-Fi
To connect to the Wi-Fi we will use `iwctl`. Type the command below.
```bash
iwctl
```
Now you will get a prompt looking like `[iwd]#`. We are now going to look at the devices we can use.
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
Now we are going to scan and get the networks.

```bash
[iwd] station YOUR_DEVICE scan
[iwd] station YOUR_DEVICE get-networks
```
Now we can connect.
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

## Changing the timezone
You can check the time with `timedatectl`.
```bash
timedatectl
```
If you think this is correct you can skip this part.

We can list the timezones with the command below.
```bash
timedatectl list-timezones
```
You can use the arrow keys to navigate and you can press `q` to exit. If you find your timezone you can type the command below
```bash
sudo timedatectl set-timezone Zone/SubZone
```

Example:
```bash
sudo timedatectl set-timezone Europe/Istanbul
```
