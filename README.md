# What is this?

This repository is for people who want [DXVK](https://github.com/doitsujin/dxvk) with the async pipeline compiler.

The async pipeline compiler was first developed by [jomihaka](https://github.com/jomihaka/dxvk-poe-hack) for Path of Exile
to improve DirectX 11 performance.

Async compilation was introduced to the main DXVK repository [in a commit](https://github.com/doitsujin/dxvk/commit/c3b542878c99b701f8f52e9e7a6b9a340421ba84),
but was later removed due to some controversy regarding [Overwatch bans](https://github.com/doitsujin/dxvk/commit/922f0382f69088fc0263c72bbfe6418aa1fce9bf)

Blizzard was quick to respond that Linux users will [NOT get banned for this](https://www.reddit.com/r/linux_gaming/comments/9g111m/blizzard_removes_bans_of_linux_overwatch_players/)
but the changes were not reverted.
This uses a modified version of jomihaka's old [Patch for DXVK](https://github.com/jomihaka/dxvk-poe-hack) but inside of a PKGBUILD
So here we are!

# Current Version

If you clone the repository as is and run
```
makepkg -si
``` 
the DXVK version you will get is the release version 0.90 up to [this commit](https://github.com/doitsujin/dxvk/commit/a53e05339174ae626d1207f636c69e548fcdd6ee) patched with the async patch

# How do I use this

If you use a Linux distribution with makepkg ( e.g. Arch based distributions ) you can clone this repository and install it like this

```
# Clone the repository
cd some/path/to/clone/to
git clone https://github.com/HeyItsBATMAN/dxvk-async.git

# Change into the cloned directory
cd dxvk-async

# Makepkg
# -s synchronizes dependencies and creates a build
# -i installs the created package
makepkg -si
```

# Enabling the async pipeline compiler

By default the async pipeline compiler is disabled.
To enable it, set the DXVK_CONFIG_FILE environment variable to the path of a dxvk config file.
Inside of the config set this variable to 'True' to enable the async pipeline compiler.
```
dxvk.asyncPipeCompiler = True
```

# Guide for Lutris

### To install a custom DXVK version for a game in Lutris

- Right click on a game in Lutris
- Click 'Configure'
- Go to 'Runner options'
- Set DXVK Version to 'Manual'
- Go to 'Game options'
- Copy the path from the field 'Wine prefix'
- Open a terminal and set the WINEPREFIX environment variable to your path
```
# If using bash
export WINEPREFIX="/paste/your/path"

# Install using winetricks
# For the 64bit version
winetricks /usr/share/dxvk/x64/setup_dxvk_aur.verb
# For the 32bit version
winetricks /usr/share/dxvk/x32/setup_dxvk_aur.verb
# If you installed the wrong one use
winetricks --force /path/to/correct/setup_dxvk_aur.verb
```

### To enable the async pipeline compiler for a game in Lutris

- Go back to Lutris
- Right click and click 'Configure'
- Go to 'System options'
- Under 'Environment variables' click 'Add'
- Change Key to 'DXVK_CONFIG_FILE'
- Change value to the full path of a dxvk.conf like the example one included in the repository

# Credits

Big thanks to

[ssorgatem](https://aur.archlinux.org/packages/dxvk-git/) for the original PKGBUILD from the AUR

[doitsujin](https://github.com/doitsujin/dxvk) for DXVK

[jomihaka](https://github.com/jomihaka/dxvk-poe-hack) for the original patch, which this one is based on

[archfan](https://github.com/archfan) for updating and testing for patch 0.91+
