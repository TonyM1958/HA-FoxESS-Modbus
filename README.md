<h2 align="center">
   <a href="https://www.fox-ess.com">FoxESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration via Modbus RS485
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>

** Please note: as of April 2023, @nathanmarlor has mirrored and extended functionality, including easier setup in this alternative https://github.com/nathanmarlor/foxess_modbus - you should likely use that instead!

---

This is a refactored version of https://github.com/StealthChesnut/HA-FoxESS-Modbus. The changes from the main branch include:

* Simplified configuration with 4 lines added into configuration.yaml for the a HA setup
* Rename of modbusUSB to modbusRS485 to properly reflect configuration required for different inverter connection methods
* Addition of templates for calculating inverter efficiency, cell imbalance, grid dependency and grid balance
* Revised calculation for inverter power in, power out and system losses
* Addition of RPower and EPS RVolt, RCurrent and RPower sensors for both LAN and RS485 connections
* Correction of InvBatCurrent and InvBatPower sensors for both LAN and RS-485 connections
* Correction of Temp to BatCurrent sensor for RS485
* Correction of unique_id for BMS Cell mV low
* Addition of BMS Cycle Count and rename of BMS Watthours Total to BMS kWh Total for consistency for RS485
* Addition of inverter running totals and daily totals for Solar Energy, Battery Charge, Battery Discharge, Grid Consumption Energy, Feed In Energy, Total Yield and (Battery) Input Energy
* Added unique_id for all entities to allow management in the HA UI and aid migration to other integrations
* Support for KH series inverter using modbusLAN with RS485 to Wifi/LAN adapter

---

Connecting to your inverter can be acheived in two ways:

* Using the inverters LAN port connected to your router/switch (no additional hardware required but requires Manager firmware 1.57 or later)  
* Connecting to the RS485 modbus using an RS485 to USB adapter or RS485 to WIFI/LAN adapter. Note: this requires basic electronics competencies to connect two wires to the inverters com connector.

---

## RS485 Connection to H1, AC and AIO series inverters (recommended)
* Hardware configuration instructions can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Select the modbusRS485.yaml file for your HA configuration (below)
* Edit modbusRS485.yaml to select the tcp connection type if you are using RS485 to Wifi/LAN adapter

## Ethernet Connection to H1, AC and AIO series inverters
* Plug the inverter ethernet port into your network and assign a static IP address. Make a note of the IP address.
* Select the modbusLAN.yaml file for your HA configuration (below)
* Edit your secrets.yaml file to add the inverter IP address

## RS485 Connection to KH series inverter

* KH does not have a LAN port, but presents LAN type sensors over RS485. Setup an RS485 to Wifi/LAN adapter connected to your inverter.
* Select the modbusLAN in your HA configuraiton (below)
* Edit your secrets.yaml file to add the inverter IP address

## Home Assistant Installation Steps

* Create a full backup of your HA instance
* Mirror the folders / files provided in custom_components in the folder CONFIG/custom_components on your HA host
* The folder custom_components/HA-FoxESS-Modbus contains files that include template text for updating the secrets.yaml and configuration.yaml files in your HA CONFIG folder 
* Open template_secrets.yaml and copy the settings into your HA secrets.yaml file
* Update your secrets.yaml with your IP address details or USB adapter port if required
* Open tempate_configuration.yaml and copy the settings into your HA configuration.yaml file. Note: if you have a complex HA configuration that already uses some of the integrations, you will need to modify the settings. Info on doing this is provided in the file template_configuraiton.yaml
* Comment / uncomment the connection method you are using (see above, chose either modbusRS485.yaml or modbusLAN.yaml)
* Go to Developer Tools and check your configuration is valid (if not, correct the problem) and then Restart HA
* Go to Settings / Devices & Services / Entities and check the entiries that are now available.
* Add the required entities to your dashboard(s)

