
# Toolchain SDK

## 1.Generate SDK
--------

1.1 Configure

- Select the appropriate **Target options** for your target CPU architecture;
- In the **Toolchain** menu, keep the default of **Buildroot toolchain** for **Toolchain type**, and configure your toolchain as desired;
- In the **System configuration** menu, select **None** as the **Init system** and **none** as **/bin/sh**;
- In the **Target packages** menu, disable **BusyBox**;
- In the **Filesystem images** menu, disable **tar the root filesystem**;

1.2 Build

```bash
make sdk -j4
```

1.3 File (such as x86_64):

```bash
output/images/x86_64-buildroot-linux-gnu_sdk-buildroot.tar.gz
```


## 2.Use Toolchain SDK

1. unzip toolchain sdk:

```bash
$ cd /home/bob/toolchain/
$ tar -xf x86_64-buildroot-linux-gnu_sdk-buildroot.tar.gz
$ cd x86_64-buildroot-linux-gnu_sdk-buildroot/
$ ./relocate-sdk.sh
$ pwd
/home/bob/toolchain/x86_64-buildroot-linux-gnu_sdk-buildroot/
```

2. configure buildroot:

```bash
$ make menuconfig
```

do this configure:

```bash
Toolchain  --->
    Toolchain type (External toolchain)  --->
    Toolchain (Custom toolchain)  --->
    Toolchain origin (Pre-installed toolchain)  --->
    (/home/bob/toolchain/x86_64-buildroot-linux-gnu_sdk-buildroot) Toolchain path
    External toolchain gcc version (12.x)  --->
    External toolchain kernel headers series (6.1.x)  --->
    External toolchain C library (glibc)  --->
    [*] Toolchain has SSP support? (NEW)
    [*]   Toolchain has SSP strong support? (NEW)
```


## Add GDB

Enable BR2_PACKAGE_HOST_GDB, BR2_PACKAGE_GDB and BR2_PACKAGE_GDB_SERVER.
