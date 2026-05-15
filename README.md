# Initial RM520N_GL Setup and Usage Guide

This module supports two different communication interfaces: **PCIe** and **USB**.

Each interface requires different driver support and configuration procedures for proper operation. 
Detailed documentation for both methods is provided separately in their respective folders, including driver installation, setup instructions, configuration steps, and usage guidelines.


### Setup Guide

"To use this **module with the Vicharak HAT**, ensure the module is securely attached on B-Key Connector while the Axon is powered off. Only then should you turn the Axon on."

### Note

The R520N-GL is designed for data only. And it do not support the voice call.
Quectel won’t open the feature for it even later.
If you must try it, you can try
AT+QCALLCFG=“voice_disable”,2
But it is not recommended. If you need the voice call feature, it is better to contact with the sales for another Quectel modem.
Refernce : https://forums.quectel.com/t/voice-call-with-the-rm520n-gl/34404/15?u=avi951

<img width="769" height="567" alt="image" src="https://github.com/user-attachments/assets/f1822752-5b8a-405c-a76f-047555710111" />


### AT Command 

| Command | Description | Return |
| :--- | :--- | :--- |
| **AT** | AT Test Command | OK |
| **ATE** | ATE1 sets echo<br>ATE0 closes echo | OK |
| **AT+CGMI** | Query module manufacturer | OK |
| **AT+CGMM** | Query module model | OK |
| **AT+CGSN** | Query product serial number | OK |
| **AT+CSUB** | Query module version and chip | OK |
| **AT+CGMR** | Query the firmware version serial number | OK |
| **AT+IPR?** | Set the module hardware serial port baud rate | +IPR:<br>OK |
| **AT+CFUN=1,1** | Reset module | OK |
| **AT+QUIMSLOT?** | Query SIM card selection:<br>Return 1, select SIM card 1;<br>Return 2, select SIM card 2 | +QUIMSLOT:<br>1/2<br>OK |
| **AT+CPIN?** | Query the status of the SIM card, return READY, the SIM card can be recognized normally | +CPIN: READY |
| **AT+COPS?** | Query the current operator, the operator information will be returned after normal networking | +COPS:<br>OK |
| **AT+CEREG?** | Query network registration status | +CEREG:<br>OK |
| **AT+C5GREG?** | Query 5G network registration status | +C5GREG:<br>OK |
| **AT+QENG="servingcell"** | Query UE system information | |
| **AT+QNWPREFCFG=?** | Network mode selection command:<br>"mode_pref": Automatic<br>"nr5g_band": 5G NR<br>"lte_band": LTE only<br>"gw_band": WCDMA only<br>... .... | OK |
