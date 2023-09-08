
## Buildroot
--------

Buildroot is a tool that simplifies and automates the process of building a complete Linux system for an embedded system, using cross-compilation.


## Development Enviorment
--------

Example: Ubuntu

```bash
$ sudo apt update
$ sudo apt install build-essential patch flex bison texinfo gawk git libncurses-dev libssl-dev
```


## Build
--------

To build and use the buildroot stuff, do the following:

1. run 'make menuconfig'
2. select the target architecture and the packages you wish to compile
3. run 'make'
4. wait while it compiles
5. find the kernel, bootloader, root filesystem, etc. in output/images

You do not need to be root to build or run buildroot.  Have fun!

Buildroot comes with a basic configuration for a number of boards. Run
'make list-defconfigs' to view the list of provided configurations.


## Example
--------

### Example 1

```bash
$ cd Buildroot
$ ls ./config/qemu_*
./configs/qemu_aarch64_sbsa_defconfig         ./configs/qemu_mips64r6el_malta_defconfig
./configs/qemu_aarch64_virt_defconfig         ./configs/qemu_nios2_10m50_defconfig
./configs/qemu_arm_versatile_defconfig        ./configs/qemu_or1k_defconfig
./configs/qemu_arm_versatile_nommu_defconfig  ./configs/qemu_ppc64_e5500_defconfig
./configs/qemu_arm_vexpress_defconfig         ./configs/qemu_ppc64_pseries_defconfig
./configs/qemu_arm_vexpress_tz_defconfig      ./configs/qemu_ppc64le_pseries_defconfig
./configs/qemu_csky610_virt_defconfig         ./configs/qemu_ppc_e500mc_defconfig
./configs/qemu_csky807_virt_defconfig         ./configs/qemu_ppc_g3beige_defconfig
./configs/qemu_csky810_virt_defconfig         ./configs/qemu_ppc_mac99_defconfig
./configs/qemu_csky860_virt_defconfig         ./configs/qemu_ppc_mpc8544ds_defconfig
./configs/qemu_m68k_mcf5208_defconfig         ./configs/qemu_riscv32_virt_defconfig
./configs/qemu_m68k_q800_defconfig            ./configs/qemu_riscv64_virt_defconfig
./configs/qemu_microblazebe_mmu_defconfig     ./configs/qemu_s390x_defconfig
./configs/qemu_microblazeel_mmu_defconfig     ./configs/qemu_sh4_r2d_defconfig
./configs/qemu_mips32r2_malta_defconfig       ./configs/qemu_sh4eb_r2d_defconfig
./configs/qemu_mips32r2el_malta_defconfig     ./configs/qemu_sparc64_sun4u_defconfig
./configs/qemu_mips32r6_malta_defconfig       ./configs/qemu_sparc_ss10_defconfig
./configs/qemu_mips32r6el_malta_defconfig     ./configs/qemu_x86_64_defconfig
./configs/qemu_mips64_malta_defconfig         ./configs/qemu_x86_defconfig
./configs/qemu_mips64el_malta_defconfig       ./configs/qemu_xtensa_lx60_defconfig
./configs/qemu_mips64r6_malta_defconfig       ./configs/qemu_xtensa_lx60_nommu_defconfig
$ make qemu_x86_64_defconfig
$ make -j4
```

### Example 2

```bash
$ cd /home/bob/br_ptoject/
$ mkdir sources build
$ git clone github.com/buildroot/buildroot.git
$ cd build/
$ touch THIS_IS_QEMU_X64
$ make O=$PWD -C ../buildroot qemu_x86_64_defconfig
$ make menuconfig
$ time(make -j4 2>&1 | tee build.log)
```

The br_ptoject skeleton is:

```bash
$ ls /home/bob/br_project
build  buildroot  dl  sources
```

- build     - buildroot output, include compile, sources, binary, target, images, etc.
- buildroot - buildroot project.
- dl        - download software packages directory (set BR2_DL_DIR = $(TOPDIR)/../dl).
- sources   - custom source code.

**Custome Source Code** (example for busybox):

```bash
$ cd /home/bob/br_project/sources/
$ wget https://www.busybox.net/downloads/busybox-1.36.1.tar.bz2
$ tar -xf busybox-1.36.1.tar.bz2
$ cd /home/bob/br_project/build/
$ echo "BUSYBOX_OVERRIDE_SRCDIR = /home/bob/br_project/sources/busybox-1.36.1" > local.mk
$ make busybox-dirclean
$ make busybox-rebuild
```

Other commands:

```bash
$ make <package>-source
$ make <package>-depends
$ make <package>-extract
$ make <package>-patch
$ make <package>-configure
$ make <package>-build
$ make <package>-install-staging
$ make <package>-install-target
$ make <package>-install
$ make <package>-show-depends
$ make <package>-show-recursive-depends
$ make <package>-show-redepends
$ make <package>-show-recursive-rdepends
$ make <package>-graph-depends
$ make <package>-graph-rdepends
$ make <package>-dirclean
$ make <package>-reinstall
$ make <package>-rebuild
$ make <package>-reconfigure
```


## Running
--------

```bash
$ cd Buildroot
$ cat ./board/qemu/x86_64/readme.txt
Run the emulation with:

  qemu-system-x86_64 -M pc -kernel output/images/bzImage -drive file=output/images/rootfs.ext2,if=virtio,format=raw -append "rootwait root=/dev/vda console=tty1 console=ttyS0" -serial stdio -net nic,model=virtio -net user # qemu_x86_64_defconfig

Optionally add -smp N to emulate a SMP system with N CPUs.

The login prompt will appear in the graphical window.
$
$
$ ./output/host/bin/qemu-system-x86_64 -M pc -kernel output/images/bzImage -drive file=output/images/rootfs.ext2,if=virtio,format=raw -append "rootwait root=/dev/vda console=tty1 console=ttyS0" -serial stdio -net nic,model=virtio -net user
```


## Log
--------

```bash
Linux version 5.10.66
...
Run /sbin/init as init process
random: fast init done
EXT4-fs (vda): re-mounted. Opts: (null)
Starting syslogd: OK
Starting klogd: OK
Running sysctl: OK
Saving random seed: random: dd: uninitialized urandom read (512 bytes read)
OK
Starting network: udhcpc: started, v1.33.1
random: mktemp: uninitialized urandom read (6 bytes read)
udhcpc: sending discover
udhcpc: sending select for 10.0.2.15
udhcpc: lease of 10.0.2.15 obtained, lease time 86400
deleting routers
random: mktemp: uninitialized urandom read (6 bytes read)
adding dns 10.0.2.3
OK

Welcome to Buildroot
buildroot login: root
# ls
# cd ..
# ls
bin         lib         lost+found  opt         run         tmp
dev         lib64       media       proc        sbin        usr
etc         linuxrc     mnt         root        sys         var
# 
# 
# 
```

## Version

The buildroot base on buildroot-2023.8.


## Link
--------

[buildroot homepage](https://buildroot.org/)

[user manual](docs/custom/buildroot_manual.md)

[contribute patches](https://buildroot.org/manual.html#submitting-patches)


## License
--------

The Buildroot is licensed under the GNU General Public License, version 2.

