<h2 align="center">
   <a href="https://www.fox-ess.com">Fox ESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>


This site contains code and information for integrating Fox ESS inverters with Home Assistant. It is a refactored and evolved version, derrived from https://github.com/StealthChesnut/HA-FoxESS-Modbus.

The changes from the main branch include:

* Simplified configuration: only 4 lines required in your HA configuration.yaml with modbus, sensors, templates and utility_meter integrations split into separate include files
* Support for single phase inverters (including H1, AC1 AIO and KH series) and three phase inverter (H3 and AC3 series)
* Parameterization and grouping of sensor scan_interval so they are more uniform and easier to change in a consistent way when required
* Revised calculation for inverter power in, power out and system losses
* Added HA templates for calculating inverter efficiency, cell imbalance, grid dependency and grid balance
* Added RPower and EPS RVolt, EPS RCurrent and EPS RPower sensors for LAN and RS485 connections
* Added BMS Cycle Count and rename / rescale of BMS Watthours Total to BMS kWh Total for consistency when working with energy values
* Added inverter energy meter total / today register values for PV Energy, Charge Energy, Discharge Energy, Grid Consumption Energy, Feed In Energy, Output Energy and Input Energy. These replace the Riemann sum values for RS485, giving greater accuracy and alignment with Fox cloud data. There is a code variant for these values to be used for the utility meters that feed the energy dashboard.
* Added unique_id for all entities to allow management in the HA UI and aid migration to other integrations
* Added HA templates for battery capacity, min soc and battery remaining. These are dynamic if BMS data is available.
* Added entities for inverter model and firmware versions and BMS / battery firmware versions
* Corrected InvBatCurrent and InvBatPower sensors for LAN and RS-485 connections
* Corrected Temp to BatCurrent sensor for RS485
* Corrected unique_id for BMS Cell mV low

Access to your inverter data can be acheived in two ways:

* Connecting the inverters LAN port, where available, to your router/switch (no additional hardware but Manager firmware 1.57 or later required for H1/AC1). This connection provides a limited set of real time data, sufficient for general management of your equipment.
* Connecting the inverter's RS485 modbus to an RS485 to USB adapter or RS485 to WIFI/LAN adapter. Note: this requires basic electronics competencies to connect 2 wires to the inverters CT / COM connector. This connection is recommended as it provides more comprehensive real time data, including BMS cycle count and cell range info and inverter running totals.

---
** Please note: April 2023, @nathanmarlor has mirrored and extended functionality in this alternative https://github.com/nathanmarlor/foxess_modbus - you may wish to look at that as well!

---


## RS485 connection to H1, AC, AIO and KH series inverters (recommended)
* Hardware configuration instructions can be found on the [wiki](https://github.com/nathanmarlor/foxess_modbus/wiki)
* Connect RS485A to pin 4 and RS485B to pin 3 of the Meter/CT/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to the inverter front panel Settings, Communication, RS485, Device ID and check the slave ID for the inverter is set to 247
* Use modbusH1_RS485.yaml for H1, AC and AIO series in your configuration and select the USB Connection type for RS485 to USB adapter or the LAN Connection type for RS485 to Wifi/LAN adapter
* Use modbusKH_RS485.yaml for KH series in your configuration and select the USB Connection type for RS485 to USB adapter or the LAN Connection type for RS485 to Wifi/LAN adapter

## RS485 connection to H3 or AC3 series inverter
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/nathanmarlor/foxess_modbus/wiki)
* Connect RS485A to pin 1 and RS485B to pin 2 of the Meter/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to inverter front panel, Settings, Communication, RS485, Device ID and set the slave ID for the inverter to 247 (the default is 000)
* Use modbusH3_RS485.yaml in your configuration and select the USB Connection type for RS485 to USB adapter or the LAN Connection type for RS485 to Wifi/LAN adapter

## Ethernet connection to H1, AC and AIO series inverters
* Plug the inverter ethernet port into your network and assign a static IP address. Make a note of the IP address. Note: a restricted data set of is provided via this port and many sensors will not be available, including your inverter energy meters and battery management information.
* Go the inverter front panel, Settings, Communications, Ethernet and check the IP address is set correctly. 
* Use modbusH1_LAN.yaml in your configuration

## Home Assistant Installation Steps

* See [Installing Home Assistant on a USFF PC](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-Home-Assistant-on-a-USFF-PC) for info on setting up a host computer to run Home Assistant
* Create a full backup of your HA instance
* See [Installing and Configuring HA-FoxESS-Modbus](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-and-Configuring-HA-FoxESS-Modbus) for info on setting up the Fox ESS integration in Home Assistant

---
This web site, code and contents are not supported by or endorsed by Fox ESS. Anyone using the information provided here, does so of their own volition and risk.
