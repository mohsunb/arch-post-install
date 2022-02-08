# Arch Linux Post-Installation Tweaks

## Select a language if unable to open Terminal:
Open ```Settings```, go to ```Region and Language```, click ```Language``` and select ```English```.

## Create a keyboard shortcut for Terminal:
Open ```Settings```, ```Keyboard```. Click on ```View and Customize Shortcuts```. Create a custom shortcut of your choice which executes ```gnome-terminal```.

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

## Installing GNOME extensions:
```
sudo pacman -S chrome-gnome-shell
```
Go to https://extensions.gnome.org/ and install the browser extension. Then search for desired extensions and install them with the toggle.
* Dash To Dock
* Clipboard Indicator
* Tray Icons

## Install ```Mac OS``` theme and icons:
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

# Playing ```Windows``` videogames the "unethical" way:
```
sudo pacman -S lutris wine wine-gecko wine-mono
```
* Open ```Lutris```.
* Click the ```+``` icon at the top left corner.
* From ```Game info```, click on ```Runner``` and select ```Wine```.
* From ```Game options```, select the installer executable. Click on ```Wine prefix``` and select ```~/Lutris```.
* From ```System options```, click ```Default installation folder``` and select ```~/Lutris```.
* Save and click ```Play```.
* Once the game is installed, change the executable to ```Redist``` contents and attempt to install them.
* Finally, select the game executable and click ```Play```.

**NOTE**: Repacks by ```Fitgirl``` cannot be installed as they require sophisticated archiving algorithms and cannot operate in ```Wine```.
