> Note  
> Clone this repository onto the Axon board and follow the compilation steps to build the driver.

## Driver Structure

```
$ tree drivers/pcie_mhi/ -L 1
drivers/pcie_mhi/
 controllers
 core
 devices
 Makefile
```

## Compiling PCI Driver 

```
sudo make 
```
**pcie_mhi.ko** file should be generated.

Now, assume that module is connected anded working in PCIe mode, then **lspci** should return below output:

```
vicharak@vicharak:~$ lspci
0002:20:00.0 PCI bridge: Rockchip Electronics Co., Ltd RK3588 (rev 01)
0002:21:00.0 Unassigned class [ff00]: Qualcomm Technologies, Inc Device 0308
```
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


## Reference logs

```
log/QXDM_OVER_PCIE.txt
log/AT_OVER_PCIE.txt
log/MBIM_OVER_PCIE.txt
log/QMI_OVER_PCIE.txt
```
