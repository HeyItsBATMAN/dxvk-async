# What is this?

This repository is for people who want [DXVK](https://github.com/doitsujin/dxvk) with the async pipeline compiler.
The async pipeline compiler was first developed by [jomihaka](https://github.com/jomihaka/dxvk-poe-hack) for Path of Exile
to improve DirectX 11 performance.
Async compilation was introduced to the main DXVK repository in dxvk commit c3b542878c99b701f8f52e9e7a6b9a340421ba84,
but was later removed due to some controversy regarding [Overwatch bans](https://github.com/doitsujin/dxvk/commit/922f0382f69088fc0263c72bbfe6418aa1fce9bf)
Blizzard was quick to respond that Linux users will [NOT get banned for this](https://www.reddit.com/r/linux_gaming/comments/9g111m/blizzard_removes_bans_of_linux_overwatch_players/)
but the changes were not reverted.

While jomihaka has his own reverted [Patch for DXVK](https://github.com/jomihaka/dxvk-poe-hack) it's not as comfortable as using a PKGBUILD for makepkg
So here we are!

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
