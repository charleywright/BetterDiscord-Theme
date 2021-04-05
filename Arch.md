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
sudo pacman -Sy zsh
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
sudo pacman -S chrome-gnome-shell
dconf write /com/ftpix/transparentbar/transparency 0
```
* [Transparent top bar](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)
* [User-themes](https://extensions.gnome.org/extension/19/user-themes/)
