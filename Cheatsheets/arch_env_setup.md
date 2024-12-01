# I'm an arch luser btw

Enable volume overamplification for Gnome:

``` bash
gsettings set org.gnome.desktop.sound allow-volume-above-100-percent 'true'
```

Install needed packages:

``` bash
pacman -S sysfsutils
```

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
