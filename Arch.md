### Yay
```bash
sudo pacman -S git
cd ~/Downloads
sudo pacman -S --needed base-devel --noconfirm
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si --noconfirm
```

### Software
```bash
multilib=`grep -n "\[multilib\]" /etc/pacman.conf | cut -f1 -d:`
sudo sed -i "$multilib s/^#//g" /etc/pacman.conf
multilib=$(( $multilib + 1 ))
sudo sed -i "$multilib s/^#//g" /etc/pacman.conf
sudo pacman -Sy
yay -S google-chrome neofetch wget curl spotify discord dotnet-sdk docker vlc ntfs-3g steam steam-fonts appimagelauncher alsa-firmware sof-firmware alsa-ucm-conf libappindicator-gtk3 noto-fonts noto-fonts-extra noto-fonts-emoji noto-fonts-cjk plank vlc
sudo usermod -aG docker $USER
```

### Zsh/Oh My ZSH
```bash
sudo yay -S zsh
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
yay -S visual-studio-code-bin
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
```
* [Transparent top bar](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)
* [User-themes](https://extensions.gnome.org/extension/19/user-themes/)
* [KStatusNotifierItem/AppIndicator Support](https://extensions.gnome.org/extension/615/appindicator-support/)
* [No Topleft Hot Corner](https://extensions.gnome.org/extension/118/no-topleft-hot-corner/)

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

### Snap, Doctl & Kubectl
```bash
yay -S snapd
systemctl enable snapd.service
systemctl start snapd.service
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
