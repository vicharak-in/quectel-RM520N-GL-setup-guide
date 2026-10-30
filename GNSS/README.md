# GNSS 

## 1. Hardware Connection
To ensure optimal signal reception and module performance, follow these physical installation steps:

* **Port:** Connect the passive GPS antenna to **ANT3** of the module.
* **Orientation:** Ensure the **lettered side** of the antenna is facing **down**.
* **Placement:** Place the antenna in an **open outdoor area** with a clear line of sight to the sky. Obstructions like buildings or heavy foliage can significantly degrade signal quality.

<img width="875" height="638" alt="image" src="https://github.com/user-attachments/assets/5328590e-24ca-40b8-8202-0e3ff81b96a8" />

-----

**W_DISABLE1** is connected to **GPIO2_B7** : Linux GPIO Number : **15**

To Low GPIO Pin run below command : 
```bash
sudo gpioset gpiochip2 15=0
```
To High GPIO Pin run below command : 
```bash
sudo gpioset gpiochip2 15=1
```

**W_DISABLE2** is connected to **GPIO2_B6** : Linux GPIO Number : **14**

To Low GPIO Pin run below command : 
```bash
sudo gpioset gpiochip2 14=0
```
To High GPIO Pin run below command : 
```bash
sudo gpioset gpiochip2 14=1
```

## 2. Software Configuration (AT Commands)
Use the **AT port** to send the following commands to control the GNSS engine.

| Command | Description | Notes |
| :--- | :--- | :--- |
| `AT+QGPS=1` | **Open GPS positioning** | Initializes the GNSS engine. |
| `AT+QGPSLOC=0` | **Get GPS position** | Retrieves the current latitude, longitude, and time. |
| `AT+QGPS=0` | **Close GPS position** | Disables the GNSS engine to save power. |


<img width="897" height="864" alt="image" src="https://github.com/user-attachments/assets/d399403d-a3e0-4e3e-940d-3e58b616310b" />

### Module is connected to PCIe
<img width="1107" height="288" alt="image" src="https://github.com/user-attachments/assets/803cd069-c7d3-4d10-abe5-6f0a5f00d9e4" />


# Quectel RM520N-GL: PCIe Detection, MHI Driver Setup & GNSS Guide

A step-by-step technical guide for bringing up the Quectel RM520N-GL 5G module over PCIe, verifying MHI driver endpoints, handling network connectivity, and configuring GNSS NMEA sentence streaming.

---

## 1. Confirm Hardware Detection over PCIe

Connect the Quectel RM520N-GL module to your host system or carrier board via the M.2 / PCIe interface and power on the system.

Verify that the PCIe hardware endpoint is detected by the kernel:

```bash
# Check Quectel PCIe device enumeration
lspci | grep -i qual

# Alternatively, search by Quectel PCI Vendor ID (1eac)
lspci -nn | grep -i 1eac
```

**Expected Output:**
```text
0002:20:00.0 PCI bridge [0604]: Rockchip Electronics Co., Ltd RK3588 [1d87:3588] (rev 01)
0002:21:00.0 Unassigned class [ff00]: Qualcomm Technologies, Inc Device [17cb:0308]
```

> **Note:** Successful output confirms that power rails, reset lines (PERST#), reference clocks (REFCLK), and PCIe lanes are properly negotiated.

---

## 2. Load the MHI Driver & Enumerate Device Nodes

The RM520N-GL utilizes Qualcomm's **Modem Host Interface (MHI)** protocol over PCIe. Ensure the kernel module (`mhi_pci` or Quectel's out-of-tree MHI driver) is loaded.

```bash
# Check if MHI kernel modules are loaded
lsmod | grep mhi

# If not loaded, load manually
sudo modprobe mhi_pci
```

Once loaded, check for the exposed MHI character device nodes:

```bash
ls -l /dev/mhi_*
```

**Typical Device Nodes:**
```text
crw------- 1 root root 510, 0 /dev/mhi_BHI       # Boot Host Interface (firmware loading)
crw------- 1 root root 511, 1 /dev/mhi_DIAG      # Qualcomm diagnostic interface
crw------- 1 root root 511, 3 /dev/mhi_DUN       # Dial-Up Networking / AT command interface
crw------- 1 root root 511, 0 /dev/mhi_LOOPBACK  # Loopback / NMEA GNSS stream interface
crw------- 1 root root 511, 2 /dev/mhi_QMI0      # QMI / control endpoint
```

---

## 3. Enable GNSS and Stream NMEA Data

Once the base driver and AT interfaces are functional, enabling GNSS is straightforward via standard Quectel AT commands.

### 4.1. Send AT Commands

Open your AT terminal on `/dev/mhi_DUN`:

1. **Enable GNSS Engine:**
   ```text
   AT+QGPS=1
   OK
   ```

2. **Configure GNSS Output Mode & Target Port:**
   ```text
   AT+QGPSCFG="outport","usbnmea"
   OK
   ```

3. **Verify GNSS Engine Status:**
   ```text
   AT+QGPS?
   +QGPS: 1
   OK
   ```

4. **Query Location Directly (Polled Mode):**
   ```text
   AT+QGPSLOC?
   +QGPSLOC: 054236.0,37.7749,-122.4194,1.5,12.0,3,180.0,0.0,0.0,100926,08
   OK
   ```

---

### 4.2. Stream Raw NMEA Sentences

Read continuous NMEA sentences directly from the MHI loopback device node:

```bash
cat /dev/mhi_LOOPBACK
```

**Expected NMEA Stream:**
```text
$GNRMC,054236.00,A,3746.494,N,12225.164,W,0.02,180.0,100926,,,A*76
$GNGGA,054236.00,3746.494,N,12225.164,W,1,08,1.2,12.0,M,-28.0,M,,*42
$GPGSV,3,1,11,01,65,045,38,03,42,120,34,08,22,280,29,11,15,310,25*78
```

---

### 4.3. Integrate with GPS Daemon (`gpsd`)

To distribute GPS/GNSS data across applications, bind the stream to `gpsd`:

```bash
# Start gpsd listening on the MHI NMEA node
sudo gpsd -N -D 2 -n /dev/mhi_LOOPBACK

# In another terminal, inspect live satellite telemetry
cgps -s
# or
gpsmon
```

---

## Summary Reference Table

| Function | Interface / Command | Purpose |
|---|---|---|
| **Bus Detection** | `lspci \| grep -i 1eac` | Verify hardware link state over PCIe |
| **Driver Nodes** | `/dev/mhi_*` | Character endpoints for control and telemetry |
| **AT Commands** | `/dev/mhi_DUN` | Send AT control strings (`AT+QGPS=1`, etc.) |
| **NMEA Output** | `/dev/mhi_LOOPBACK` | Stream NMEA `$GPGGA`, `$GPRMC`, `$GPGSV` |
| **Data Session** | `quectel-CM` (`mhi_swip0`) | Mobile broadband IP link |

