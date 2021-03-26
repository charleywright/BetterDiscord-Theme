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
yay -S google-chrome --noconfirm
```

### ZSH
```bash
> ~/.zshrc
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Terminator
```bash
sudo pacman -S terminator
wget https://raw.githubusercontent.com/charleywright/Linux-Scripts/master/Terminator/config -O ~/.config/terminator/config
```
