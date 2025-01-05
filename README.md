<img src="data/icons/dev.vlinkz.NixSoftwareCenter.svg"/>

(Fixed) Nix Software Center
===
This fork is a replacement for the broken Nix Software Center of snowfallorg.

It fixes the `‘gnome.adwaita-icon-theme’ was moved to top-level.` error appearing on `nixos-rebuild`.

No other changes were made, so old version, sources, and autorship are displayed when running the program.

To get it working on your system, modify `configuration.nix` and `flake.nix` as instructed.
Both files are located at `/etc/nixos`.

Manual for existing installs with broken Nix Software Center:
===
In `configuration.nix`, find lines:
```
owner = "snowfallorg";
repo = "nix-software-center";
rev = "0.1.2";
sha256 = "xiqF1mP8wFubdsAQ1BmfjzCgOD3YZf7EGWl9i69FTls=";
```
Delete them and replace them with:
```
owner = "ljubitje";
repo = "nix-software-center";
rev = "0.1.3"; # on update, change this value manually!
sha256 = "HVnDccOT6pNOXjtNMvT9T3Op4JbJm2yMBNWMUajn3vk="; # on update, change this value manually!
```
In `flake.nix`, find line:
```
nix-software-center.url = "github:snowfallorg/nix-software-center";
```
Delete it and replace it with:
```
nix-software-center.url = "github:ljubitje/nix-software-center";
```
Move to location of your flake and apply new configuration by running this as sudo in your terminal:
```
cd /etc/nixos && nix flake update && nixos-rebuild switch --upgrade
```

Manual for fresh install:
===
Remove any code/lines from previous (broken) Nix Software Center installation.

To `configuration.nix`, add `"flakes"` and `"nix-command"` experimental features, like this line:
```
nix.settings.experimental-features = [ "flakes" "nix-command" ];
```
To `configuration.nix`, add:
```
nixpkgs.config.packageOverrides = pkgs: {
  nix-software-center = pkgs.callPackage (pkgs.fetchFromGitHub {
    owner = "ljubitje";
    repo = "nix-software-center";
    rev = "0.1.3"; # on update, change this value manually!
    sha256 = "HVnDccOT6pNOXjtNMvT9T3Op4JbJm2yMBNWMUajn3vk="; # on update, change this value manually!
  }) {};
};
```
To `configuration.nix` system packages, add the (Fixed) Nix Software Center package like so:
```
environment.systemPackages = with pkgs; [
  # your other packages here
  nix-software-center
];
```
To `flake.nix` inputs section, add:
```
nix-software-center.url = "github:ljubitje/nix-software-center";
nix-software-center.inputs.nixpkgs.follows = "nixpkgs";
```
Move to location of your flake and apply new configuration by running this as sudo in your terminal:
```
cd /etc/nixos && nix flake update && nixos-rebuild switch --upgrade
```
Note: Some parts of my Manual for fresh install may be redundant as I don't fully understand it myself due to NixOS lacking documentation.

Manual of the third kind:
===
NixOS can be a complicated mess to set up.  
You may find it best to consult original Nix Software Center repository for first installation.  
If you get `‘gnome.adwaita-icon-theme’ was moved to top-level.` error when doing `nixos-rebuild`, it means you did everything right and are now hitting the broken part of official Nix Software Center.  
If so, return here to update to my version.  
Follow the Manual for existing installs and you should be done!
Move to location of your flake and apply new configuration by running this as sudo in your terminal:
```
cd /etc/nixos && nix flake update && nixos-rebuild switch --upgrade
```
