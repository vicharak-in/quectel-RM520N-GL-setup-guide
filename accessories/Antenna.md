# Antenna Interfaces

## Pin Definition

The pin definition of antenna interfaces is shown below.

### RM520N-GL Pin Definition of Antenna Interfaces

| Pin Name | I/O | Description | LB (MHz) | MHB (MHz) | UHB (MHz) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **ANT0** | AIO | **Antenna 0 interface:**<br><br>**5G NR:**<br>- Refarming: LB TX0 /PRX & MHB TX0 /PRX & UHB TX1/DRX<br>- n41 TX0/PRX<br>- n77/n78/n79 TX1/DRX<br>**LTE:** LB TX0/PRX & MHB TX0/PRX & UHB TX1/DRX<br>**WCDMA:** LMB TRX | 600–960 | 1427–2690 | 3300–5000 |
| **ANT1** | AIO | **Antenna 1 interface:**<br><br>**5G NR:**<br>- Refarming: MHB PRX MIMO & UHB PRX MIMO<br>- n41 PRX MIMO<br>- n77/n78/n79 PRX MIMO<br>**LTE:** MHB PRX MIMO & UHB PRX MIMO & LAA PRX<br>**GNSS:** L5 | - | 1176–2690 | 3300–5925 |
| **ANT2** | AIO | **Antenna 2 interface:**<br>**5G NR:**<br>- Refarming: MHB TX1 <sup>22</sup>/DRX MIMO & UHB TX0/PRX<br>- n41 TX1/DRX MIMO<br>- n77/n78/n79 TX0/PRX<br>**LTE:** MHB TX1 <sup>22</sup>/DRX MIMO & UHB TX0/PRX | - | 1710–2690 | 3300–5000 |
| **ANT3** | AIO | **Antenna 3 interface:**<br>**5G NR:**<br>- Refarming: LB TX1 <sup>23</sup> / DRX & MHB DRX & UHB DRX MIMO<br>- n41 DRX<br>- n77/n78/n79 DRX MIMO<br>**LTE:** LB TX1 <sup>24</sup>/DRX & MHB DRX & UHB DRX MIMO & LAA DRX<br>**WCDMA:** LMB DRX<br>**GNSS:** L1 | 600–960 | 1427–2690 | 3300–5925 |

#### Reference Doc : doc/Quectel_RM520N_Series_Hardware_Design_V1.4.pdf
