# Caketop OS

Caketop OS is a live image based on Debian.

## TODO

 - [ ] Name all Ansible tasks
 - [ ] Write & compare checksums
 - [ ] Artifacts
 - [ ] Cache guestfs stuff in volume
 - [ ] Build bootable image on FAT for removable media
 - [ ] posixovl over FAT
 - [ ] Seamless transition from plymouth to alacritty
 - [ ] udiskie & gvfs
 - [ ] Notifications
 - [ ] Samba server
 - [ ] SSH server
 - [ ] Web UI for file transfer / status / etc
 - [ ] btrfs user disk on qcow on FAT
 - [ ] Use ostree to generate update layers on user disk
 - [ ] Python/QML launcher
   - [ ] Menus
   - [ ] Joystick support
   - [ ] On-screen keyboard support
   - [ ] Network / wifi config
   - [ ] Bluetooth config
   - [ ] Service config
   - [ ] Audio / video config
   - [ ] Overlay
   - [ ] Theming w/ QML
   - [ ] App store
 - [ ] Input server
   - [ ] Mapping joystick to mouse/keyboard
   - [ ] Proxying input from remote app - possible to copy Apple TV protocol?
   - [ ] Proxying input from IR remote
 - [ ] Podman containers on user disk
 - [ ] Containerized apps / plugins
   - [ ] RetroArch (Lakka build?)
   - [ ] Kodi
   - [ ] Steam
   - [ ] MAME
   - [ ] DOSBox
   - [ ] SMBX / TheXTech
   - [ ] Various desktops
   - [ ] Super stretch goal: Android TV
 - [ ] Build bootable image on LVM for fixed media
 - [ ] Installer
 - [ ] GPT & UEFI support
 - [ ] Split builder into separate repo
 - [ ] Publish builder on Docker Hub
 - [ ] VS Code stuff / devcontainer
 - [ ] Better builder CLI

## Dev log

### 2020-05-20

- Re: GRUB & hybrid GPT/MBR: https://www.slivermetal.org/2016/09/18/how-to-create-an-hybrid-uefi-gpt-bios-gptmbr-boot-usb-disk/

### 2020-05-19

- Re: splash screen/seamless transition
  - Discovered that syslinux doesn't have an equivalent to GRUB2's [gfxpayload](https://www.gnu.org/software/grub/manual/grub/html_node/gfxpayload.html) - bootloader will need to be GRUB2 for seamless transition from bootloader to splash screen to desktop.
  - Need to investigate differences between Plymouth in Ubuntu and Plymouth in Debian and possibly build the Ubuntu version of it.

- Original plan was to use xorg + steamos-compositor and then put off doing a custom Wayland compositor until a version 2, but it looks like Qt5 makes whipping up a custom Wayland compositor [dead-ass easy](https://doc.qt.io/qt-5/qtwaylandcompositor-index.html) so I think we're going Wayland from the start.

- Leaving this here to raid for ideas re: containerized application runtime: https://github.com/mviereck/x11docker

- Re: Android
  - https://www.collabora.com/news-and-blog/blog/2019/04/01/running-android-next-to-wayland/
  - https://gitlab.collabora.com/spurv
