Written for Ubuntu.

# Initial config
Make sure you're using Xorg (compatibility with streaming/recording software and drag-drop functionality for browsers)
```
sudo sed -i '/WaylandEnable=false/s/^# //g' /etc/gdm3/custom.conf
sudo systemctl restart gdm3
echo "$XDG_SESSION_TYPE"
```

To switch back to Wayland, run:
```
sudo sed -i '/WaylandEnable=false/s/^/# /g' /etc/gdm3/custom.conf
sudo systemctl restart gdm3
echo "$XDG_SESSION_TYPE"
```

TODO: Rustup, Zsh and Oh My Zsh + Zellij, Alacritty