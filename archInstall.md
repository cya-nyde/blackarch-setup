Arch Install | Cya-nyde
=======================

## Pre-Install

- The BlackArch ISO can be downloaded [here](https://blackarch.org/downloads.html)
- We will be using the netinstall ISO to get the most updated packages from the Arch/BlackArch repositories
- If installing baremetal, make sure to flash the ISO to a USB drive using a tool such as [Rufus](https://rufus.ie/en/)
- If installing as a VM, you can use the ISO on its own as a virtual disk
- Important note: **THIS WILL NOT WORK WITH SECURE BOOT ON**
- Ensure Secure Boot is disabled in your BIOS
- This guide is meant for installation on a UEFI system (will be using EFI boot partition and GPT)

## 1) Boot into netinstall ISO

Once you have booted into the netinstall ISO, you should be prompted for a *blackarch login*

The default credentials are:

```
blackarch login: root
Password: blackarch
```

This should give you a shell as root on the live system. Keep in mind that any changes made in this system are volatile.

## 2) Use blackarch-install to connect to network

- Use the command `blackarch-install` to begin the installer prompts; ignore all errors for now.
- Follow the prompts and change your keymap/other settings as needed
- When prompted, select your network interface from the list and connect to your desired network
    - If installing in a VM, ensure your virtual network adapter is set up beforehand so that it can act as an ethernet connection on `eth0`
-Once the `Look for the best server` prompt appears, Ctrl+c to escape the prompt

## 3) Configure pacman to properly install keyrings

Unfortunately, this part requires you to use VIM, as it is the only command line text editor that comes installed with this ISO. Use `vim /etc/pacman.conf` to edit the pacman configuration file and scroll down to

```
[Community]
Include = /etc/pacman.d/mirrorlist
```

(You can do this with the **j** key to scroll down to the proper line and then **i** to begin editing) Make sure to comment out both of those lines. They should look like this when you are done:

```
#[Community]
#Include = /etc/pacman.d/mirrorlist
```

Once you've confirmed that those lines are correct, press esc to exit Insert mode (the `-- INSERT --` at the bottom left should disappear) then type ":wq" to write to the file and quit VIM.

## 4) Disk partitioning

At this point, if you run blackarch-install with a working connection and properly configured pacman.conf, the keyring update should happen without any errors. From there, you can follow the installer as normal until the disk partitioning step. Make sure to press yes (y) to find the best server.

Once you reach the *Hard Drive Setup* step, follow the prompt to select your desired install disk (i.e. sda). If you are not dual booting, select no (n) when prompted. Select yes (y) for both the cfdisk and in memory zeroed partition table prompts.

**IMPORTANT**
Once again, this guide is meant for UEFI systems, and what is done in this step will only work for a UEFI system.

### Here are the steps to partition the disk:

- select `gpt` as your label type
- Select `new` to create a new partition, and size it to 500M
- On the partition you just created, select type and change the type to `EFI System`
- Navigate down one to highlight the next free space
- Select `new` to create a new partition, and size it around 4-6G
- On the partition you just created, select type and change the type to `Linux swap`
- Finally, highlight the next free space and press enter twice

Your final partition table should look something like this:
![partition table](/assets/image.png)