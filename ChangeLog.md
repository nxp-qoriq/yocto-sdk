# Date: 6/30/2023
# Release: 6.1.22-2.0.0
### New Features
- Kernel v6.1.22
- TF-A v2.8
- U-boot v2023.04
- Yocto Mickledore 4.2
- DPDK v22.11
- DPAA2-eth: add support for 10G <-> 25G dynamic reconfiguration
- MC 10.37.0
- OPTEE v3.21
- OVS-DPDK v3.1.0
- VPP v2302
- Preempt RT kernel branch lf-6.1.y-rt to be published on 31-July on https://github.com/nxp-real-time-edge-sw/real-time-edge-linux

### Bug fixes
| ID    |  Description   |
| --- | --- |
| LF-7784 | Sleep can't work on LX2160ARDB |
| DPDK-3801 | There is "Segmentation fault" when exiting from dpdk application |
| DPDK-3843 | Link speed display does not work for 100G interface on LX2160ARDB |
| LF-9183 | Build mesa-demos fails for GPU on LS1028ARDB |

### Known limitation 
- GPU demo fails run on LS1028ARDB display. (Work around: export WAYLAND_DISPLAY=wayland-1 before running demos)
- Secure boot fails on DPAA1 platforms and LS1012A platforms.
