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
