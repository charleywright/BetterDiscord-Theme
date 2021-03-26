
### Download first
* [VSCode Deb](https://code.visualstudio.com/docs/?dv=linux64_deb)
* [Chrome Deb](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)
* [Discord Deb](https://discord.com/api/download?platform=linux&format=deb)
* [Steam Deb](https://cdn.akamai.steamstatic.com/client/installer/steam.deb)

### General stuff
```bash
cd ~/Downloads
sudo apt install curl git -y
sudo dpkg -i google-chrome-stable_current_amd64.deb code* steam*
sudo apt --fix-broken install -y
sudo dpkg -i steam*
```

### BetterDiscord
```bash
sudo dpkg -i discord*
sudo apt --fix-broken install -y
sudo dpkg -i discord*
curl -O https://raw.githubusercontent.com/bb010g/betterdiscordctl/master/betterdiscordctl
chmod +x betterdiscordctl
sudo mv betterdiscordctl /usr/local/bin
betterdiscordctl install
mkdir -p ~/.config/BetterDiscord/plugins ~/.config/BetterDiscord/themes
git clone https://github.com/charleywright/Linux-Scripts.git
cp Linux-Scripts/BetterDiscord/Themes/* ~/.config/BetterDiscord/themes/
cp Linux-Scripts/BetterDiscord/Plugins/* ~/.config/BetterDiscord/plugins/
rm -Rf Linux-Scripts
```

### Spicetify
```bash
curl -sS https://download.spotify.com/debian/pubkey_0D811D58.gpg | sudo apt-key add -
echo "deb http://repository.spotify.com stable non-free" | sudo tee /etc/apt/sources.list.d/spotify.list
sudo apt update && sudo apt install spotify-client -y
```
#### Run spotify, and log in, then continue
```bash
sudo chmod a+wr /usr/share/spotify
sudo chmod a+wr /usr/share/spotify/Apps -R
curl -fsSL https://raw.githubusercontent.com/khanhas/spicetify-cli/master/install.sh | sh
mkdir -p $(dirname "$(~/spicetify-cli/spicetify -c)")/Extensions
git clone https://github.com/charleywright/Linux-Scripts.git
cp Themes/Spicetify/* "$(dirname "$(~/spicetify-cli/spicetify -c)")/Themes" -r
cp "$(dirname "$(~/spicetify-cli/spicetify -c)")/Themes/Dribbblish/dribbblish.js" "$(dirname "$(~/spicetify-cli/spicetify -c)")/Extensions"
~/spicetify-cli/spicetify config extensions dribbblish.js
~/spicetify-cli/spicetify config current_theme Dribbblish color_scheme base
~/spicetify-cli/spicetify config inject_css 1 replace_colors 1 overwrite_assets 1
~/spicetify-cli/spicetify backup apply
rm -Rf Themes
```

### ZSH
```bash
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
```bash
sudo git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
sudo git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
sed -i -e 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/g' ~/.zshrc
sed -i -e "s/plugins=(git)/plugins=(docker zsh-autosuggestions zsh-syntax-highlighting)/g" ~/.zshrc
echo "zstyle ':completion:*:*:docker:*' option-stacking yes\nzstyle ':completion:*:*:docker-*:*' option-stacking yes" >> ~/.zshrc
sudo apt install fonts-hack -y
source ~/.zshrc
```

### Hide menubar in Konsole
```bash
echo "\n[KonsoleWindow]\nShowMenuBarByDefault=false" >> ~/.config/konsolerc
```

### Dotnet
```bash
cd ~/Downloads
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
sudo apt update
sudo apt install -y apt-transport-https
sudo apt update
sudo apt install -y dotnet-sdk-5.0

```

### Nodejs
```bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install nodejs yarn -y
mkdir ~/npm
npm config set prefix ~/npm
echo "export PATH=\"$PATH:$HOME/npm/bin\"" >> ~/.zshrc
```

### VSCode
```bash
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
echo "{\"workbench.iconTheme\": \"material-icon-theme\",\"workbench.colorTheme\": \"One Dark Pro\"}" > ~/.config/Code/User/settings.json
```

### Lutris
```bash
sudo add-apt-repository ppa:lutris-team/lutris -y
sudo apt update
sudo apt install lutris -y
```

### Gcloud SDK
```bash
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
sudo apt install apt-transport-https ca-certificates gnupg -y
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt update && sudo apt install google-cloud-sdk -y
gcloud init
```

### Sync snap apps on login (Add to .bashrc or .zshrc)
```bash
for i in /var/lib/snapd/desktop/applications/*.desktop; do
    if [ ! -f ~/.local/share/applications/${i##*/} ];then
            ln -s /var/lib/snapd/desktop/applications/${i##*/} ~/.local/share/applications/${i##*/};
    fi;
done
```

### Docker
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
# sudo groupadd docker
sudo usermod -aG docker $USER
```


### Doctl & kubectl
```bash
sudo snap install kubectl --classic
sudo snap install doctl
mkdir ~/.kube
sudo snap connect doctl:kube-config
sudo snap connect doctl:dot-docker
```

### BetterGnome
```bash
sudo apt install gnome-tweaks gnome-shell-extensions papirus-icon-theme plank -y
mkdir -p ~/.themes
wget "https://github.com/EliverLara/Nordic/releases/latest/download/Nordic-darker.tar.xz" -O ~/.themes/nordic-darker.tar.xz
tar xf ~/.themes/nordic-darker.tar.xz -C ~/.themes/
gsettings set org.gnome.desktop.wm.preferences theme Nordic-darker
gsettings set org.gnome.desktop.interface gtk-theme Nordic-darker
dconf write /org/gnome/desktop/interface/text-scaling-factor 0.80
```

#### Shell Extensions
* [Transparent top bar](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)
* [User-themes](https://extensions.gnome.org/extension/19/user-themes/)
* [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
