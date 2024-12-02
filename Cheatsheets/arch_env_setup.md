# I'm an arch luser btw

Enable volume overamplification for Gnome:

``` bash
gsettings set org.gnome.desktop.sound allow-volume-above-100-percent 'true'
```

Install needed packages:

``` bash
pacman -S multilib sysfsutils pipewire pipewire-docs net-tools zip firefox python-pip python-flask python-requests python-pwntools python-cryptography python-pycryptodome
```

## Gnome setup

Disable hot corner and enable over-amplification:

``` bash
gsettings set org.gnome.desktop.interface enable-hot-corners false
gsettings set org.gnome.desktop.sound allow-volume-above-100-percent 'true'
```

In Gnome Extensions, enable Apps Menu, Places Status Indicator, Status Icons, and System Monitor.

Install Caffeine:

``` bash
yay -S gnome-shell-extension-caffeine
```

Restart computer and then enjoy.

## Bluetooth setup

Install required packages:

``` bash
sudo pacman -S bluez
sudo pacman -S bluez-utils
```

Check if the `btusb` kernel module is loaded:

``` bash
systool -v -m btusb
```

Enable and start Bluetooth service:

``` bash
systemctl enable bluetooth.service
systemctl start bluetooth.service
```

Restart and enjoy.

## Font

Install MonoLisa Regular, following [this](https://medium.com/@salswa/install-monolisa-font-in-vscode-and-github-codespaces-d5b81ed7f047) link as a guide.
