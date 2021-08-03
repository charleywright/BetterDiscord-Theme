[Skip to software](#software)

# Setup, Mostly from [ItsFoss Guide](https://itsfoss.com/install-arch-linux/)
### Set keyboard layout for setup
```bash
ls /usr/share/kbd/keymaps/**/*.map.gz           # List all keyboard layouts
loadkeys uk
```

### Partition Disk
```bash
fdisk -l                                        # List disks
fdisk /dev/nvme0n1                              # My disk, yours might be different
  # Delete all partitions, repeat for every partition on the disk, Windows normally has 4, Linux normally has 2 or 3
  d
  
  # Create EFI partition
  n
  Leave as Default
  Leave as Default
  +512M
  Y
  
  # Change partition type to EFI System
  t
  1
  
  # Create root partition (no home or swap because it's easier)
  n
  Leave as Default
  Leave as Default
  Leave as Default
  
  # Write to disk
  w
```

### Create filesystem. Partitions might be different
```bash
mkfs.fat -F32 /dev/nvme0n1p1
mkfs.ext4 /dev/nvme0n1p2
```

### Connect to WiFi. This is a lot of effort, I recommend using powerline because it is WAY easier and faster
* [WiFi](https://wiki.archlinux.org/title/Iwd#iwctl)
* [Mobile Broadband](https://wiki.archlinux.org/title/Mobile_broadband_modem#ModemManager)

### Update mirrors for your location
```bash
pacman -Syy
pacman -S reflector --noconfirm
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak                # Backup incase something goes wrong
reflector -c "GB" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist
```

### Install Arch. Partition might be different, should be the ext4 (2nd) partition
```bash
mount /dev/nvme0n1p2 /mnt
pacstrap /mnt base linux linux-firmware vim nano
```

### Configure
```bash
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

### Timezone
```bash
timedatectl list-timezones
timedatectl set-timezone Europe/London
```

### Locale
```bash
# Either open the file in nano or use `sed` to uncomment your locale
less /etc/locale.gen
sed -i -e 's/#en_GB.UTF-8/en_GB.UTF-8/' /etc/locate.gen

# Generate Config
locale-gen
echo LANG=en_GB.UTF-8 > /etc/locale.conf
export LANG=en_GB.UTF-8
```

### Network
```bash
# Set hostname, use whatever you want
echo HP-Spectre-X360 > /etc/hostname
echo $'127.0.0.1\tlocalhost\n::1\t\tlocalhost\n127.0.1.1\t'$(cat /etc/hostname) > /etc/hosts
```

### Set root password
```bash
passwd
```

### Install GRUB Bootloader
```bash
pacman -S grub efibootmgr --noconfirm
mkdir /boot/efi
mount /dev/nvme0n1p1 /boot/efi
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```

### Install Xorg & Gnome. They won't be enabled
```bash
pacman -S xorg gnome networkmanager --noconfirm     # This might take a while
exit                                                # Exit chroot
shutdown now
```

### Unplug your USB here, then turn on your computer. You will need to login, the username is `root`, password is what you set earlier

### Enable Gnome
```bash
systemctl enable gdm
systemctl enable NetworkManager
systemctl enable bluetooth
reboot
```

### Install sudo and create user 
```bash
pacman -S sudo --noconfirm
sed -i -e 's/# %wheel ALL=(ALL) ALL/%wheel ALL=(ALL) ALL/' /etc/sudoers       # Please don't kill me for using sed on /etc/sudoers. visudo is hard to explain
useradd -G wheel -m charley                                                   # Set the username to what you want
passwd charley                                                                # Set password for your user
reboot
```

# Software
### Yay
```bash
sudo pacman -S git
cd ~/Downloads
sudo pacman -S --needed base-devel --noconfirm
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si --noconfirm
```

### Useful stuff
```bash
cat "keyserver keyserver.ubuntu.com" > ~/.gnupg/gpg.conf
multilib=`grep -n "\[multilib\]" /etc/pacman.conf | cut -f1 -d:`
sudo sed -i "$multilib s/^#//g" /etc/pacman.conf
multilib=$(( $multilib + 1 ))
sudo sed -i "$multilib s/^#//g" /etc/pacman.conf
sudo pacman -Sy
yay -S google-chrome neofetch wget curl spotify discord dotnet-sdk docker vlc ntfs-3g steam steam-fonts appimagelauncher alsa-firmware sof-firmware alsa-ucm-conf libappindicator-gtk3 noto-fonts noto-fonts-extra noto-fonts-emoji noto-fonts-cjk plank vlc handbrake
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
```

### Zsh/Oh My ZSH
```bash
yay -S zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
```bash
sudo git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
sudo git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
sed -i -e 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/g' ~/.zshrc
sed -i -e "s/plugins=(git)/plugins=(docker zsh-autosuggestions zsh-syntax-highlighting)/g" ~/.zshrc
echo "zstyle ':completion:*:*:docker:*' option-stacking yes\nzstyle ':completion:*:*:docker-*:*' option-stacking yes" >> ~/.zshrc
source ~/.zshrc
```

### Terminator
```bash
yay -S terminator nerd-fonts-hack
mkdir -p ~/.config/terminator/
wget https://raw.githubusercontent.com/charleywright/Linux-Scripts/master/Terminator/config -O ~/.config/terminator/config
```

### Nodejs
```bash
yay -S nodejs npm
mkdir ~/npm
npm config set prefix ~/npm
echo "export PATH=\"$PATH:$HOME/npm/bin\"" >> ~/.zshrc
```

### VSCode
```bash
yay -S code
wget https://raw.githubusercontent.com/charleywright/Linux-Scripts/master/VSCode/Keybinds.json -O ~/.config/Code/User/keybindings.json
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
code --install-extension rvest.vs-code-prettier-eslint
code --install-extension ritwickdey.liveserver
code --install-extension octref.vetur
code --install-extension vuetifyjs.vuetify-vscode
code --install-extension zhuangtongfa.material-theme
code --install-extension pkief.material-icon-theme
code --install-extension xabikos.javascriptsnippets
code --install-extension icrawl.discord-vscode
code --install-extension formulahendry.auto-rename-tag
code --install-extension formulahendry.auto-close-tag
code --install-extension ms-dotnettools.csharp # Only if you have already installed the dotnet CLI
mkdir -p ~/.config/Code/User
echo "{\"workbench.iconTheme\": \"material-icon-theme\",\"workbench.colorTheme\": \"One Dark Pro\"}" > ~/.config/Code/User/settings.json
```

### BetterGnome
```bash
sudo sed -i "s/#WaylandEnable=false/WaylandEnable=false/g" /etc/gdm/custom.conf
sudo systemctl restart gdm
```
```bash
yay -S gnome-shell-extensions gnome-tweaks papirus-icon-theme chrome-gnome-shell filemanager-actions
dconf write /com/ftpix/transparentbar/transparency 0
dconf write /org/gnome/desktop/interface/icon-theme "'Papirus-Dark'"
dconf write /org/gnome/desktop/interface/text-scaling-factor 0.80
mkdir -p ~/.themes
wget "https://github.com/EliverLara/Nordic/releases/latest/download/Nordic-darker.tar.xz" -O ~/.themes/nordic-darker.tar.xz
tar xf ~/.themes/nordic-darker.tar.xz -C ~/.themes/
gsettings set org.gnome.desktop.wm.preferences theme Nordic-darker
gsettings set org.gnome.desktop.interface gtk-theme Nordic-darker
gsettings set org.gnome.shell disable-extension-version-validation "true"
gsettings set org.gnome.desktop.peripherals.touchpad click-method areas
gsettings set org.gnome.desktop.interface enable-hot-corners false
```
* [Transparent top bar](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)
* [User-themes](https://extensions.gnome.org/extension/19/user-themes/)
* [KStatusNotifierItem/AppIndicator Support](https://extensions.gnome.org/extension/615/appindicator-support/)

### [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
```bash
dconf write /org/gnome/shell/extensions/dash-to-dock/preferred-monitor 0
dconf write /org/gnome/shell/extensions/dash-to-dock/dock-position "'BOTTOM'"
dconf write /org/gnome/shell/extensions/dash-to-dock/dash-max-icon-size 32
dconf write /org/gnome/shell/extensions/dash-to-dock/show-show-apps-button false
dconf write /org/gnome/shell/extensions/dash-to-dock/show-trash false
dconf write /org/gnome/shell/extensions/dash-to-dock/custom-theme-shrink true
dconf write /org/gnome/shell/extensions/dash-to-dock/transparency-mode "'FIXED'"
dconf write /org/gnome/shell/extensions/dash-to-dock/background-opacity 0.0
```

### Correct monitor config on Gnome login screen
```bash
sudo cp -v ~/.config/monitors.xml /var/lib/gdm/.config/
sudo chown gdm:gdm /var/lib/gdm/.config/monitors.xml
```

### Snap, Doctl & Kubectl
```bash
yay -S snapd
sudo systemctl enable snapd.service
sudo systemctl start snapd.service
sudo ln -s /var/lib/snapd/snap /snap
echo "export PATH=\"$PATH:/snap/bin\"" >> ~/.zshrc
```
```bash
sudo snap install kubectl --classic
sudo snap install doctl
mkdir ~/.kube
sudo snap connect doctl:kube-config
sudo snap connect doctl:dot-docker
```
```bash
doctl auth init
doctl registry login
doctl kubernetes cluster kubeconfig save CLUSTER_ID
```

### Flatpak & Hydrapaper
```bash
yay -S flatpak
flatpak install org.gabmus.hydrapaper
flatpak run org.gabmus.hydrapaper
```


### Sync snap apps on login (Add to .bashrc or .zshrc)
```bash
for i in /var/lib/snapd/desktop/applications/*.desktop; do
    if [ ! -f ~/.local/share/applications/${i##*/} ];then
            ln -s /var/lib/snapd/desktop/applications/${i##*/} ~/.local/share/applications/${i##*/};
    fi;
done
```

### Microphone not showing in Gnome settings
Run with device unplugged, then run again after plugging it in: 
```bash
pacmd list-sources | grep 'name:.*input'
```
Find the name that appears after plugging the device in, for me that was `alsa_input.usb-Burr-Brown_from_TI_USB_Audio_CODEC-00.analog-stereo-input`. Now append these lines to `/etc/pulse/default.pa`, where DEVICE_NAME is the name from the previous step:
```bash
load-module module-remap-source source_name=record_mono master=DEVICE_NAME master_channel_map=front-left chan>
set-default-source record_mono
```
