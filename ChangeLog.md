# Date: 6/30/2022
# Release: 5.15.32-2.0.0
### New Features
- Kernel v5.15.32
- Preempt RT Kernel v5.15.32
- Yocto Kirkstone 4.0
- TF-A v2.6
- U-boot v2022.04
- DPDK
  - Event mode dpdk-ipsec-secgw application supported on DPAA2
  - Support for FAT Tunnel over multiple cores on DPAA2
  - TM framework：QoS implementing multi channels and shaping on DPAA2
  - QDMA demo with upstreamed DMA driver on DPAA1
  - PTP client application support on DPAA1
- Linux kernel driver
  - DPAA1 upstream Ethernet support with UEFI ACPI boot, requiring to enable the following Linux kernel configs:
      ```
        CONFIG_EFI_GENERIC_STUB_INITRD_CMDLINE_LOADER=y
        CONFIG_CMA_SIZE_MBYTES=256
        CONFIG_CMA_ALIGNMENT=12
      ```
  - DPAA2 Ethernet
    - Dynamic port reconfiguration
    - Dynamic interrupt coalescing
    - Software TSO
  - Felix switch
    - Per port and per flow mirroring
    - Basic QoS classification
    - Backporting upstream v5.18 DSA
  - Enetc
    - Software TSO
- Management Complex 10.32.0
- OPTEE 3.17
  - Enabling DTB overlay
- Vulnerability Fix
  - Inclusion of CVE-2022-23960
  - Inclusion of New ARM Spectre Variant
  - Inclusion of CVE-2022-0847
### Bug Fixes
| ID    |  Description   |
| --- | --- |
| LF-2893 | Functionalities that require PCI reset, such as VFIO, will work only with PCI endpoints that support Function Level Reset (FLR) |
| LF-5134    | Display firmware does not work on LS1028ARDB    |
| LF-6222    |  Audio does not work on LS1028ARDB   |
| QUEFI-1999    |  There is no dpaa ethernet port in UEFI dtb mode on LS1046ARDB   |

# Date: 3/31/2022
# Release: 5.15.5-1.0.0
### New Features
- Kernel v5.15.5
- Preempt RT Kernel v5.15.5
- Yocto Honister 3.4
- DPDK 21.11
- Management Complex 10.31.1
- OpenSSL 3.0
### Bug Fixes
| ID    |  Description   |
| --- | --- |
| DPDK-1827    | ovs-host_20g_6c failed with 100% load   |
| DPDK-3070    | IPSEC ESN Checks fails with raw buffer implementation    |
| LF-2595    | tsn_switch_802_1qav_performance test fails on LS1028ARDB    |
| LF-3792    | Kexec fails to switch to second kernel on LX2160ARDB    |
| LF-3954    | In kdump, usb device cannot be probed in the second kernel    |
| LF-4650    | Fuse provisioning build fails on LS1043A and LS1046A in LSDK21.08    |
| LF-5105    | Probing PCIe EPs behind PCIe switch fails with u-boot from LSDK 21.08 release    |
| LF-5284    | Correct HNF-SNF mapping and measure DDR performance improvement on LX2160ARDB   |
| LF-5365    | DPAA2 1588 SINGLE_STEP performance degrades    |
| LFU-155    | On LS1012AFRWY, the Ethernet PHY driver does not work well in U-Boot    |
