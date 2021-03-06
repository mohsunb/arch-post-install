# Arch Linux Post-Installation Tweaks

# Table of contents:
* [Alacritty Gnome Wayland](#alacritty-wayland-launch-command-gnome-shell)
* [doas](#switch-sudo-to-doas)
* [Gnome Wayland](#gnome-shell-switch-wayland-to-x11)
* [CPU uCode](#cpu-microcode-update)
* [AMDGPU](#install-amdgpu-drivers)
* [X11 VRR](#x11-enable-variable-refresh-rate)
* [YAY](#aur-helper)
* [AUR SMT](#makepkg-multithreading)
* [Shell](#custom-shell)
* [Win font](#windows-fonts)
* [Gnome addons](#gnome-shell-extensions)
* [Gnome Mac OS](#gnome-shell-mac-os-theme)
* [Gnome slideshow](#gnome-shell-42-slideshow)
* [Flameshot](#advanced-screenshot-tool---flameshot)
* [Customize GDM](#customize-gdm-login-screen-background)
* [Keychron FN](#fix-for-keychron-function-keys)
* [Steam libc error](#if-steam-breaks-saying-libcso6-no-found)
* [Gnome thumbnails](#better-thumbnails-gnome-shell)
* [Firefox Wayland](#firefox-wayland)
* [Discord Wayland](#discord-wayland)
* [Dolphin thumbnails](#dolphin-video-thumbnails)
* [tar/gzip](#tar-arguments)

## ```Alacritty``` Wayland launch command (GNOME Shell):
```
env WAYLAND_DISPLAY=alacritty alacritty
```

## Switch ```sudo``` to ```doas```:
```
yay -S opendoas
```
Create ```/etc/doas.conf``` and enter: (must end with a newline)
```
permit persist :wheel

```
```
sudo chown -c root:root /etc/doas.conf
```
```
sudo chmod -c 0400 /etc/doas.conf
```
Create aliases:
```
alias sudo='doas'
alias sudoedit='doas rnano'
```
Make ```yay``` use ```doas```:
```
yay --sudo doas --save
```

## **GNOME Shell**: Switch ```Wayland``` to ```X11```:
Edit ```/etc/gdm/custom.conf``` and uncomment:
```
WaylandEnable=false
```

## CPU microcode update:
```
sudo pacman -S amd-ucode
```
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Install ```AMDGPU``` drivers:
```
sudo pacman -S mesa xf86-video-amdgpu vulkan-radeon libva-mesa-driver mesa-vdpau
```
For 32-bit support:
```
sudo pacman -S lib32-mesa lib32-vulkan-radeon lib32-libva-mesa-driver lib32-mesa-vdpau
```

## X11: Enable Variable Refresh Rate:
Create ```/etc/X11/xorg.conf.d/20-amdgpu.conf``` and enter:
```
Section "Device"
     Identifier "AMD"
     Driver "amdgpu"
     Option "VariableRefresh" "true"
EndSection
```

## ```AUR``` helper:
```
sudo pacman -S base-devel
```

```
git clone https://aur.archlinux.org/yay.git
```
```
cd yay
```
```
makepkg -si
```
```
cd ..
```
```
rm -rf yay
```

## MAKEPKG multithreading:
Edit ```/etc/makepkg.conf``` and set:
```
MAKEFLAGS="-j$(nproc)"
```

## Custom shell:
```
sudo pacman -S zsh
```
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
```
sh -c "$(curl -fsSL https://starship.rs/install.sh)"
```
Edit ```~/.zshrc``` and append:
```
eval "$(starship init zsh)"
```

## ```Windows``` fonts:
Note: disabling ```automount``` is only necessary on GNOME Shell.
```
sudo pacman -S dconf-editor
```
Run ```dconf-editor```, search for ```automount``` and disable both results.
```
yay -S ttf-ms-win11-auto
```
Run ```dconf-editor``` and enable ```automount``` options.

## GNOME Shell extensions:
```
yay -S chrome-gnome-shell
```
https://extensions.gnome.org/
* Dash To Dock
* Clipboard Indicator
* Tray Icons

## GNOME Shell: ```Mac OS``` theme:
```
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git
```
```
cd WhiteSur-gtk-theme
```
```
sudo chmod +x ./install.sh
```
```
./install.sh
```
```
cd ..
```
```
rm -rf WhiteSur-gtk-theme
```
```
git clone https://github.com/btd1337/La-Sierra-Icon-Theme ~/.icons/La-Sierra
```

## GNOME Shell 42>= slideshow:
```
sudo pacman -S variety
```

## Advanced screenshot tool - ```Flameshot```:
Note: Doesn't work on Gnome, Wayland. Works on KDE, Wayland, but buggy.
```
sudo pacman -S flameshot
```
Open ```Settings```, ```Keyboard```. Click on ```View and Customize Shortcuts```. Click ```Screenshots``` and disable ```Print``` key function.
Then create a custom shortcut for ```Print``` key which executes ```flameshot gui```.

## Customize GDM login screen background:
```
yay -S gdm-tools
```
```
set-gdm-theme -s default LOCATION_OF_IMAGE
```

## Fix for ```Keychron``` function keys:
```
echo 0 | sudo tee /sys/module/hid_apple/parameters/fnmode
```
```
echo "options hid_apple fnmode=0" | sudo tee -a /etc/modprobe.d/hid_apple.conf
```
```
sudo mkinitcpio -P
```

## If Steam breaks saying ```libc.so.6 no found```:
* Uninstall Steam
* Delete ```~/.steam```, ```~/.steampath```, ```~/.steampid```, ```~/.local/share/Steam```;
* Delete ```libc.so.6``` link:
```
sudo rm -rf /usr/lib32/libc.so.6
```
* Reinstall the dependencies and then Steam:
```
sudo pacman -S glibc lib32-glibc
```

## Better thumbnails (GNOME Shell):
```
yay -S ffmpegthumbnailer
```
* Edit ```/usr/share/thumbnailers/totem.thumbnailer``` and replace all text with:
```
[Thumbnailer Entry]
TryExec=ffmpegthumbnailer
Exec=ffmpegthumbnailer -s %s -i %i -o %o -c png -f -t 10
MimeType=application/mxf;application/ogg;application/ram;application/sdp;application/vnd.ms-wpl;application/vnd.rn-realmedia;application/x-extension-m4a;application/x-extension-mp4;application/x-flash-video;application/x-matroska;application/x-netshow-channel;application/x-ogg;application/x-quicktimeplayer;application/x-shorten;image/vnd.rn-realpix;image/x-pict;misc/ultravox;text/x-google-video-pointer;video/3gpp;video/dv;video/fli;video/flv;video/mp2t;video/mp4;video/mp4v-es;video/mpeg;video/msvideo;video/ogg;video/quicktime;video/vivo;video/vnd.divx;video/vnd.rn-realvideo;video/vnd.vivo;video/webm;video/x-anim;video/x-avi;video/x-flc;video/x-fli;video/x-flic;video/x-flv;video/x-m4v;video/x-matroska;video/x-mpeg;video/x-ms-asf;video/x-ms-asx;video/x-msvideo;video/x-ms-wm;video/x-ms-wmv;video/x-ms-wmx;video/x-ms-wvx;video/x-nsv;video/x-ogm+ogg;video/x-theora+ogg;video/x-totem-stream;audio/x-pn-realaudio;audio/3gpp;audio/ac3;audio/AMR;audio/AMR-WB;audio/basic;audio/midi;audio/mp2;audio/mp4;audio/mpeg;audio/ogg;audio/prs.sid;audio/vnd.rn-realaudio;audio/x-aiff;audio/x-ape;audio/x-flac;audio/x-gsm;audio/x-it;audio/x-m4a;audio/x-matroska;audio/x-mod;audio/x-mp3;audio/x-mpeg;audio/x-ms-asf;audio/x-ms-asx;audio/x-ms-wax;audio/x-ms-wma;audio/x-musepack;audio/x-pn-aiff;audio/x-pn-au;audio/x-pn-wav;audio/x-pn-windows-acm;audio/x-realaudio;audio/x-real-audio;audio/x-sbc;audio/x-speex;audio/x-tta;audio/x-wav;audio/x-wavpack;audio/x-vorbis;audio/x-vorbis+ogg;audio/x-xm;application/x-flac;
```
* Clear all thumbnails:
```
rm -r ~/.cache/thumbnails
```

## Firefox Wayland:
* Edit ```/etc/environment``` and enter:
```
MOX_ENABLE_WAYLAND=1
```

## Discord Wayland:
* Edit application icon and enter:
```
Exec="/usr/bin/discord" --enable-features=UseOzonePlatform --ozone-platform=wayland
```

## ```Dolphin``` video thumbnails:
```
sudo pacman -S ffmpegthumbs
```
Go to Dolphin Settings > Configure Dolphin > General > Previews > Tick ```Video Files (ffmpegthumbs)```.

## ```tar``` arguments:
* For ```gzip``` compression:
```
tar -cvzf file_name.tar.gz DIRECTORY_NAME
```
* For extracting:
```
tar -xf file_name.tar.gz
```
