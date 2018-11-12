# QorIQ Yocto SDK Project
QorIQ Yocto SDK is a complete Linux kit for NXP QorIQ SoC's and 
the reference and evaluation boards.

## Supported boards
ls1012ardb
ls1021atwr
ls1043ardb
ls1046ardb
ls1088ardb-pb
ls2088ardb
 

## Setting up to use the Yocto project
Follow below guide to get your build host ready:
http://www.yoctoproject.org/docs/2.4/yocto-project-qs/yocto-project-qs.html#yp-resources

Install the repo utility:
```
$ mkdir ~/bin
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
```

Download the metadata:
```
$ export PATH=${PATH}:~/bin
$ mkdir yocto-sdk
$ cd yocto-sdk
$ repo init -u https://bitbucket.sw.nxp.com/scm/dnyocto/yocto-sdk.git -b edgescale-dev
$ repo sync --no-clone-bundle
```

## Building images
Take ls1012ardb as an example:

1. Setup build envrioment
```
$ . ./setup-env -m ls1012ardb
```

2. Build images used to generate EdgeScale bootstrap image
```
$ bitbake fsl-image-kernelitb
```

Note 1: Images will be found under tmp/deploy/images/ls1012ardb/.

Note 2: To build images with optee, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " edgescale-optee"
```
Note 3: To enable the ima_evm feature, need to add following line to build_ls1012ardb/conf/local.conf
```
DELTA_KERNEL_DEFCONFIG = "ima-evm.config"
```

## Build EdgeScale bootstrap images
Take ls1012ardb for example:

1. Download flash image tool
```
$ git clone ssh://git@bitbucket.sw.nxp.com/~nxa23275/flash-image-tool.git -b master
```

2. Build flash image without ima_evm
```
$ cd flash-image-tool
$ ./gen_flash_image.pl -c ls1012ardb/flashmap_qspi.cfg -e ls1012ardb/uboot_env_qspi.txt -d <PATH>/build_ls1012ardb/tmp/deploy/images/ls1012ardb -o qspi_ls1012a.img
```

3. Build flash image with ima_evm (must be integrated the kernel option, detailes see above "Note 3")
```
$ cd flash-image-tool
$ ./gen_flash_image.pl -c ls1012ardb/flashmap_qspi.cfg -e ls1012ardb/uboot_env_qspi_enable_ima_evm.txt -d <PATH>/build_ls1012ardb/tmp/deploy/images/ls1012ardb -o qspi_ls1012a.img
```

Note： please choose suitable kernelitb image and then update the flashmap file.
