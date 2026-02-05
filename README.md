<a href="https://www.buymeacoffee.com/tonym1958" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174" align="right"></a>
This website, code and contents are not supported by or endorsed by Fox ESS. Anyone using the information provided here does so of their own volition and risk.
##

<h2 align="center">
   <a href="https://www.fox-ess.com">Fox ESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>

---

** Please note: April 2023, this intergation only has limited maintenance and is not being updated to support new inverter models.
@nathanmarlor has mirrored and extended functionality in this alternative integration: https://github.com/nathanmarlor/foxess_modbus

---

This site contains code and information for integrating Fox ESS inverters with Home Assistant. It is a refactored and evolved version, derived from https://github.com/StealthChesnut/HA-FoxESS-Modbus.

The changes from the main branch include:

* Simplified configuration: fewer lines are required in your configuration.yaml with 'modbus', 'sensors', 'templates' and 'utility_meter' integrations split into separate include files
* Support for single phase inverters (including H1, AC1 AIO and KH series) and three phase inverter (H3 and AC3 series)
* Parameterization and grouping of sensor scan_interval so they are more uniform and easier to change consistently when required
* Revised calculation for inverter power in, power out and system losses
* Added HA templates for calculating inverter efficiency, cell imbalance, grid dependency and grid balance
* Added RPower and EPS RVolt, EPS RCurrent and EPS RPower sensors for LAN and RS485 connections
* Added BMS Cycle Count and rename / rescale of BMS Watt-hours Total to BMS kWh Total for consistency when working with energy values
* Added inverter energy meter total / today register values for PV Energy, Charge Energy, Discharge Energy, Grid Consumption Energy, Feed In Energy, Output Energy and Input Energy. These replace the Riemann sum values for RS485, giving greater accuracy and alignment with Fox cloud data. There is a code variant for these values to be used for the utility meters that feed the energy dashboard.
* Added unique_id for all entities to allow management in the HA UI and aid migration to other integrations
* Added HA templates for Battery Specification, Capacity, Cell Imbalance, Temperature Imbalance, Duration and Production, State of Discharge (SoD), State of Health (SoH), Energy, Energy per Cycle and Life Remaining. These are dynamic if BMS data is available.
* Added template for PV Voltage (sum of PV1 - PV4 voltage) for monitoring max DC input voltage
* Added entities for inverter model and firmware versions and BMS / battery firmware versions
* Corrected InvBatCurrent and InvBatPower sensors for LAN and RS-485 connections
* Corrected Temp to BatCurrent sensor for RS485
* Corrected unique_id for BMS Cell mV low
* Added automation to upload generation and consumption data to pvoutput.org

Access to your inverter data can be achieved by connecting the inverter's RS485 Modbus to an RS485 to USB adapter or RS485 to WiFi/LAN adapter. Note: this requires basic electronics competencies to connect 2 wires to the inverter's CT / COM connector.


## RS485 connection to H1, AC, AIO, H1-G2, AC-G2 and KH series inverters (recommended)
* Hardware configuration instructions can be found on Nathan's [wiki](https://github.com/nathanmarlor/foxess_modbus/wiki)
* Connect RS485A to pin 4 and RS485B to pin 3 of the Meter/CT/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to the inverter front panel Settings, Communication, RS485, Device ID and check the slave ID for the inverter is set to 247
* Use 'modbusXX_RS485.yaml' when using an RS485 to USB adapter
* Use 'modbusXX_RS485_LAN.yaml' when using an RS485 to Wifi/LAN adapter
* **where XX is replaced by:** 'H1G1' for H1, AC1 or AIO series, 'H1G2' for H1-G2 or AC-G2 series, 'KH' for K series pre Manager 1.19 firmware, 'KH119' for K series with Manager 1.19 to 1.32, KH133 for K series with Manager 1.33 or later.

