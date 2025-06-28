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

- Use the command `blackarch-install` to begin the installer; ignore all errors for now
- Select the following options at each prompt:
    - [1] `Install from Blackarch repository (online)`
    - [1] `Quiet`
    - [1] `Set a locale`
    - [?] \<press enter>
    - [1] `Set a keymap`
    - [?] \<press enter>
    - `Set your hostname:` \<type your own>
- Once you are met with the `[+] Network interface configuration` menu, choose [1] if you are using Ethernet/VM and [2] if using WiFi
- When prompted, select your network interface from the list and connect to your desired network
    - If installing in a VM, ensure your virtual network adapter is set up beforehand so that it can act as an ethernet connection on `eth0` or similar interface
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

At this point, if you run blackarch-install with a working connection and properly configured pacman.conf, the keyring update should happen without any errors. From there, you can follow the installer as before until the disk partitioning step. Make sure to press yes (y) after connecting to find the best server.

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

## 5) Hard Drive Setup

- Once disks are partitioned, you will have the option to have a fully encrypted root
    - This is more secure but will drastically reduce the life of your drive (use at your own risk)
- You will then be prompted to map your EFI, root, and swap (optional) partitions that you just made to the system
    - When asked for EFI, choose the /dev/xxx that says *EFI System* next to it
    - When asked for Root partition, choose the /dev/xxx that says *Linux filesystem* next to it
    - When asked for Swap partition, choose the /dev/xxx that says *Linux swap* next to it
- Select yes at each prompt to confirm changes - your drive will be formatted according to the partitions you set up

#### Disclaimer: There are many other ways to partition your drives; this is just the way I chose to partition mine, and is a good starting point when booting a UEFI system

## 6) User/Timezone Setup

- Once the base Arch packages install, you will be prompted for user setup
- You **must** create a root account - use a strong password
- It is strongly recommended for you to create a standard user so that you can build the habit of not doing everything with root permissions
- Once you have created your users, you will be prompted to edit your timezone and select a mirror
    - The mirror you choose only affects how fast the packages will be downloaded (choose a mirror close to you for best performance)

At this point, you can choose to install X11 and the default window managers that come with BlackArch (GUI). If you are planning on installing a different desktop or using BlackArch as shell only, select no (n). If you are unsure, select yes (y).

## 7) BlackArch Packages

At this point, if you choose not to install any of the BlackArch packages, you essentially have a base Arch Linux installation. If you are looking to explore the more security-focused tools that come with BlackArch, feel free to install them. Otherwise, your installation of Arch/BlackArch is complete!