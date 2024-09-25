Written for Ubuntu.

# Initial config
Make sure you're using Xorg (compatibility with streaming/recording software and drag-drop functionality for browsers)
```
sudo sed -i '/WaylandEnable=false/s/^#//g' /etc/gdm3/custom.conf
sudo systemctl restart gdm3
echo "$XDG_SESSION_TYPE"
```

To switch back to Wayland, run:
```
sudo sed -i '/WaylandEnable=false/s/^/#/g' /etc/gdm3/custom.conf
sudo systemctl restart gdm3
echo "$XDG_SESSION_TYPE"
```

Install Gnome Tweaks, then use the Tweaks app:
```
sudo apt install gnome-tweaks
```

Install Gnome Extensions:
```
sudo apt install gnome-shell-extensions
```

To install Gnome extensions, either use the browser plugin (doesn't work with snap installations) or install manually. [Info here.](https://itsfoss.com/gnome-shell-extensions/). Must-have extensions: gnome-shell-extension-caffeine (Caffeine version from the Ubuntu software store causes shell crashes), gnome-shell-system-monitor-applet is cool too.

# Terminal
Install zsh:
```
sudo apt install zsh
chsh -s $(which zsh)
```

Log out and back in. Test with:
```
$SHELL --version
```

Install Oh My Zsh:
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Plugins:
```
git clone --depth 1 -- https://github.com/marlonrichert/zsh-autocomplete.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autocomplete
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

Rustup:
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup override set stable
rustup update stable
```

Alacritty:
```
rustup override set stable
rustup update stable
sudo apt install cmake pkg-config libfreetype6-dev libfontconfig1-dev libxcb-xfixes0-dev libxkbcommon-dev python3
cargo install alacritty
```
You may need to also copy the `alacritty` binary to your `bin`:
```
cp .cargo/bin/alacritty /usr/local/bin/
```

Then go to Settings -> Keyboard -> Shortcuts -> Custom Shortcuts and set Ctrl+Shift+A to `alacritty`

Zellij:
```
cargo install --locked zellij
mkdir ~/.config/zellij
zellij setup --dump-config > ~/.config/zellij/config.kdl
```

Use the provided dotfiles directly in your home directory.

# Other tools
GEF:
```
bash -c "$(curl -fsSL https://gef.blah.cat/sh)"
```

RustScan:
```
sudo apt install nmap
cargo install rustscan
```
