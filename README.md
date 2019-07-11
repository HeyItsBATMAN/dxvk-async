# What is this?

This repository is for people who want [DXVK](https://github.com/doitsujin/dxvk) with the async pipeline compiler.

The async pipeline compiler was first developed by [jomihaka](https://github.com/jomihaka/dxvk-poe-hack) for Path of Exile
to improve DirectX 11 performance.

Async compilation was introduced to the main DXVK repository [in a commit](https://github.com/doitsujin/dxvk/commit/c3b542878c99b701f8f52e9e7a6b9a340421ba84),
but was later removed due to some controversy regarding [Overwatch bans](https://github.com/doitsujin/dxvk/commit/922f0382f69088fc0263c72bbfe6418aa1fce9bf)

Blizzard was quick to respond that Linux users will [NOT get banned for this](https://www.reddit.com/r/linux_gaming/comments/9g111m/blizzard_removes_bans_of_linux_overwatch_players/)
but the changes were not reverted.

~~This uses a modified version of jomihaka [Patch for DXVK](https://github.com/jomihaka/dxvk-poe-hack) but inside of a PKGBUILD~~

# Current Version

If you clone the repository as is and run
```
makepkg -si
```
the DXVK version you will get is the release version 1.2.3 up to [this commit](https://github.com/doitsujin/dxvk/commit/4e22e4bc3ad2f8c7f541fbdcc3caa2587f802b1d) patched with the async patch

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
To enable it, set the DXVK_ASYNC environment variable to 1
```
DXVK_ASYNC=1
```

# Guide for Lutris

### To install a custom DXVK version for a game in Lutris

Using "Manual" folder for runtime doesn't seem to work anymore
Inside a terminal, cd to .local/share/lutris/runtime/dxvk
```
cd .local/share/lutris/runtime/dxvk
```
Symlink any valid dxvk version to the patched one, e.g.
```
ln -s /usr/share/dxvk/ ./1.2.3
```
Now select the version you symlinked in your game configuration

### To enable the async pipeline compiler for a game in Lutris

- Go back to Lutris
- Right click and click 'Configure'
- Go to 'System options'
- Under 'Environment variables' click 'Add'
- Change Key to 'DXVK_ASYNC'
- Change value to 1

# Credits

Big thanks to

[ssorgatem](https://aur.archlinux.org/packages/dxvk-mingw-git) for the original PKGBUILD from the AUR

[doitsujin](https://github.com/doitsujin/dxvk) for DXVK

[archfan](https://github.com/archfan) for updating and testing for patch 0.91+ (outdated now)

[jomihaka](https://github.com/jomihaka/dxvk-poe-hack) for the original and most updated patch
