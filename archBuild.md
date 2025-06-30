Building Arch
============

Now that your base system installed, it is time to build it out. Let's start with some tools and packages you'll need to complete your base setup.

## Pre-Boot

- Make sure you are still in your live BlackArch ISO (if not, you can boot back into it)
- If you are booting back in to it after the fact, you may need to run `blackarch-install` again to reconnect to network or otherwise reconnect using wpa supplicant, iwctl, etc
- Use `mount | grep /mnt` to check if your main system is mounted
- If you did not shut down your system following the initial install, this is likely already done
- If not mounted, use the folloiwng:
    - `mount /dev/<your root partition> /mnt`
    - `mount /dev/<your efi partition> /mnt/boot`
    - `swapon /dev/<your swap partition>
- Once done, `arch-chroot /mnt` to temporarily change your root device to your actual Arch system

### Key system components

You can now install any packages you wish using `pacman` as if you were in the system itself. I recommend the following as a minimum:

- Networking tools to connect WiFi/Ethernet | iwd, dhcpcd, networkmanager
- Text editor(s) | vim, nano, gedit
- System tools | sudo

`sudo pacman -S iwd dhcpcd vim nano gedit sudo`

If you are here because you want to explore the BlackArch tools, you can also add blackarch to that list (`pacman -S blackarch`)

## Troubleshooting

If you are having issues with downloading the packages using pacman within your chrooted environment, here are some of the common issues:

- Check your connection using `ip addr` and `ping`
    - If you are connected, your interface will return an IP when `ip addr` is used - if your connection fails here, your interface is down or you are not connected to a network
    - If you do have an IP, you can `ping` common DNS servers like 1.1.1.1 or 8.8.8.8 to confirm you have a connection - if your connection fails here, you are unable to reach DNS servers
    - Then, ping archlinux.org - if your connection fails here, your DNS is not resolving properly
- Check that your devices are mounted properly
    - Use `mount | grep /mnt` to make sure that your root partition is mounted at /mnt and your EFI partition is mounted at /mnt/boot
        - You may want to try mounting to /mnt/boot/efi instead if you are having trouble editing EFI/bootloader information
    - Use `exit` to confirm that you are in fact chrooted into the proper system
        - If you are not chrooted into your actual Arch installation, `exit` will log you out
    - Re-use `arch-chroot /mnt` to confirm that you are mounted onto the system correctly
    - `cd /` to confirm that the root is the correct system

## Booting into your official build

Once your tools install, you can now reboot into your normal system and unplug your live ISO/change your boot order in your VM.

- Log in to your normal account (not root)
- By default, your normal user will not have sudo permissions
- If this is not the case, you can skip the next section

### Registering your main user as sudoer

- `logout` of your current user and log in using the root user
    - Please do not use the root user for everything - it is most definitely not best practice
- As root, use `visudo`
- Find the line that says "Uncomment to allow members of group wheel to execute any command"
    - Uncomment the line right below this one, then save your file
- Use `usermod -aG wheel <your username>` to add \<your username> to the "sudoers" equivalent group for Arch
- `logout` of root and log back in to your normal user
- Test `sudo` with a benign command like `ls` or `whoami`
    - If you use `sudo` with `whoami` it will return root even if you are signed in as someone else

### Connecting to a network

Assuming you are connected to ethernet/VM:

- `ip link` to find the name of the interface you want to use
    - Most likely starts with e if it is an ethernet interface or eth0 if VM
- `sudo ip link set <interface name> up` to enable the interface
- `sudo dhcpcd <interface name>` to get an IP from your DHCP server

Assuming you are connecting to a WiFi network:

- `ip link` to find the name of the interface you want to use
    - Often wlan0 or starts with w
- `iwctl` then `station <interface name> connect <name of network>`
- `ip link set <interface name> up`

## Finalizing

Your Arch install is now set up. You can install any desktop environment of your choice on top of it. Here are some examples of display manager/desktop environment combos:

- [gnome](https://wiki.archlinux.org/title/GNOME) Desktop + [gdm](https://wiki.archlinux.org/title/GDM) Display manager | Best option for beginners. Very user-friendly UI, large Icons and app search, very mouse dependent, but polished.
- [xfce](https://wiki.archlinux.org/title/Xfce) + [lightdm](https://wiki.archlinux.org/title/LightDM) | The most lightweight GUI stack in the list, simple but effective desktop environment with a display manager that specializes in being resource light.
- [kde (plasma)](https://wiki.archlinux.org/title/KDE) + [sddm](https://wiki.archlinux.org/title/SDDM) | Highly featured but more resource intensive desktop environment. Ideal for baremetal installs/daily driver systems that need a comprehensive app suite with a great deal of customizability.
- [hyprland] (https://wiki.archlinux.org/title/Hyprland) + [sddm](https://wiki.archlinux.org/title/SDDM) | Most customizable but difficult to set up desktop environment in the list. Similar to Arch in its modular nature, and works best with SDDM, but is also compatible with other display managers.

Note: You can combine most of these display managers with most desktop environments, but these are some of the combinations I have tested personally.