Once you boot up to the live environment:

1. first step is to connect to internet (duh). use iwctl for wifi.
	```bash
	iwctl
	device list
	static <device> scan
	station <device> get-network
	station <device> connect SSID
	```
	then ping `ping.archlinux.org` to check connectivity
	
2.  once the connectivity is there, next step would be to do update packages and systemclock update
	```bash
	pacman -Syy
	timedatectl
	```

3. next is disk partitioning (to be done carefully):
	1. first do `lsblk` do find the exact drive on which the partitioning to be done
	2. next we do a `sudo fdiks <drive (kinda link /dev/nvme0n1)>`
	3. here use `d` to delete, `p` to print, `n` to create `w` to write the partition table
4. once that is done, lets format and mount the new partition:
	1. `sudo mkfs.ext4 /dev/nvme0n1p5

5. lets mount the partition so that arch can be installed: 
	1. `mount /dev/_root_partition_ /mnt` 
	2. `mount --mkdir /dev/_efi_system_partition_ /mnt/boot`
	3. check the two with: `lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
6. alright once mounting is done, we can move on to installation of base system. Before that lets update the mirrors:
```shell
reflector --country India --country Singapore --country Japan --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```
7. Now we can install linux onto the mount:
```bash
pacstrap -K /mnt base linux linux-firmware amd-ucode nano man-db man-pages
```
8. Now we generate the fstab:
```bash
genfstab -U /mnt >> /mnt/etc/fstab

#you can inspect the same using:
cat /mnt/etc/fstab
```

9. Now that arch is installed lets go inside:
```bash
arch-chroot /mnt
```

10. Now we setup the time and localisation:
```bash
ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime

hwclock --systohc
```
enable the ntp service:
```bash
systemctl enable systemd-timesyncd
systemctl start systemd-timesyncd
```

now open the locale.gen file for localization:
```bash
nano /etc/locale.gen
```
select whichever you want, i did en_us and en_in. then run:
```bash
locale-gen
```
Then make the one you want to be the default as:
```bash
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

11. Network config: set your **hostname** and enable **networking**
```bash
echo "deepblue-arch" > /etc/hostname
```

now do `nano /etc/hosts` and add:

```bash
127.0.0.1   localhost
::1         localhost
127.0.1.1   deepblue-arch
```

now enable the network manager:

```bash
pacman -S networkmanager

systemctl enable NetworkManager
```

12. **Initramfs**: Creating a new initramfs is usually not required, because mkinitcpio was run on installation of the kernel package with pacstrap.
```bash
mkinitcpio -P
```

13. Set the root password:
```bash
passwd
```

14. Install the bootloader: i go with grub always, so here's that:

Since we mounted the EFI partition at `/mnt/boot`, So inside chroot, EFI is located at `/boot` . Install grub:

```bash
pacman -S grub efibootmgr

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

grub-mkconfig -o /boot/grub/grub.cfg
```

15. exit chroot and reboot:
```bash
exit

umount -R /mnt

reboot
```

remember to unplug the install media.

16. Select arch linux at grub. now we should be at tty screen. Login using root username and password selected at step 13.
17. Now first we create a non-root user for obvious reasons:
```bash
useradd -m -G wheel,video,audio,storage soumil

passwd soumil
```


> [!NOTE] Can't connect to network?
> Thats fine. That happened because our arch install is unaware of the network connects we did during install process. Here we can do the following:
> ```bash
> sudo systemctl start NetworkManager
> sudo systemctl status NetworkManager
> ```
>
> If everything done correctly above should show active and running. Now we can connect to wifi:
> ```bash
>nmcli device wifi list
> # Connect to the ssid using:
> nmcli device wifi connect SSID --ask
> ```

Now lets install sudo and enable it for wheel group:

```bash
pacman -S sudo
EDITOR=nano visudo
```

Uncomment this line: `%wheel ALL=(ALL:ALL) ALL`

Save and exit.

18. install nvidia stuff:
```bash
pacman -S nvidia nvidia-utils nvidia-prime
```

19. installing our DE:
```bash
pacman -S hyprland waybar wofi hyprpaper hypridle hyprlock swaync
```

20. Now for the display manager. I have used gdm and sddm in past. GDM is good but usually it makes sense in a gnome based env. For whatever reason sddm just doesnt agree when i use it with hyprland. So options are either do a tty login (which is fine but tiring after a while), or look for some other one... This is when i came acroos ly. Looks cool so going to install that one.
    ```
    sudo pacman -S ly 
    sudo systemctl enable ly.service
    ```

	now it should start on the next reboot.

21. Next thing i have to figure out is my fonts. specifically the icons... right now they are all just random gibber... 
	1. one is the icons: for that we install the font-awesome lib: `woff2-font-awesome`
	2. one is the font: for now just this: `ttf-jetbrains-mono-nerd`
22. to setup screen-sharing simply do:
    ```
    sudo pacman -S xdg-desktop-portal-hyprland
    ```
	and test using the pipewire source. the xdph will automatically start of hyprland start. simply reboot or do the chad way: `killall hyprland` and tty back to it. Test can be done using obs-studio (hopefully gmeet also works...)

23. Now for the basic apps: file manager, text editor, video player, document viewer...
    1. file manager: i will be going with nautilus, have used in past like the minimalism there...
    2. text editor: again going with gedit
    3. doc viewer: evince has worked well in the past, can use that
    4. video player: still think mpv
    5. office apps: libre is good, not needed for now
24. theming the gtk apps. while earlier this was a pain, now just added stuff to hyprland.conf and we can stow the assets via the repo. I have installed nwg-look but not sure what utility that is bringing... will delete and check. damn it, nautilus is creating problem. Scratch that, libadwaita again is a pain. however this time i did a stow from gtk-themes directly onto the theme provided config folders for gtk-2.0, gtk-3.0 and gtk-4.0. Nothing can/should break that. Also i have stowed the profile just in case that could also be triggered if required.
25. next up, power configs. So i am on an asus "laptop". So essentially i need two things. one is the asusctl/supergfxctl for the asus specific things. And i will be using tlp for laptop based power mgmt. This time we'll also be using power daemon to our control...
	1. asusctl/supergfxctl: should be simple. just yay both of them
	2. tlp: `sudo pacman -S tlp tlp-pd tlp-rdw` . it comes with a base config preset for settings which i found good enough. For now i'll be keeping it to that. 
26. added vscode and git configs. We need to install gnome-keyring in order for git credential store to store our one time PAT for all personal gitops. Also adding config for vscode as it also requires to store an OSS keyring for its git usage in the same password store.
27.  

