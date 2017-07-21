<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#org5dce095">1. Partition</a></li>
<li><a href="#orgdaff809">2. Install</a></li>
<li><a href="#org0221ecd">3. Configure</a></li>
<li><a href="#org1a38d67">4. After Installation</a></li>
</ul>
</div>
</div>
Just a note of Arch Linux installation.


<a id="org5dce095"></a>

# Partition

    
    fdisk -l
    
    cfdisk /dev/sdX
    
    mkfs.ext4 /dev/sdXI
    
    mkfs.vfat -F32 /dev/sdXK
    
    mkswap /dev/sdXJ
    
    swapon /dev/sdXJ
    
    mount /dev/sdXI /mnt
    
    mkdir /mnt/boot
    
    mount /dev/sdXK /mnt/boot

Learn more about partition: <https://wiki.archlinux.org/index.php/Partitioning>


<a id="orgdaff809"></a>

# Install

    
    pacstrap /mnt base base-devel
    
    genfstab -U /mnt >> /mnt/etc/fstab

Learn more about installation: <https://wiki.archlinux.org/index.php/installation_guide>


<a id="org0221ecd"></a>

# Configure

    
    arch-chroot /mnt
    
    ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
    
    hwclock --systohc
    
    nano /etc/locale.gen # uncomment en_US.UTF-8 UTF-8
    
    locale-gen
    
    echo LANG=en_US.UTF-8 > /etc/locale.conf
    
    echo "my_hostname" > /etc/hostname # replace "my_hostname" to the hostname you want
    
    nano /etc/hosts # add "127.0.1.1 my_hostname.localdomain my_hostname" below ::1
    
    passwd
    
    pacman -S dosfstools
    
    pacman -S os-prober
    
    pacman -S grub efibootmgr
    
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
    
    pacman -S intel-ucode # if using intel cpu
    
    grub-mkconfig -o /boot/grub/grub.cfg
    
    useradd -m -g users "username" # add new user named "username"
    
    passwd "username"
    
    nano /etc/sudoers # add "username ALL=(ALL) ALL"
    
    mkinitcpio -p linux
    
    exit
    
    umount -R /mnt
    
    reboot # After doing this, the os installation is done.

Learn more about grub settings: <https://wiki.archlinux.org/index.php/GRUB>


<a id="org1a38d67"></a>

# After Installation

    
    # First, login to os.
    
    sudo bash
    
    systemctl enable dhcpcd.service # if using wired connection and login to OS
    
    ping 168.95.1.1 # check network reachable
    
    pacman -Syu # synchronize package databases
    
    
    # Prepare to install GUI, I use KDE.
    
    pacman -Syu xorg xterm xorg-twm xorg-xinit xorg-server
    
    pacman -Syyu
    
    pacman -S plasma-meta kdebase # install KDE Plasma
    
    pacman -S ttf-freefont
    
    systemctl enable sddm.service
    
    
    # Complete network tools by using networkmanager
    
    pacman -S networkmanager plasma-nm
    
    systemctl stop dhcpcd.service
    
    systemctl disable dhcpcd.service
    
    systemctl enable NetworkManager
    
    systemctl start NetworkManager
    
    pacman -S openconnect networkmanager-openconnect # if need vpn
    
    
    # Install apps
    
    # Just install whatever app you want...
    pacman -S ark firefox qt4 gimp libreoffice flashplugin filezilla chromium emacs atom git
    
    pacman -S archlinux-wallpaper
    
    reboot
    
    
    # After login to GUI OS
    
    sudo pacman -S gtkmm net-tools flatpak
    
    
    # AUR
    
    mkdir ~/builds # just a place to store aur package
    
    cd ~/builds
    
    git clone htps://aur.archlinux.org/google-chrome.git # google-chrome as example
    
    cd google-chrome
    
    nano PKGBUILD # usually do this to check code, can ignore this
    
    nano google-chrome.install # usuall do this to check code, can ignore this
    
    makepkg -s
    
    sudo pacman -U google-chrome-version.pkg.tar.xz

Learn more about AUR: <https://aur.archlinux.org/>

