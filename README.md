# Arch Linux Post-Installation Tweaks

## Create a keyboard shortcut for Terminal:
Open ```Settings```, ```Keyboard```. Click on ```View and Customize Shortcuts```. Create a custom shortcut of your choice which executes ```gnome-terminal```.

## Install Bluetooth drivers:
```
sudo pacman -S bluez bluez-utils
```
```
sudo systemctl enable bluetooth
```
```
sudo systemctl start bluetooth
```

## Enable ```Multilib``` repository:
Edit ```/etc/pacman.conf``` and uncomment:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

## Switch ```Wayland``` to ```X11```:
Edit ```/etc/gdm/custom.conf``` and uncomment:
```
WaylandEnable=false
```

## Install ```AMDGPU``` drivers:
```
sudo pacman -S mesa xf86-video-amdgpu vulkan-radeon libva-mesa-driver mesa-vdpau
```
For 32-bit support:
```
sudo pacman -S lib32-mesa lib32-vulkan-radeon lib32-libva-mesa-driver lib32-mesa-vdpau
```

## Enable Variable Refresh Rate:
Create ```/etc/X11/xorg.conf.d/20-amdgpu.conf``` and enter:
```
Section "Device"
     Identifier "AMD"
     Driver "amdgpu"
     Option "VariableRefresh" "true"
EndSection
```

## Install ```AUR``` packages:
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

Now use ```yay -S PACKAGE_NAME``` to install AUR packages.

## Make terminal transparent:

```
yay -S gnome-terminal-transparency
```

Open ```Terminal```. Click three dots, ```Preferences```. Select your profile. Go to ```Colors``` and tick ```Transparent background```. Use the slider to adjust transparency.

## Install ```Z-Shell```, ```Oh-My-Zsh``` and ```Starship Prompt```:
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

## Install ```Microsoft Windows``` fonts:
```
sudo pacman -S dconf-editor
```
Run ```dconf-editor```, search for ```automount``` and disable both results.
```
yay -S ttf-ms-win10-auto
```
Run ```dconf-editor``` and enable ```automount``` options.

## Install ```OnlyOffice```:
```
yay -S onlyoffice-bin
```

## Installing GNOME extensions:
```
yay -S chrome-gnome-shell
```
Go to https://extensions.gnome.org/ and install the browser extension. Then search for desired extensions and install them with the toggle.
* Dash To Dock
* Clipboard Indicator
* Tray Icons: Reloaded

## Install ```Mac OS``` theme and icons:
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
Run ```Tweaks```, go to ```Appearance```. Click on ```Applications``` drop down menu and select ```WhiteSur-dark```. Click on ``Icons`` drop down menu and select ```La-Sierra```.

## Slideshow wallpapers:
```
sudo pacman -S shotwell
```

## Advanced screenshot tool - ```Flameshot```:
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

## Better thumbnails and fix broken ones:
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
* Restart Nautilus:
```
nautilus -q
```

## Set up Java programming:
```
sudo pacman -S jre jdk code
```
