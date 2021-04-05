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
yay -S google-chrome neofetch wget curl spotify
```

### ZSH
```bash
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Terminator
```bash
sudo pacman -S terminator
yay -S nerd-fonts-hack
mkdir -p ~/.config/terminator/
wget https://raw.githubusercontent.com/charleywright/Linux-Scripts/master/Terminator/config -O ~/.config/terminator/config
```

### BetterGnome
```bash
yay -S gnome-shell-extensions gnome-tweaks
sudo pacman -S papirus-icon-theme chrome-gnome-shell
dconf write /com/ftpix/transparentbar/transparency 0
dconf write /org/gnome/desktop/interface/icon-theme "'Papirus-Dark'"
mkdir -p ~/.themes
wget "https://github.com/EliverLara/Nordic/releases/latest/download/Nordic-darker.tar.xz" -O ~/.themes/nordic-darker.tar.xz
tar xf ~/.themes/nordic-darker.tar.xz -C ~/.themes/
gsettings set org.gnome.desktop.wm.preferences theme Nordic-darker
gsettings set org.gnome.desktop.interface gtk-theme Nordic-darker
```
* [Transparent top bar](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)
* [User-themes](https://extensions.gnome.org/extension/19/user-themes/)
