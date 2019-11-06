# QorIQ Yocto SDK Project
QorIQ Yocto SDK is a complete Linux kit for NXP QorIQ SoC's and 
the reference and evaluation boards.

## Supported boards
ls1012ardb
ls1012afrwy
ls1021atwr
ls1028ardb
ls1043ardb
ls1046ardb
ls1046afrwy
ls1088ardb-pb
ls2088ardb
lx2160ardb
 

## Setting up to use the Yocto project
Follow below guide to get your build host ready:
https://www.yoctoproject.org/docs/2.6/brief-yoctoprojectqs/brief-yoctoprojectqs.html

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
$ repo init -u https://source.codeaurora.org/external/qoriq/qoriq-components/yocto-sdk -b refs/tags/yocto_2.6_es_1909
$ repo sync --no-clone-bundle
```

## Building images
Take ls1012ardb as an example:

1. Setup build envrioment
```
$ . ./setup-env -m ls1012ardb
```

Note : To build single bootstrap images need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " singleboot"
```

2. Build EdgeScale bootstrap images
```
$ bitbake single-source-bootstrap
```

Note 1: Edgescale bootstrap images will be found under tmp/deploy/images/ls1012ardb/single-bootstrap/.

Note 2: To build images with optee, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " optee"
```
Note 3: To enable the ima_evm feature, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " ima-evm"
```
Note 4: To enable the Manufacture, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " mft"
```
Note 5: To enable the secure model, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " secure"
ROOTFS_IMAGE = "fsl-image-edgescale"
KERNEL_ITS = "kernel-all.its"
```
Note 6: Key pairs and files required for the EdgeScale secure bootstrap images will be found under tmp/deploy/images/ls1012ardb/.
        Without setting the specified key path, the generated key is random
        To use the specified key pair,need to modify following lines to ../sources/meta-qoriq-demos/recipes-devtools/cst/cst_git.bbappend
```
#SECURE_PRI_KEY = "/path/srk.pri"
#SECURE_PUB_KEY = "/path/srk.pub"
```
Note 7: ls1021atwr bootstrap image doesn't support single bootstrap,It can only be compiled with "bitbake edgescale-bootstrap" to build images
and you need to add following line to build_ls1021atwr/conf/local.conf
```
DISTRO_FEATURES_append = " ota"
```
The ls1021atwr bootstrap images will be found under tmp/deploy/images/ls1021atwr/edgescale-bootstrap/.

# Docker build environment for edgescale bootstrap images

edgescale bootstrap images can be built using the docker containers like on
the real build servers. you don't have to install the linux distros which
support special yocto version, no need to install the required packages,
all the yocto layers will be included in the docker images.

## Build docker image

Choose one of the dockerfiles, then run the following command:
```
$ docker build \
--build-arg http_proxy=$http_proxy \
--build-arg https_proxy=$https_proxy \
--build-arg host_uid=$(id -u) \
--build-arg host_gid=$(id -g) \
--no-cache \
-t edgescale-bootstrap:v1 .
```
Note:
if the proxy is required, please set the proxy using the following commands:
```
$ export http_proxy="xxxxxxxxx"
$ export https_proxy="xxxxxxxxx"
```

## Build edgescale bootstrap images using docker

Take ls1046ardb as an example
1. Start-up docker container:
```
$ mkdir -p yocto/build_ls1046ardb yocto/downloads yocto/sstate-cache
$ docker run -it  --name edgescale-bootstrap-ls1046ardb \
-v $PWD/yocto/build_ls1046ardb:/home/edgescale/yocto-sdk/build_ls1046ardb \
```

Note:
you can get the bootstrap images from the following two locations:
+ inside docker container:
/home/edgescale/yocto-sdk/build_ls1046ardb/tmp/deploy/images/ls1046ardb/single-bootstrap/

+ outside docker container(local host):
$PWD/yocto/build_ls1046ardb/tmp/deploy/images/ls1046ardb/single-bootstrap/

