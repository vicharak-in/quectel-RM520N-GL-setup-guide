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


### Antenna Requirements

| Type | Requirements |
| :--- | :--- |
| Cellular | <ul><li>VSWR: ≤ 2</li><li>Efficiency: > 30 %</li><li>Input Impedance: 50 Ω</li><li>Cable insertion loss:<ul><li>< 1 dB: LB (<1 GHz)</li><li>< 1.5 dB: MB (1–2.3 GHz)</li><li>< 2 dB: HB (> 2.3 GHz)</li></ul></li></ul> |
| GNSS | <ul><li>Frequency range:<ul><li>L1: 1559–1609 MHz</li><li>L5: 1166–1187 MHz</li></ul></li><li>Polarization: RHCP or linear</li><li>VSWR: ≤ 2 (Typ.)</li></ul><hr>**For passive antenna usage:**<ul><li>Passive antenna gain: > 0 dBi</li></ul> |

> **NOTE**
> 1. It is recommended to use a passive GNSS antenna when LTE B13 or B14 is supported, as the use of an active antenna may generate harmonics which will affect the GNSS performance.
> 2. It is not recommended to add an external LNA when using a passive antenna.
#### Reference Doc : doc/Quectel_RM520N_Series_Hardware_Design_V1.4.pdf
