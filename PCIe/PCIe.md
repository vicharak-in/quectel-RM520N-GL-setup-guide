# Initial Hardware Setup and Configuration Guide

## Pre-requisites

- 40 Pins FPC Cable
- Axon 4G/5G HAT
- 4 Antenna Connectors are available on RM520N-GL

### GPIO Table

| Left Side | Pin | | Pin | Right |
|:---:|:---:|:---:|:---:|:---:|
| **12V** | 1 | | 2 | NC |
| **12V** | 3 | | 4 | NC |
| NC | 5 | | 6 | **GND** |
| NC | 7 | | 8 | **GND** |
| **FULL_CARD_POWER_OFF** | 9 | | 10 | **W_DISABLE1#** |
| NC | 11 | | 12 | **W_DISABLE2#** |
| NC | 13 | | 14 | **3V3** |
| NC | 15 | | 16 | **3V3** |
| NC | 17 | | 18 | **WAKE_ON_WAN#** |
| NC | 19 | | 20 | **RESET** |
| NC | 21 | | 22 | **3V3** |
| **DPR** | 23 | | 24 | NC |
| NC | 25 | | 26 | **GND** |
| NC | 27 | | 28 | NC |
| NC | 29 | | 30 | NC |


## Setup and Compile Driver

1. Clone this repository onto the Axon board and follow the compilation steps to build the driver.

Now, assume that module is connected anded working in PCIe mode, then **lspci** should return below output:

```
vicharak@vicharak:~$ lspci
0002:20:00.0 PCI bridge: Rockchip Electronics Co., Ltd RK3588 (rev 01)
0002:21:00.0 Unassigned class [ff00]: Qualcomm Technologies, Inc Device 0308
```

2. Compiling PCI Driver 

```
sudo make 
```
**pcie_mhi.ko** file should be generated.

After that, insert PCIe_MHI driver in Axon.

```
sudo insmod ./pcie_mhi.ko
```

## Log

```
vicharak@vicharak~# dmesg | grep mhi
[  138.483252] mhi_init Quectel_Linux_PCIE_MHI_Driver_V1.3.0.6
[  138.492350] mhi_pci_probe pci_dev->name = 0000:01:00.0, domain=0, bus=1, slot=0, vendor=17CB, device=0306
```
3. Attach QConnectManager tool into linux

- ``cd tools``
- ``sudo ./quectel-CM &``

4. Now, AT command can be configures using below command.

```
sudo minicom -D /dev/mhi_DUN
```

## Reference logs you can find in pcie_mhi_driver folder

```
log/QXDM_OVER_PCIE.txt
log/AT_OVER_PCIE.txt
log/MBIM_OVER_PCIE.txt
log/QMI_OVER_PCIE.txt
```
