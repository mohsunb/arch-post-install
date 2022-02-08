# Arch Linux Post-Installation Tweaks

## Select a language if unable to open Terminal:
Open ```Settings```, go to ```Region and Language```, click ```Language``` and select ```English```.

## Enable Multilib repository:
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

## Install AUR packages:
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
