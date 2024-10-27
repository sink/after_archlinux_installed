# after_archlinux_installed (sk-chimeraos)

## remove console motd
cjust toggle-user-motd

## first unlock immutable system
sudo frzr-unlock

## change pacman mirror and update database
cat "Server=https://mirrors.aliyun.com/archlinux/\$repo/os/\$arch" > /etc/pacman.d/mirrorlist
sudo pacman -Sy

## install development base (make autoconf ...)
sudo pacman -S base-devel
sudo pacman -S cmake

## install dash to panel and vitals
sudo pacman -S gnome-shell-extension-dash-to-panel
sudo pacman -S gnome-shell-extension-vitals

## install cockpit (localhost:9090, system management)
sudo pacman -S cockpit
systemctl enable cockpit.service
systemctl start cockpit.service

## install ibus-rime and frost pinyin
sudo pacman -S ibus-rime
git clone https://ghproxy.cc/https://github.com/gaboolic/rime-frost Rime\n\n
cd Rime
cp -r * ~/.config/ibus/rime

## change zsh theme to robbyrussell and enable archlinux plugin
vi .zshrc
ZSH_THEME="robbyrussell"
plugins=(git sudo z fast-syntax-highlighting archlinux)

## set flatpak mirror
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub
then install microsoft-edge

## set efi boot order
sudo efibootmgr -o 000x,000y,000z

## install steamcommunity_302 for steam deck
cd SteamDeck_302
./install.sh

## install netease-cloud-music
git clone https://aur.archlinux.org/netease-cloud-music.git
cd netease-cloud-music
makepkg -si

## install rust
yay rustup
echo 'export RUSTUP_UPDATE_ROOT=https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup' >> ~/.zshrc
echo 'export RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup' >> ~/.zshrc
rustup default stable

## install nodejs
yay nodejs
yay npm
npm config set registry https://registry.npmmirror.com

## install vscode
yay -S visual-studio-code-bin

## install proton-ge
cd .steam/steam/compatibilitytools.d
wget https://ghproxy.cc/https://github.com/GloriousEggroll/proton-ge-custom/releases/download/GE-Proton9-16/GE-Proton9-16.tar.gz
tar zxf GE-Proton9-16.tar.gz

## git config
ssh-keygen -t rsa -C "xx@xx.com"
vi ~/.ssh/config
```
Host github.com
  Hostname ssh.github.com
  Port 443
```
vi ~/.gitconfig
```
[alias]
    st = status
    co = checkout
    br = branch
    mg = merge
    ci = commit 
    md = commit --amend
    dt = difftool
    mt = mergetool
    last = log -1 HEAD
    cf = config
    line = log --oneline
    latest = for-each-ref --sort=-committerdate --format='%(committerdate:short) %(refname:short) [%(committername)]'
    ls = log --pretty=format:\"%C(yellow)%h %C(blue)%ad %C(red)%d %C(reset)%s %C(green)[%cn]\" --decorate --date=short
    hist = log --pretty=format:\"%C(yellow)%h %C(red)%d %C(reset)%s %C(green)[%an] %C(blue)%ad\" --topo-order --graph --date=short
    type = cat-file -t
    dump = cat-file -p
```