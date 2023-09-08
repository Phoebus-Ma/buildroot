
# Build qt project

## 1.Create weston project style 1

```bash
$ make vm_x64_weston_defconfig
$ make -j4
```


## 2.Create weston project style 2

The isolinux.cfg:

```bash
$ cat buildroot/fs/iso9660/isolinux.cfg
default 1
label 1
      kernel __KERNEL_PATH__
      initrd __INITRD_PATH__
      append root=/dev/sda rw
```

The linux.config Adding:

```bash
CONFIG_DRM_VMWGFX=y
CONFIG_DRM_VMWGFX_FBCON=y
CONFIG_DRM_VMWGFX_MKSSTATS=y
```

The configure:

```bash
$ make pc_x86_64_efi_defconfig
$ make menuconfig
```

```bash
Toolchain  --->
    [*] Install glibc utilities
    [*] Enable C++ support

Target packages  --->
    Audio and video applications  --->
        [*] alsa-utils  --->
        [*] pulseaudio
        [*]   start as a system daemon

    Graphic libraries and applications (graphic/text)  --->
        [*] mesa3d  --->
            [*]   Gallium vmware svga driver
            -*-   gbm
            [*]   OpenGL EGL
            [*]   OpenGL ES
        [*] weston
              default compositor (drm)  --->
        -*-   DRM compositor
        [*]   desktop shell
        [*]   fullscreen shell
        [*]   ivi shell
        [*]   kiosk shell
        [*]   screenshare
        -*- xkeyboard-config

    Networking applications  --->
        [*] openssh
        [*]   client
        [*]   server
        [*]   key utilities
        [*]   use sandboxing

Filesystem images  --->
    (320M) exact size
    [*] iso image
        Bootloader (isolinux)   --->
    (fs/iso9660/isolinux.cfg) Boot menu config file
    [*]	Use initrd

Bootloaders  --->
    [*] grub2
    [*]   x86-64-efi
    [*] syslinux
    -*-     install isolinux
```

```bash
$ make -j4
```


## 3.Link and launch

```bash
$ ln -s output/images/rootfs.iso9660 ${HOME}/rootfs.iso
```

Create virtualbox machine (also vmware):

1. Create virtual machine:

- Type: Linux
- Version: Linux 2.6 / 3.x / 4.x / 5.x (64-bit)
- Base Memory: 1024 MB
- Processors: 1 CPU
- Virtual Hard Disk: Do Not Add a Virtual Hard Disk
- Graphics Controller: VMSVGA
- Audio Controller: Intel HD Audio
- Network Adapter: Bridged Adapter
- Optical Drive: rootfs.iso

2. Start system:

Start system.

- User name: root
- Password: none

3. For qt project:

```bash
$ mkdir -p /tmp/wayland
$ chmod 0700 /tmp/wayland
$ export XDG_RUNTIME_DIR=/tmp/wayland
$ weston
```
