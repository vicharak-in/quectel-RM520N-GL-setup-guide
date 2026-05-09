# USB Guide

---

## 1. System Preparation
Before hardware integration, ensure the system kernel and package repositories are up to date.

```bash
sudo apt update && sudo apt upgrade -y

```

**Note:** A system reboot is required to ensure all kernel-level changes and driver updates are active.

```bash
sudo reboot

```


## 2. Hardware Assembly

Follow these steps to ensure proper physical connection:

1. **M.2 Integration:** Seat the Quectel 5G Module into the M.2 connector of the Axon HAT.


2. **HAT Attachment:** Secure the HAT onto the Axon board.


3. **Data Link:** Connect a USB Type-C cable from the Module's USB port to one of the Axon's USB host ports.


4. **Power On:** Turn on the Axon device.


## 3. Initial Device Verification

Verify that the system recognizes the hardware at the USB bus level.

```bash
lsusb
```

#### AT Port can be verified using `mmcli -L` and`mmcli -m <Number>`

Check for the generation of TTY interfaces to ensure the module is communicating:

```bash
ls /dev/ttyUSB*

```


## 4. Module Configuration (USB Interface Mode)

The module must be explicitly set to USB mode and the appropriate network interface protocol must be defined using AT commands.

1. **Open Serial Console:** Access the AT command interface (typically `/dev/ttyUSB2`).


```bash
sudo minicom -D /dev/ttyUSB2

```

Set Data Interface:** Configure the module to use the standard USB data interface.

```
AT+QCFG="data_interface",0,0
```

3. **Set USB Network Mode:** Configure the `usbnet` setting to enable the appropriate network stack, enable according to user choice.


```at
AT+QCFG="usbnet",<value>
```

The `<value>` parameter is an integer type that defines how the 5G module presents its network interface to the host operating system.

### Configuration Values

| Value | Mode | Description |
| --- | --- | --- |
| **0** | **RMNET** | Qualcomm proprietary interface using QMI (Qualcomm MSM Interface) for management.|
| **1** | **ECM** | Ethernet Control Model. Standard protocol that emulates an Ethernet NIC.|
| **2** | **MBIM** | Mobile Broadband Interface Model.|
| **3** | **RNDIS** | Remote Network Driver Interface Specification.|

---

### Use Cases and Environment Selection

#### 1. RMNET (`usbnet,0`)

* **Best For:** Embedded Linux systems using `qmi_wwan` drivers.
* **Use Case:** High-performance data links where fine-grained control over the modem (via QMI) is required. It is widely used in OpenWrt and professional cellular gateways.

#### 2. ECM (`usbnet,1`)

* **Best For:** Linux
* **Use Case:** Systems where "driverless" operation is preferred. The module appears as a standard USB Ethernet adapter (`ethX` or `usbX`), making it very easy to set up on Axon/Linux platforms without complex dial-up scripts.

#### 3. MBIM (`usbnet,2`)

* **Best For:** Modern Linux distributions (using `cdc_mbim`).
* **Use Case:** The standard for mobile broadband.

#### 4. RNDIS (`usbnet,3`)

* **Use Case:** Used primarily when the host system only supports Microsoft's specific Ethernet-over-USB implementation. It is generally being phased out in favor of MBIM or ECM.

---

*(Note: Be sure to select the value that matches your host OS driver support.)*


4. **Power Cycle:** Power off the Axon completely and restart to allow the module to re-enumerate with the new configurations.



## 5. Driver and Protocol Verification
After rebooting, verify that the necessary kernel modules (such as `option`, `usbnet`, and `cdc_ether`) are loaded[cite: 1].

```bash
lsmod | grep -E "option|usbnet|cdc_ether"

```

**Expected Output Example:**

```text
Module                  Size  Used by
cdc_ether              24576  0
usbnet                 40960  1 cdc_ether
option                 65536  2
usb_wwan               24576  1 option

```


## 6. Modem Management via mmcli

`mmcli` is the command-line interface for ModemManager. Use it to inspect the 5G stack.

### List Detected Modems

```bash
mmcli -L

```

You should see an output similar to: `/org/freedesktop/ModemManager1/Modem/0 [Quectel] RM520N-GL`.

### Detailed Modem Inspection

Query the full hardware and network status using the modem index (e.g., `0`).

```bash
mmcli -m 0

```

#### Key Parameters to Monitor:

* **Hardware:** Confirm `model: RM520N-GL` and `supported: gsm-umts, lte, 5gnr`.


* **System:** Ensure drivers like `option` and `qmi_wwan` are active.


* **Status:** Look for `state: registered` and `power state: on`.


* **3GPP:** Verify the `operator name` and ensure `packet service state` is `attached`.



## 7. Advanced Status (Bands & Modes)

The RM520N-GL supports a wide array of 5G NR (Next Radio) bands. You can verify the currently allowed and preferred modes in the `Modes` section of the `mmcli -m 0` output to ensure 5G is enabled.

---

**Troubleshooting Tip:** If the modem is not detected by `mmcli`, ensure the `ModemManager` service is running (`sudo systemctl status ModemManager`) and the module is correctly powered.

```
