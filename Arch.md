### Yay
```bash
cd ~/Downloads
sudo pacman -S --needed base-devel --noconfirm
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si --noconfirm
```


### Software
```bash
yay -S google-chrome neofetch wget curl spotify discord plank
```

### Zsh/Oh My ZSH
```bash
sudo pacman -S zsh
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
sudo pacman -S terminator
yay -S nerd-fonts-hack
mkdir -p ~/.config/terminator/
wget https://raw.githubusercontent.com/charleywright/Linux-Scripts/master/Terminator/config -O ~/.config/terminator/config
```

### Force Xorg instead of wayland
```bash
sudo sed -i -e 's/#WaylandEnable=false/WaylandEnable=false/g' /etc/gdm/custom.conf
```

### BetterGnome
```bash
yay -S gnome-shell-extensions gnome-tweaks
sudo pacman -S papirus-icon-theme chrome-gnome-shell
dconf write /com/ftpix/transparentbar/transparency 0
dconf write /org/gnome/desktop/interface/icon-theme "'Papirus-Dark'"
dconf write /org/gnome/desktop/interface/text-scaling-factor 0.80
mkdir -p ~/.themes
wget "https://github.com/EliverLara/Nordic/releases/latest/download/Nordic-darker.tar.xz" -O ~/.themes/nordic-darker.tar.xz
tar xf ~/.themes/nordic-darker.tar.xz -C ~/.themes/
gsettings set org.gnome.desktop.wm.preferences theme Nordic-darker
gsettings set org.gnome.desktop.interface gtk-theme Nordic-darker
```
* [Transparent top bar](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)
* [User-themes](https://extensions.gnome.org/extension/19/user-themes/)
