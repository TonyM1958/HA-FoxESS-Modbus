<h2 align="center">
   <a href="https://www.fox-ess.com">FoxESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>

** Please note: as of April 2023, @nathanmarlor has mirrored and extended functionality, including easier setup in this alternative https://github.com/nathanmarlor/foxess_modbus - you should likely use that instead!

---

This is a refactored version of https://github.com/StealthChesnut/HA-FoxESS-Modbus. The changes from the main branch include:

* Simplified configuration: only 4 lines required in your HA configuration.yaml with modbus, sensors, templates and utility_meter integrations split into separate include files with variants for RS485 and LAN connections
* Support for KH series inverter using modbusLAN with RS485 to Wifi/LAN adapter
* Rename of modbusUSB to modbusRS485 to reflect the data available via the different inverter connections
* Parameterization and grouping of sensor scan_interval so they are more uniform and easier to change in a consistent way when required
* Revised calculation for inverter power in, power out and system losses
* Addition of HA templates for calculating inverter efficiency, cell imbalance, grid dependency and grid balance
* Addition of RPower and EPS RVolt, EPS RCurrent and EPS RPower sensors for LAN and RS485 connections
* Addition of BMS Cycle Count and rename / rescale of BMS Watthours Total to BMS kWh Total for consistency when working with energy values
* Addition of inverter running totals and daily totals for Solar Energy, Battery Charge, Battery Discharge, Grid Consumption Energy, Feed In Energy, Total Yield and (Battery) Input Energy. These can be used to replace Riemann sum approximations previously used giving greater accuracy and alignment with Fox cloud data.
* Added unique_id for all entities to allow management in the HA UI and aid migration to other integrations
* Correction of InvBatCurrent and InvBatPower sensors for LAN and RS-485 connections
* Correction of Temp to BatCurrent sensor for RS485
* Correction of unique_id for BMS Cell mV low

Access to your inverter data can be acheived in two ways:

* Connecting the inverters LAN port to your router/switch (no additional hardware but Manager firmware 1.57 or later required for H1/AC1). This connection provides a limited set of real time data, sufficient for general management of your equipment.
* Connecting to the inverter's RS485 modbus using an RS485 to USB adapter or RS485 to WIFI/LAN adapter. Note: this requires basic electronics competencies to connect two wires to the inverters CT / COM connector. This connection provides a more comprehensive set of real time data, including BMS cycle count and cell range info and inverter running totals.


---


## Ethernet connection to H1, AC and AIO series inverters
* Just plug the inverter ethernet port into your network and assign a static IP address. Make a note of the IP address.
* Edit your secrets.yaml file to add the inverter IP address
* Select modbusLAN.yaml and templateLAN.yaml for the integrations in your HA configuration (see below)

## RS485 connection to H1, AC and AIO series inverters (recommended)
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Edit modbusRS485.yaml to select the rtu connection type if you are using an RS485 to USB adapter
* Select modbusRS485.yaml and templateRS485.yaml for the integrations in your HA configuration (see below)

## RS485 connection to KH series inverter

* KH does not have a LAN port, but presents LAN type sensors over RS485. Setup an RS485 to Wifi/LAN adapter connected to your inverter as described in the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Edit your secrets.yaml file to add the inverter IP address
* Select modbusLAN.yaml and templateLAN.yaml for the integrations in your HA configuraiton (see below)

## Home Assistant Installation Steps

* Create a full backup of your HA instance
* Mirror the folders / files provided in custom_components in the folder CONFIG/custom_components on your HA host (for example, using either the File Editor or Studio Code Server add-ons)
* The folder custom_components/HA-FoxESS-Modbus contains files that include template text for updating the secrets.yaml and configuration.yaml files in your HA CONFIG folder 
* Open template_secrets.yaml and copy the settings into your HA secrets.yaml file
* Update your secrets.yaml with your IP address details or USB adapter port if required
* Open tempate_configuration.yaml and copy the settings into your HA configuration.yaml file. Note: if you have a complex HA configuration that already uses some of the integrations, you will need to modify the settings. Basic info on doing this is provided in the file template_configuraiton.yaml. As you got this far already, you should probably know what to do!
* Comment / uncomment the connection method you are using (see above, chose either modbusRS485.yaml and templateRS485.yaml or modbusLAN.yaml and templateLAN.yaml for the integrations)
* Go to Developer Tools and check your configuration is valid (if not, correct the problem) and then Restart HA
* Go to Settings / Devices & Services / Entities and check the entities that are now available.
* Add the required entities to your dashboard(s)

