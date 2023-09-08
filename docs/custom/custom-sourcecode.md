
# Custom source code

## 1.Download buildroot
--------

```bash
$ cd /home/bob/br_project/
$ git clone github.com/buildroot/buildroot.git
```


## 2.Create build folder
--------

```bash
$ cd /home/bob/br_project/
$ mkdir br_build1
$ cd br_build1/
$ make O=$PWD -C ../buildroot qemu_x86_64_defconfig
```


## 3.Use custom busybox source code
--------

### 3.1 Download busybox

```bash
$ cd /home/bob/br_project/
$ mkdir sources
$ cd sources/
$ wget https://www.busybox.net/downloads/busybox-1.36.1.tar.bz2
$ tar -xf busybox-1.36.1.tar.bz2
$ cd busybox-1.36.1/
$ pwd
/home/bob/br_project/sources/busybox-1.36.1
```

### 3.2 Change configure

```bash
$ cd /home/bob/br_project/br_build1/
$ touch local.mk
$ vim local.mk
```

Adding:

```bash
BUSYBOX_OVERRIDE_SRCDIR = /home/bob/br_project/sources/busybox-1.36.1
```

Note 1: <pkg1>_OVERRIDE_SRCDIR can also be defined for other packages such as LINUX_OVERRIDE_SRCDIR.

Note 2: Its principle is to copy the source code of the custom directory to the build directory under the compilation directory through rsync. If any files have been modified, only the modified (or newly added) files will be copied next time, but if the files are deleted, it will not detect At this point, you need to manually delete the corresponding files or the entire package in the build directory.


## 4.Build
--------

### 4.1 build project:

```bash
$ cd /home/bob/br_project/br_build1/
$ time(make -j4 2>&1 | tee build.log)
```

### 4.2 build busybox package only:

```bash
$ cd /home/bob/br_project/br_build1/
$ time make busybox-build
```

### 4.3 Other build options:

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
$ make <package>-show-rdepends
$ make <package>-show-recursive-rdepends
$ make <package>-graph-depends
$ make <package>-graph-rdepends
$ make <package>-dirclean
$ make <package>-reinstall
$ make <package>-rebuild
$ make <package>-reconfigure
```
