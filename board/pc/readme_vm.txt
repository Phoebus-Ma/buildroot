Bare PC sample config
=====================

1. Build

  First select the appropriate target you want.

  For EFI-based boot strategy, music player:

  $ make vm_x64_music_player_defconfig

  For EFI-based boot strategy, qt environment:

  $ make vm_x64_qt_defconfig

  For EFI-based boot strategy, weston (wayland) environment:

  $ make vm_x64_weston_defconfig

  Add any additional packages required and build:

  $ make

2. link iso image

  The build process will create a pendrive image called
  rootfs.iso9660 in output/images.

  link iso image:

  $ ln -s output/images/rootfs.iso9660 ${HOME}/rootfs.iso

  You can use a virtual machine to load the rootfs.iso image
  and use.

3. Enjoy

Emulation in virtualbox
=======================

1. Create virtual machine:

  Type: Linux
  Version: Linux 2.6 / 3.x / 4.x / 5.x (64-bit)
  Base Memory: 1024 MB
  Processors: 1 CPU
  Virtual Hard Disk: Do Not Add a Virtual Hard Disk
  Graphics Controller: VMSVGA
  Audio Controller: Intel HD Audio
  Network Adapter: Bridged Adapter  

2. Start system:

  Start system.
  User name: root
  Password: none

3. For music player project:

	mpg123 test.mp3

4. For qt project:

	./calculator

5. For weston project:

	mkdir -p /tmp/wayland
	chmod 0700 /tmp/wayland
	export XDG_RUNTIME_DIR=/tmp/wayland
	weston


Emulation in wmware
===================

See virtualbox.