## RS485 connection to H3 or AC3 series inverter
* Hardware configuration instructions for connection to RS485 can be found on the [wiki](https://github.com/nathanmarlor/foxess_modbus/wiki)
* Connect RS485A to pin 1 and RS485B to pin 2 of the Meter/RS485 connector using a suitable length of UTP cable (e.g. network cable)
* Go to inverter front panel, Settings, Communication, RS485, Device ID and set the slave ID for the inverter to 247 (the default is 000)
* Use 'modbusH3_RS485.yaml' when using an RS485 to USB adapter
* Use 'modbusH3_RS485_LAN.yaml' when using an RS485 to Wifi/LAN adapter

## Home Assistant Installation Steps

* See [Installing Home Assistant on a USFF PC](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-Home-Assistant-on-a-USFF-PC) for info on setting up a host computer to run Home Assistant
* Create a full backup of your HA instance
* See [Installing and Configuring HA-FoxESS-Modbus](https://github.com/TonyM1958/HA-FoxESS-Modbus/wiki/Installing-and-Configuring-HA-FoxESS-Modbus) for info on setting up the Fox ESS integration in Home Assistant

## Change Log

v1.6.6<br>
Update Battery Throughput with latest warranty details (UK v4.2 20250313)

v1.6.5<br>
Fix problem with renamed sensor pv_power_sum.

v1.6.4<br>
Added System Efficiency Total template sensor.
Removed Certificate Expiry template sensor as no longer required with Tailscale VPN.

v1.6.3<br>
Fix initialisation problem with certificate expiry template.
Update remote power input to use kW instead of W.
Add Remote Enable button and Remote Time and Remote Power inputs.
Add automations and UI button for remote control enable and disable.
Add 'sensor.eps_mode' and fix load power calculation when using EPS.

v1.6.0<br>
Add "RTC Hour" to allow tracking of when time changes when clocks go forwards / backwards.
Add BMS 1 Cell temps and mV sensors to H1-G2.
Start adding new sensors for KH with Manager 1.33 or later (not complete or tested).
Upate address for System Enable on H3.
Rename 'Force Charge' to 'Battery Hold' when decoding charge times to avoid confusion with Force Charge in schedules.

v1.5.9<br>
Increase SoC scan rate from low to medium.
Correct unique_id for BMS 1 and 2 Salve serial numbers in Modbus v2.
Add Modbus v2 registers from specification 20240516 v1.05.
Update template.yaml to add state_class to inverter firmware version decodes to stop missing state_class notifications in HA.
Added input_number ct1_handling. Setting to 1 inverts the processing of grid_ct.
Added input_number residual_handling. Set to 0 if bms_kwh_remaining is not available, 1 when bms_kwh_remaining is available, 2 when bms_kwh_remaining returns current bayyery capacity.
Moved input_datetime and input_number configuration to include files to simplify configuration.yaml.
Update battery energy allowances to match Battery Warranty Policy 1.4 (June).
Updated battery ageing (when capacity is not available) to use exponential decay instead of linear decay.
Updated sensor.ct2_power_now to check sensor.meter_2_connection_code to set state
Updated sensors Master, Slave and Manager version to allow hex decode. Please see [this issue](https://github.com/TonyM1958/HA-FoxESS-Modbus/issues/23) if the sensors are unavailable after updating.
Added pv_power_total, pv_total_daily that provide the total PV power and energy including CT2.
Removed check for load_power < 0 to avoid masking load power calculation errors caused by incorrect CT clamps.
Added 'modbusKH119_RS486.yaml' and 'modbusKH119_RS485_LAN.yaml' for testing.
Updated scan intervals for Max Soc and Min Soc.
Removed obscure sensors that are not used to reduce modbus scans.
Added 'modbusH1G1_RS485.yaml' for testing. This uses holding registers, where available, instead of input registers.
Add Language Code / Language Settings at 40007 for various models. TBC, may only update on inverter start-up?
Fix input_type for EPS RCurrent, RPower and RFrequency.

v1.4.9:<br>
Update H3 registers by cross-reference to other inverter models. Addresses to be confirmed are marked tbc in 'modbusH3_RS485_LAN.yaml'
Fix errors in 'template.yaml' and 'templateH1G2.yaml' affecting Battery Warranty Remaining and Battery Life Remaining.<br>
Improve handling of unknown values in 'template.yaml' and 'templateH1G2.yaml'.<br>
Setting for the number of days to keep moved to 'secrets.yaml'. See [note](https://github.com/TonyM1958/HA-FoxESS-Modbus/issues/20) for info on configuring this.
Updates to 'modbusH1G2_RS485.yaml', 'templateH1G2.yaml' and 'viewsH1G2_sensor.yaml'.
Added 'templateH1G2.yaml' to support H1-G2 and AC-G2 inverters.
Moved preset values to input helpers to avoid overwriting values during HACS update. This requires some additional settings to be added to configuration.yaml. See 'template_configuration.yaml'.<br>
See [note](https://github.com/TonyM1958/HA-FoxESS-Modbus/issues/19) for info on configuring this.
Moved database settings to recorder_ha.yaml and recorder_maria.yaml so the database being used for history does not change during HACS update.<br>
See [note](https://github.com/TonyM1958/HA-FoxESS-Modbus/issues/18) if you get a configuration error after updating.
Added 'modbusH1G2_RS485.yaml' and 'modbusH1G2_RS485_LAN.yaml' to support H1-G2 and AC-G2 series inverters.<br>
