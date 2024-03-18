<a href="https://www.buymeacoffee.com/tonym1958" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174" align="right"></a>
This web site, code and contents are not supported by or endorsed by Fox ESS. Anyone using the information provided here, does so of their own volition and risk.
##

<h2 align="center">
   <a href="https://www.fox-ess.com">Fox ESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>

This site contains code and information for integrating Fox ESS inverters with Home Assistant. It is a refactored and evolved version, derrived from https://github.com/StealthChesnut/HA-FoxESS-Modbus.

The changes from the main branch include:

* Simplified configuration: fewer lines are required in your configuration.yaml with modbus, sensors, templates and utility_meter integrations split into separate include files
* Support for single phase inverters (including H1, AC1 AIO and KH series) and three phase inverter (H3 and AC3 series)
* Parameterization and grouping of sensor scan_interval so they are more uniform and easier to change in a consistent way when required
* Revised calculation for inverter power in, power out and system losses
* Added HA templates for calculating inverter efficiency, cell imbalance, grid dependency and grid balance
* Added RPower and EPS RVolt, EPS RCurrent and EPS RPower sensors for LAN and RS485 connections
* Added BMS Cycle Count and rename / rescale of BMS Watthours Total to BMS kWh Total for consistency when working with energy values
* Added inverter energy meter total / today register values for PV Energy, Charge Energy, Discharge Energy, Grid Consumption Energy, Feed In Energy, Output Energy and Input Energy. These replace the Riemann sum values for RS485, giving greater accuracy and alignment with Fox cloud data. There is a code variant for these values to be used for the utility meters that feed the energy dashboard.
* Added unique_id for all entities to allow management in the HA UI and aid migration to other integrations
* Added HA templates for Battery Specification, Capacity, Cell Imbalance, Temperature Imbalance, Duration and Production, State of Discharge (SoD), State of Health (SoH), Energy, Energy per Cycle and Life Remaining. These are dynamic if BMS data is available.
* Added template for PV Voltage (sum of PV1 - PV4 voltage) for monitoring max DC input voltage
* Added entities for inverter model and firmware versions and BMS / battery firmware versions
* Corrected InvBatCurrent and InvBatPower sensors for LAN and RS-485 connections
* Corrected Temp to BatCurrent sensor for RS485
* Corrected unique_id for BMS Cell mV low
* Added automation to upload generation and consumption data to pvoutput.org

Access to your inverter data can be acheived by connecting the inverter's RS485 modbus to an RS485 to USB adapter or RS485 to WIFI/LAN adapter. Note: this requires basic electronics competencies to connect 2 wires to the inverters CT / COM connector.

---
** Please note: April 2023, @nathanmarlor has mirrored and extended functionality in this alternative https://github.com/nathanmarlor/foxess_modbus - you may wish to look at that as well!

---


## RS485 connection to H1, AC, AIO, H1-G2, AC-G2 and KH series inverters (recommended)
* Hardware configuration instructions can be found on Nathan's [wiki](https://github.com/nathanmarlor/foxess_modbus/wiki)
* Connect RS485A to pin 4 and RS485B to pin 3 of the Meter/CT/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to the inverter front panel Settings, Communication, RS485, Device ID and check the slave ID for the inverter is set to 247
* Use modbusXX_RS485.yaml when using an RS485 to USB adapter
* Use modbusXX_RS485_LAN.yaml for H1 when using an RS485 to Wifi/LAN adapter
* where XX is H1 for H1, AC1 or AIO series, H1G2 for H1-G2 or AC-G2 series or KH for K series

## RS485 connection to H3 or AC3 series inverter
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/nathanmarlor/foxess_modbus/wiki)
* Connect RS485A to pin 1 and RS485B to pin 2 of the Meter/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to inverter front panel, Settings, Communication, RS485, Device ID and set the slave ID for the inverter to 247 (the default is 000)
* Use modbusH3_RS485.yaml when using an RS485 to USB adapter
* Use modbusH3_RS485_LAN.yaml when using an RS485 to Wifi/LAN adapter

## Home Assistant Installation Steps

* See [Installing Home Assistant on a USFF PC](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-Home-Assistant-on-a-USFF-PC) for info on setting up a host computer to run Home Assistant
* Create a full backup of your HA instance
* See [Installing and Configuring HA-FoxESS-Modbus](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-and-Configuring-HA-FoxESS-Modbus) for info on setting up the Fox ESS integration in Home Assistant

## Change Log
v1.4.5: added 'templateH1G2.yaml' to support H1-G2 and AC-G2 inverters.<p>
v1.4.4: moved preset values to input helpers to avoid over-writing values during HACS update. This requires some additional settings to be added to configuration.yaml. See 'template_configuration.yaml'.<p>
v1.4.3: moved database settings to recorder_ha.yaml and recorder_maria.yaml so the database being used for history does not change during HACS update.<p>
v1.4.2: added 'modbusH1G2_RS485.yaml' and 'modbusH1G2_RS485_LAN.yaml' to support H1-G2 and AC-G2 series inverters.<p>
