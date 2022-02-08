# Arch Linux Post-Installation Tweaks

## Select a language if unable to open Terminal:
Open ```Settings```, go to ```Region and Language```, click ```Language``` and select ```English```.

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
sudo pacman -S mesa xf86-video-amdpgu vulkan-radeon libva-mesa-driver mesa-vdpau
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
cd .. | rm -rf yay
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
Run ```dconf-editor```, search for ```automount``` and disable both results.
```
yay -S ttf-ms-win10-auto
```
Run ```dconf-editor``` and enable ```automount``` options.

## Install ```OnlyOffice```:
```
yay -S onlyoffice-bin
```

## Install Mac OS theme:
```
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git
```
```
cd WhiteSur-gtk-theme
```
```
sudo chmod +x ./install.sh | ./install.sh
```
```
cd .. | rm -rf WhiteSur-gtk-theme
```
```
git clone https://github.com/btd1337/La-Sierra-Icon-Theme ~/.icons/La-Sierra
```
