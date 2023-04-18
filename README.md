<h2 align="center">
   <a href="https://www.fox-ess.com">FoxESS</a> and<a href="https://www.home-assistant.io"> Home Assistant</a> integration via Modbus RS485
   </br></br>
   <img src="https://github.com/home-assistant/brands/raw/master/custom_integrations/foxess/logo.png" >
   </br>
</h2>


This is a refactored version of https://github.com/StealthChesnut/HA-FoxESS-Modbus. The changes from the main branch include:
* simplified configuration with just 4 lines added to configuration.yaml
* Additional RS485 sensors for BMS Cycle Count
* Additional templates for calculating inverter efficiency and cell imbalance
* Correction of sensor values for InvBatCurrent and InvBatPower
* incorporation of unique_ids for entities to allow management in the HA UI

---

Please note: as of April 2023, @nathanmarlor has mirrored and extended functionality, including easier setup in this alternative https://github.com/nathanmarlor/foxess_modbus - you should likely use that instead!
This integration is being kept open in case the information here is useful.

Connecting to your inverter can be acheived in two ways:  
* Using the inverters LAN port connected to your router/switch (no additional hardware required)  
* 22/12/22 - Manager version 1.56 appears to break the LAN connectivity option!
  
* Connecting to the COM port using a [RS485 to USB](https://www.amazon.co.uk/dp/B078X5H8H7?ref_=cm_sw_r_cp_ud_dp_CR8FQK7A50FNCH530QJP) adapter or [WIFI/LAN RS485](https://www.amazon.co.uk/dp/B07DNWM62H?ref_=cm_sw_r_cp_ud_dp_BPWX7Z53PDES4WJ9JY89) converter  

⚠️ Additional hardware requires basic electronics competencies to connect the two additional wires for the RS485 interface to the inverters com connector.⚠️

---

## Manual Specific installation
* Hardware configuration instructions can be found on the [wiki](https://github.com/StealthChesnut/HA-FoxESS-Modbus/wiki/)
* Copy the Required modbus file (USB or LAN) file to /config/custom_components/HA-FoxESS-Modbus/modbusLAN.yaml

## Then, Common Installation Steps

* Create a full backup of your HA instance including the configuration.yaml file
* Copy the Required modbus line (USB or LAN) and following contents of the [configuration.yaml](https://github.com/StealthChesnut/HA-FoxESS-Modbus/blob/main/custom_components/HA-FoxESS-Modbus/configuration.yaml) file to your config file
* For LAN, create your Secrets file entry
* Check your config is valid, then Restart HA
* Map energy dashboard as per below example and enjoy configuring dashboards using near realtime data.

