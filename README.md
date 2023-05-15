<h2 align="center">
   <a href="https://www.fox-ess.com">FoxESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>

** Please note: April 2023, @nathanmarlor has mirrored and extended functionality in this alternative https://github.com/nathanmarlor/foxess_modbus - you may wish to look at that as well!

---

This is a refactored version of https://github.com/StealthChesnut/HA-FoxESS-Modbus. The changes from the main branch include:

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


## Ethernet connection to H1, AC and AIO series inverters
* Plug the inverter ethernet port into your network and assign a static IP address. Make a note of the IP address. Note: a restricted data set of is provided via this port and many sensors will not be available, including your inverter energy meters and battery management information.
* Go the inverter front panel, Settings, Communications, Ethernet and check the IP address is set correctly. 
* Edit modbusH1.yaml to select the modbus tcp connection for H1, AC and AIO inverters.
* Edit your secrets.yaml file to add the inverter IP address

## RS485 connection to H1, AC, AIO and KH series inverters (recommended)
* Hardware configuration instructions can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Connect RS485A to pin 4 and RS485B to pin 3 of the Meter/CT/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to the inverter front panel Settings, Communication, RS485, Device ID and check the slave ID for the inverter is set to 247
* Edit modbusH1.yaml to select the modbus rtu connection type with RS485 to USB adapter or the modbus tcp connection type for RS485 to Wifi/LAN adapter
* Edit your secrets.yaml file to add the inverter IP address if required

## RS485 connection to H3 or AC3 series inverter
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Connect RS485A to pin 1 and RS485B to pin 2 of the Meter/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to inverter front panel, Settings, Communication, RS485, Device ID and set the slave ID for the inverter to 247 (the default is 000)
* Edit modbusH3.yaml to select the modbus rtu connection type with RS485 to USB adapter or the modbus tcp connection type for RS485 to Wifi/LAN adapter
* Edit your secrets.yaml file to add the inverter IP address if required

## Home Assistant Installation Steps

* See [wiki](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-Home-Assistant-on-a-USFF-PC) for info on setting up a host computer to run Home Assistant
* Create a full backup of your HA instance
* Copy the folders / files provided under custom_components into the folder /config/custom_components on your HA host (for example, using either the File Editor or Studio Code Server add-ons)
* The folder custom_components/HA-FoxESS-Modbus contains files with template text for updating configuration.yaml and secrets.yaml in your /config folder 
* Open tempate_configuration.yaml and copy the settings into your HA configuration.yaml file. Note: if you have a complex HA configuration that already uses some of the integrations, you will need to modify the settings. Basic info on doing this is provided in the file template_configuration.yaml. As you got this far already, you should probably know what to do!
* Select modbusH1.yaml for single phase inverters. If you have a 3 phase inverter, comment out the line with modbusH1.yaml and uncomment the line with modbusH3.yaml
* Open modbusH1.yaml or modbusH3.yaml and update the settings for your connection type: use modbus rtu for a USB connection or modbus tcp for a LAN connection
* Open template_secrets.yaml and copy the settings into your HA secrets.yaml file
* Update your secrets.yaml with your IP address details or USB adapter port if required
* Go to Developer Tools and check your configuration is valid (if not, correct the problem) and then Restart HA
* Go to Settings / Devices & Services / Entities and check the entities that are now available.
* Add the required entities to your dashboard(s)

## Configuration

After you have installed and setup HA, configure it by editing template.yaml as follows:

* Find the sensor 'Install Date' and update the date in quotes to your installation date. This value is used when calculating the remaining battery life.
* Find the line containing _capacity = (6 * 2.56 * 0.9)_ in the sensor 'Battery Capacity'. This is your default battery capacity - change the numbers to refelct your configuration: set the first number to the number of batteries you have installed and the second number to the capacity of eacb battery in kWh (HV2600 = 2.56, HV2500 = 2.45, ECS4100 = 4.03, ESC2900 = 2.88).

Optionally, you may also configure recorder to use MariaDB instead of SQLite:

* Go to Settings, Add-ons and find MariaDB in the Add-On Store. Install and configure the add-on, making a note of your db password.
* Open your secrets.yaml file and update the mariadb_url with your db password
* Open the file recorder.yaml and uncomment the line starting _db_url_ 

When you are finished editing, close the file, go to Settings, Developer Tools, check your configuration, correct any errors and restart HA.
