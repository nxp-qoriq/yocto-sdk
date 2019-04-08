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
lx2160ardb
 

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
$ repo init -u ssh://git@bitbucket.sw.nxp.com/dcca/edgescale-bootstrap.git -b master
$ repo sync --no-clone-bundle
```

## Building images
Take ls1012ardb as an example:

1. Setup build envrioment
```
$ . ./setup-env -m ls1012ardb
```

2. Build EdgeScale bootstrap images
```
$ bitbake edgescale-bootstrap
```

Note 1: Edgescale bootstrap images will be found under tmp/deploy/images/ls1012ardb/edgescale-bootstrap/.

Note 2: To build images with optee, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " edgescale-optee"
```
Note 3: To enable the ima_evm feature, need to add following line to build_ls1012ardb/conf/local.conf
```
DISTRO_FEATURES_append = " ima-evm"
```

