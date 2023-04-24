<h2 align="center">
   <a href="https://www.fox-ess.com">FoxESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>

** Please note: as of April 2023, @nathanmarlor has mirrored and extended functionality, including easier setup in this alternative https://github.com/nathanmarlor/foxess_modbus - you should likely use that instead!

---

This is a refactored version of https://github.com/StealthChesnut/HA-FoxESS-Modbus. The changes from the main branch include:

* Simplified configuration: only 4 lines required in your HA configuration.yaml with modbus, sensors, templates and utility_meter integrations split into separate include files
* Support for single phase inverters including H1, AC1 AIO and KH series
* Parameterization and grouping of sensor scan_interval so they are more uniform and easier to change in a consistent way when required
* Revised calculation for inverter power in, power out and system losses
* Added HA templates for calculating inverter efficiency, cell imbalance, grid dependency and grid balance
* Added RPower and EPS RVolt, EPS RCurrent and EPS RPower sensors for LAN and RS485 connections
* Added BMS Cycle Count and rename / rescale of BMS Watthours Total to BMS kWh Total for consistency when working with energy values
* Added running and daily totals for PV Energy, Charge Energy, Discharge Energy, Grid Consumption Energy, Feed In Energy, Output Energy and Input Energy. These replace Riemann sum approximations for RS485 giving greater accuracy and alignment with Fox cloud data. Where available, these sensors are aliased to the Riemann sums for consistency when using a LAN connection.
* Added unique_id for all entities to allow management in the HA UI and aid migration to other integrations
* Added HA templates for battery capacity, min soc and battery remaining. These are dynamic if BMS data is available.
* Added entities for inverter model and firmware versions and BMS / battery firmware versions
* Corrected InvBatCurrent and InvBatPower sensors for LAN and RS-485 connections
* Corrected Temp to BatCurrent sensor for RS485
* Corrected unique_id for BMS Cell mV low

Access to your inverter data can be acheived in two ways:

* Connecting the inverters LAN port to your router/switch (no additional hardware but Manager firmware 1.57 or later required for H1/AC1). This connection provides a limited set of real time data, sufficient for general management of your equipment.
* Connecting to the inverter's RS485 modbus using an RS485 to USB adapter or RS485 to WIFI/LAN adapter. Note: this requires basic electronics competencies to connect two wires to the inverters CT / COM connector. This connection provides a more comprehensive set of real time data, including BMS cycle count and cell range info and inverter running totals.


---


## Ethernet connection to H1, AC and AIO series inverters
* Just plug the inverter ethernet port into your network and assign a static IP address. Make a note of the IP address.
* Edit modbusH1.yaml to select the modbus tcp connection for H1, AC and AIO inverters.
* Edit your secrets.yaml file to add the inverter IP address

## RS485 connection to H1, AC, AIO and KH series inverters (recommended)
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Edit modbusH1.yaml to select the modbus rtu connection type with RS485 to USB adapter or the modbus tcp connection type for RS485 to Wifi/LAN adapter
* Edit your secrets.yaml file to add the inverter IP address if required

## RS485 connection to H3 series inverter
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Edit modbusH3.yaml to select the modbus rtu connection type with RS485 to USB adapter or the modbus tcp connection type for RS485 to Wifi/LAN adapter
* Edit your secrets.yaml file to add the inverter IP address if required

## Home Assistant Installation Steps

* Create a full backup of your HA instance
* Mirror the folders / files provided in custom_components in the folder CONFIG/custom_components on your HA host (for example, using either the File Editor or Studio Code Server add-ons)
* The folder custom_components/HA-FoxESS-Modbus contains files that include template text for updating the secrets.yaml and configuration.yaml files in your HA CONFIG folder 
* Open modbusH1.yaml and comment / uncomment the settings for your connection type: use modbus rtu for a USB connection or modbus tcp for a LAN connection
* Open template_secrets.yaml and copy the settings into your HA secrets.yaml file
* Update your secrets.yaml with your IP address details or USB adapter port if required
* Open tempate_configuration.yaml and copy the settings into your HA configuration.yaml file. Note: if you have a complex HA configuration that already uses some of the integrations, you will need to modify the settings. Basic info on doing this is provided in the file template_configuraiton.yaml. As you got this far already, you should probably know what to do!
* Select modbusH1.yaml for single phase inverters. If you have a 3 phase inverter, comment out the line with modbusH1.yaml and uncomment the line with modbusH3.yaml
* Go to Developer Tools and check your configuration is valid (if not, correct the problem) and then Restart HA
* Go to Settings / Devices & Services / Entities and check the entities that are now available.
* Add the required entities to your dashboard(s)

