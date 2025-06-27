# blackarch-setup

How to install BlackArch Linux in 2025

## Intro

After trying tons of distros, I found that Arch with Hyprland was the best blend of performance and aesthetic. As someone who does a lot of pentesting, BlackArch made the most sense to me out of all the Arch variations...except that the installers on [blackarch.org](blackarch.org) are all broken.

As you probably know, the official BlackArch ISOs have not been updated since 2023. The recommended installation method is to first install Arch, then sync the BlackArch repositories from AUR. After going through this process (realistically for no reason other than for fun), I recommend that you install the BlackArch repository over Arch as a base if possible. If you are stubborn and want to use the BlackArch installer in its current state, and/or would like to see what that process currently looks like, you are in the right place. Just know that this is for educational purposes and there are better ways to do this.

This guide will be broken down as follows:

- [How to install Arch (with BlackArch Installer)](/archInstall.md)
- [How to build Arch (intro)](/archBuild.md)
- [How to install *my* preferred GUI (Hyprland)](/hyprlandInstall.md)

## Disclaimers

- Follow this guide at your own risk
- By following this guide, you understand that you may do damage to your existing OS partitions
- This guide assumes you are installing on a UEFI system
- This is a guide of what I did for *my* setup - I chose not to dual boot and opted for open-source alternatives to Windows programs and/or Wine if I really need the Windows version
- If you do not know what you are doing, please try this in a VM
- For educational purposes **only** - I am not responsible for how you use the tools mentioned

## Credits

- Shoutout to Damian Cruz from Logos Red, who helped find workarounds for some of the older bugs that still exist with the netinstall iso - his guide can be found [here](https://logos-red.com/blog/how-to-install-blackarch-a-step-by-step-guide/)
