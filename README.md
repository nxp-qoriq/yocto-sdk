Yocto with LSDK Components
==========================
Yocto with LSDK components provides recipes for the last Yocto release to use the latest and greatest components from LSDK as they get released. This eventually makes its way into the next community Yocto release at yoctoproject.org.

Supported boards
================
  * ls1012ardb
  * ls1012afrwy
  * ls1021atwr
  * ls1043ardb
  * ls1046ardb
  * ls1046afrwy
  * ls1088ardb-pb
  * ls1028ardb
  * ls2088ardb
  * lx2160ardb-rev2
  * lx2162aqds

Prepare build environment
=========================
To make sure the build host is prepared for Yocto running and build, please follow this [guide](https://docs.yoctoproject.org/brief-yoctoprojectqs/index.html) to prepare the build environment. 


Install 'repo' tool
===================
To use this manifest repo, the 'repo' tool must be installed first
```
$: mkdir ~/bin
$: curl http://commondatastorage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
$: chmod a+x ~/bin/repo
$: PATH=${PATH}:~/bin
```

Download Yocto Layers
=====================
The following is the step of how to use repo utility to download all Yocto layers according to the repo manifest. Replace <branch> with the real name (see Yocto branch and version below).

```
$: mkdir yocto-sdk
$: cd yocto-sdk
$: repo init -u https://github.com/nxp-qoriq/yocto-sdk -b <branch>
$: repo sync --force-sync
```

Yocto branch and version
========================
| Branch      | Version          |
|-------------|------------------|
| whinlatter  | YP 5.3-lf-6.18.2 |
| walnascar   | YP 5.2-lf-6.12.49|
| styhead     | YP 5.1-lf-6.12.3 |
| scarthgap   | YP 5.0-lf-6.6.52 |
| nanbield    | YP 4.3-lf-6.6.3  |
| mickledore  | YP 4.2–lf-6.1.55 |
| langdale    | YP 4.1–lf-6.1.1  |
| kirkstone   | YP 4.0–lf-5.15.71|
| honister    | YP 3.4–lf-5.15.5 |
| hardknott   | YP 3.3–lf-5.10.72|
| gatesgarth  | YP 3.2–lf-5.10.9 |
| dunfell     | YP 3.1–LSDK 2004 |
| zeus        | YP 3.0–LSDK 1909 |
| warrior     | YP 2.7–LSDK 1906 |
| thud        | YP 2.6–LSDK 1809 |
| sumo        | YP 2.5–LSDK 1806 |

Building images
===============
Take lx2160ardb-rev2 as an example:
```
$: . ./setup-env -m lx2160ardb-rev2
$: bitbake fsl-image-networking
```
or:
```
$: bitbake fsl-image-networking-full
```
Images will be found under tmp/deploy/images/lx2160ardb-rev2/.


flex-installer tools introducing
=====================================================================
If want to use flex-installer tools to write image to embedded device.


Build boot image
------------------------------
```
$ bitbake qoriq-composite-firmware
```
firmware_<machine>_uboot_sdboot.img will be found under tmp/deploy/images/lx2160ardb-rev2/


Generate boot partition tarball
------------------------------
```
$ bitbake generate-boottgz
```
boot_<machine>_lts_6.18.tgz will be found under tmp/deploy/images/lx2160ardb-rev2/



Install image by flex-installer
------------------------------
prepare for install flex-installer on Linux host PC
```
$ sudo cp meta-qoriq/meta-qoriq-sdk/recipes-extended/flex-installer/flex-installer/flex-installer  /usr/bin/
$ sudo chmod +x /usr/bin/flex-installer
```
 Plugin SD card on linux host PC and install Layerscape BSP firmware, boot tarball and Yocto-rootfs as below:
```
$ flex-installer -i pf -d /dev/mmcblkX (format SD card)
$ flex-installer -m <machine> -d /dev/<sdX/mmcblkX> -f firmware_<machine>_uboot_sdboot.img -b boot_lts_6.18.tgz -r fsl-image-networking-<machine>.rootfs.tar.gz
```

FAQ
====
1. How to compile atf with OPTEE?
   Set DISTRO_FEATURES:append = " optee" in your local.conf.

2. How to compile atf with specify different RCW image instead of the default one?
   modify  RCWNOR or RCWSD and RCWNAND in machine conf
   For example, ls1043ardb.conf in meta-freescale:
     RCWNOR ?= "RR_FQPP_1455/rcw_1600"

3. How to add custom additional packages to default rootfs?
   Set 'IMAGE_INSTALL:append = " <packages to be added>"' in your local.conf

4. Secure boot compile
   For build secure boot image ,you need to set the following variables in local.conf
   DISTRO_FEATURES:append = " secure"
```
$: bitbake secure-boot-qoriq
```


   For information on how to use secure boot, see the "6.1 Secure boot" section.
   You can find at  https://www.nxp.com/support/developer-resources/run-time-software/linux-software-and-development-tools/layerscape-software-development-kit:LAYERSCAPE-SDK?tab=Documentation_Tab
