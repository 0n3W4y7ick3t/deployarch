# deployarch

## fstab example
```
UUID=a9a9be08-f60f-457f-90c6-6af92fb51e8f /            btrfs noatime,ssd,rw,relatime,space_cache=v2	       0 0
# owner set to comman user(uid=1000,gid=1000), directory mode 1744, file mode 0644
UUID=38AB08CE23021498                     /home/media  ntfs  noatime,ssd,dmask=033,fmask=133,uid=1000,gid=1000 0 0
tmpfs                                     /tmp         tmpfs noatime                                           0 0
```
## bootloader
use refind, just copy refind directory to any FAT16/32 partition and you are good to go. btrfs,ext4 drivers are all ready installed.
```
some partition root/EFI/refind/+
 - refind/
 - refind.conf
```
It can detect os through partition labels, and show coresponding logo.

## runit services
`sudo pacman -S dhcpcd-runit wpa_supplicant-runit`
### enable them

```
ln -s /etc/runit/sv/dhcpcd /etc/runit/runsvdir/default
ln -s /etc/runit/sv/wpa_supplicant /etc/runit/runsvdir/default
```
### genarate a wifi profile:
`wpa_passphrase SSID password >> /etc/wpa_supplicant/wpa_supplicant.conf`
now reboot to your new os, or use `sv start wpa_supplicant` to connect wifi and continue

## setup pacman
```
Color
ILoveCandy
ParallelDownloads = 8
```

## sign keyring(if you didn't enable repos other than artix's offical ones, skip this):
1. make changes in /etc/pacman.conf:
`SigLevel  = TrustAll`
2. `pacman -Syy artix-keyring archlinux-keyring`, after step 2, this should not return any signature error.
3. change SigLevel back to original, i.e. SigLevel = Required DatabaseOptional


## media
```
sudo pacman -S mpc mpd mpv ncmpcpp pipewire-pulse pulsemixer pamixer ueberzug lf mediainfo poppler ffmpegthumbnailer sxiv feh
```
## system
```
sudo pacman -S btrfs-progs ntfs-3g dunst dhcpcd gnome-keyring libnotify man-db  polkit wpa_supplicant
```

## X
```
sudo pacman -S arandr python-qdarkstyle ttf-hack ttf-fira-code noto-fonts-emoji xorg-server xorg-xwininfo xorg-xprop xorg-xdpyinfo xorg-xbacklight xorg-xinput
```

## utility
```
sudo pacman -S atool bat bc copyq fzf lynx maim moreutils socat firefox unclutter xcape xclip xdotool zip unzip zathura zathura-pdf-mupdf
```
## AUR
```
yay -S zsh-plugin-wd-git zsh-fast-syntax-highlighting zsh-autosuggestions
yay -S htop-vim picom-git
```

## dwm, dwmblocks, st, dmenu
compile from git repo, you should have all dependencies, after installed mpv.

## misc
`sudo nvim /etc/hosts`
127.0.0.1  localhost

`sudo nvim /etc/runit/runsvdir/default/agetty-tty1/conf`
set `GETTY_ARGS="--noclear --autologin username"`

## fcitx5
`sudo pacman -S fcitx5 fcitx5-gtk fcitx5-configtool`

You must install fcitx5-gtk, otherwise GTK based application will not be able to use fcitx5, such as firefox.
export environment variables:
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```
set to fcitx5 is also ok
