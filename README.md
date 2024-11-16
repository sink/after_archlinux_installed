# after_archlinux_installed (sk-chimeraos)


## install development base (make autoconf ...)
sudo pacman -S base-devel cmake git

## set display scale 
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"

## install zsh
sudo pacman -S zsh
chsh -s /usr/bin/zsh
sudo sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

## install bluetooth
sudo pacman -S bluez bluez-utils
sudo systemctl enable bluetooth
sudo systemctl start bluetooth

## config git
ssh-keygen -t rsa -C "xx@xx.com"
git config --global user.name xxx
git config --global user.email xx@xx.com
ssh-keygen -t rsa -C "xx@xx.com"
vi ~/.ssh/config

Host github.com
  Hostname ssh.github.com
  Port 443

vi ~/.gitconfig

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
[url "https://ghproxy.cc/https://github.com/"]
        insteadof = https://github.com/

## install yay
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

## install chinese fonts
sudo pacman -S adobe-source-han-sans-otc-fonts
sudo bash -c 'cat << EOF > /etc/fonts/conf.d/64-language-selector-prefer.conf
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Source Han Sans SC</family>
      <family>Source Han Sans TC</family>
      <family>Source Han Sans HW</family>
      <family>Source Han Sans K</family>
    </prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Source Han Sans SC</family>
      <family>Source Han Sans TC</family>
      <family>Source Han Sans HW</family>
      <family>Source Han Sans K</family>
    </prefer>
  </alias>
</fontconfig>
EOF'
fc-cache -fv

## install dash to panel and vitals
sudo pacman -S gnome-shell-extension-dash-to-panel
sudo pacman -S gnome-shell-extension-vitals

## install ibus-rime and frost pinyin
sudo pacman -S ibus-rime
git clone https://ghproxy.cc/https://github.com/gaboolic/rime-frost
cp -r rime-frost/* ~/.config/ibus/rime

## set efi boot order
sudo grub-install /dev/nvmeXXXX
sudo grub-mkconfig -o /boot/grub/grub.cfg

## install steamcommunity_302 for steam deck
cd SteamDeck_302
./install.sh

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
