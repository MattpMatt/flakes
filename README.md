0. Prepare a 64-bit nixos [minimal iso image](https://channels.nixos.org/nixos-22.11/latest-nixos-minimal-x86_64-linux.iso) and burn it, then enter the live system. Suppose I have divided two partitions `/dev/vda1` `/dev/vda2` 

1. Format the partition 
```bash
mkfs.fat -F 32 /dev/vda1 
mkfs.ext4 /dev/vda2

2. Moount 
```bash 
mount /dev/vda2 /mnt 
mkdir /mnt/boot 
mount /dev//vda1 /mnt/boot
```
3. Generate a basic configuration 
```bash
nixos-generate-config --root /mnt
```
4. Clone the repository locally 
```bash
nix-shell -p git
git clone  https://github.com/Ruixi-rebirth/flakes.git --branch=minimal /mnt/etc/nixos/Flakes 
cd /mnt/etc/nixos/Flakes/
nix develop --extra-experimental-features nix-command --extra-experimental-features flakes 
```
5. Copy `hardware-configuration.nix` from /mnt/etc/nixos to /mnt/etc/nixos/Flakes/hosts/laptop/hardware-configuration.nix 
```bash 
cp /mnt/etc/nixos/hardware-configuration.nix /mnt/etc/nixos/Flakes/hosts/laptop/hardware-configuration.nix
```
6. remove '/mnt/etc/nixos/Flakes/.git' 
```bash 
rm -rf .git
```
7. Username modification: edit `/mnt/etc/nixos/Flakes/flake.nix` to modify **user** variable, hostname modification: edit `/mnt/etc/nixos/Flakes/hosts/system.nix` to modify* The **hostName** value in the **networking** property group

8. 9. Use the hash password generated by the `mkpasswd {PASSWORD} -m sha-512` command to replace the value of `users.users.<name>.hashedPassword` in `/mnt/etc/nixos/Flakes/hosts/laptop/{wayland | x11}/default.nix` ( there is two place needs to be displace )

9. Install bspwm or hyprland see [here]() and [here]() 

10. Perform install
```bash
nixos-install --no-root-passwd --flake .#laptop
```

11. Reboot 
```bash
reboot
```

12. Enjoy it!

