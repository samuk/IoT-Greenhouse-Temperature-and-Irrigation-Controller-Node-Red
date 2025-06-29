
# Fork of IoT-Greenhouse-Temperature-&-Irrigation-Controller

See details of the original project below. This project adapts that work to integrate with

[Olimex iMX8 open hardware SBC](https://www.olimex.com/Products/SOM/NXP-iMX8/iMX8MP-SOM-EVB-IND/open-source-hardware) produced until [2036](https://www.nxp.com/products/nxp-product-information/nxp-product-programs/product-longevity:PRDCT_LONGEVITY_HM)
- [Buildroot](https://github.com/OLIMEX/buildroot-imx) & [RT patch]( https://gitlab.com/buildroot.org/buildroot/-/tree/master/package?ref_type=heads) with [Docker](https://www.reddit.com/r/embedded/comments/120r5za/comment/jdy1hg0/) 
- [IOT stack](https://sensorsiot.github.io/IOTstack/) for container compose files
- [InfluxDB](https://sensorsiot.github.io/IOTstack/Containers/InfluxDB/) Container for long term logging
- [Mosquitto](https://sensorsiot.github.io/IOTstack/Containers/Mosquitto/) Container to run broker
- [MariaDB](https://sensorsiot.github.io/IOTstack/Containers/MariaDB/) Container to run MySQL 
- [Zigbee2MQTT](https://sensorsiot.github.io/IOTstack/Containers/Zigbee2MQTT/) Container for Zigbee devices, eg [B-parasite sensor](https://github.com/rbaron/b-parasite?tab=readme-ov-file)
- [Modbus2MQTT](https://github.com/Instathings/modbus2mqtt) for Modbus devices, eg [OLIMEXINO-STM32F3](https://www.olimex.com/Products/Duino/STM32/OLIMEXINO-STM32F3/open-source-hardware) & [MOD-RS485](https://www.olimex.com/Products/Modules/Interface/MOD-RS485-ISO/open-source-hardware) or [SPE](https://store.arduino.cc/products/uno-spe-shield) running [FreeRTOS](https://github.com/alejoseb/Modbus-STM32-HAL-FreeRTOS)
- [Can2MQTT](https://github.com/c3re/can2mqtt) for Canbus devices, eg [Rootsense](https://docs.openhydroponics.com/hardware/rootsense.html)
- [BACnet](https://github.com/BiancoRoyal/node-red-contrib-bacnet) Building Automation Control (BAC) communications
- [OPC-UA](https://github.com/BiancoRoyal/node-opcua?tab=readme-ov-file) Open Platform Communications.
- [ESPhome](https://community.home-assistant.io/t/solved-config-example-of-how-to-retrofit-mqtt-onto-devices-that-previously-used-api/709391/5) for MQTT from WiFi devices

Discarded ideas
[OpenWRT](https://github.com/nxp-imx/imx_openwrt/tree/imx-openwrt-24.10) (readonly FS) as [Docker host](https://openwrt.org/docs/guide-user/virtualization/docker_host) [Maybe](https://forum.openwrt.org/t/is-openwrt-worth-as-operative-system-for-high-performance-hardware/114434/9)


# IoT-Greenhouse-Temperature-&-Irrigation-Controller-Node-Red
*This Project was developed as part of the Alliance of Bioversity International & CIAT IoT initiatives.

**This is a full Node-Red flow that was initially developed for automatically controlling the air temperature inside the greenhouses (in campus Palmira, Colombia) and the irrigation of the plants for the Bean Physiology Team. However, it can be implemented in other experiment locations where temperature or irrigation must be controlled.

***This is one of the projects developed focused on the most important thing in agriculture research, the PLANTS!!!

***You will find control logic and specific programming based on many years of real experience working with plants of different crops (mainly Beans).

***Developed for beginners and experts in Node-Red programming to have quick interaction between Software and Hardware with easy-to-implement IoT web applications.

***The programming uses MQTT and MySQL connections. Developed for any Raspberry Pi use. In addition, a user-friendly dashboard was developed for all users in the world can easily interact remotely and set their own parameters according to their plants' needs.

*This project involved collaboration with many plant scientists in the Alliance of Bioversity International & CIAT. Special thanks to Drs. Milan Urban, Ph.D.; Steve Beebe, Ph.D.; for their invaluable support and funding. And Eng Harold Diaz, my colleague who began whit these implementations and developed the first version used as a basic template for this project.

*In addition, our technician Fabricio Soto and the Electronical Engineer trainees Alejandra Guzman and Sebastian Pinzon contributed to this project.

# Before starting:
- Node-Red full programming for greenhouse IoT automation using Raspberry Pi.
- The JSON programming code is able to control the temperature in a greenhouse based on data from Wi-Fi sensors connected through MQTT (deploy after setting your personalized configuration). 
- This system was tested in Raspberry Pi 4, Zero, and Zero W. However you can deploy it on Raspberry Pi 3 also.
- The version of the Node-RED was 1.3.5. However, it is possible to use it in recent versions of Node-Red. Note to in recent versions (newest NodeJs versions), the variables must be declared with the type before the name (example: var name_variable;).
- Keep into account that the programming is connected to the physical output pins of the Pi. This means that must be control actuators (fans, heat extractors, etc) connected through relays to these pins to the system can run effectively the control action automatically.
- The same with the irrigation, some valves or pumps must be controlled with relays connected to the Pi outputs.
- There are providers such as Amazon or Waveshare that offer many HAT options for Raspberry Pi with relays included. Example: https://www.waveshare.com/product/raspberry-pi/hats.htm
- In normal conditions, heat extractors are turned ON to decrease or OFF to increase the temperatures inside the greenhouses. But also for increasing the temperature, heaters can be adapted to this system.
- All devices connected through MQTT must be in the same Wi-Fi network. And the Raspberry Pi will be used as a broker.
- The sensors used were the sonoff TH Elite (https://sonoff.tech/product/diy-smart-switches/th-elite/) with a SI7021 Temperature & Humidity sensor.
- Each one of these sensors must be TASMOTIZED (recommendable using VisualStudio with the developed by Theo Arends https://github.com/arendst/Tasmota).
- However, it is possible to use this system with any MQTT sensor.
- Remember that it is necessary to install the mosquitto broker on the Raspberry Pi (https://randomnerdtutorials.com/how-to-install-mosquitto-broker-on-raspberry-pi/).
- Necessary to install MariaDB server in Raspberry Pi for MySQL use (https://raspberrytips.com/install-mariadb-raspberry-pi/).
- Remember that you need to install the missing modules that Node-Red will notify you after importing the flows.
- In addition by setting personalized parameters is possible to store the data in a MySQL database and download them in a CSV format.
- Please navigate to the center of every flow where nodes are programmed.

# The Node-Red Flows:

All flows and some nodes include comments inside to easily understand the code.

- User Parameters Flow

![App_config](App_config.png)
Web User Application - Tab settings configuration

You will find here: 

Nodes that in case of some unexpected electrical reboot the system must be initialized with previous parameters set by the user, this information is extracted from the database directly.

The flows for setting the experiment information and email where alerts will be sent.

Apply the configuration selected and extracted from MySQL after an unexpected reboot

- Alarms

You will find here:

The nodes are necessary to alert the user when temperature or humidity is over the previously set reference limits.

- Sensors & Data Integration

![App_monitoring](App_monitoring.png)
Web User Application - Tab monitoring

You will find here:

Sensors data provisional replace for precise the control logic in case of some sensor fails (because of electrical or network failure), it will take data from the other one placed closely.

Calculate the average data of the Temperature, Humidity, Dew point & VPD.

- Control

You will find here:

This is based on a system controlled by relays connected to the Raspberry Pi and an additional electrical power system to control motors.

These motors are working as heat extractors (extractors/fans) and will be controlled regarding the data from sensors and references set by the user in the dashboard.

There are four extractors (in our project) connected to pins 29,31,33 and 36 of the Raspberry Pi. We called them:

Fan left 1 (fl1), Fan left 2 (fl2), Fan right 1 (fr1), Fan right 2 (fr2).

Users can set the reference values for temperature control

fref1 = Temperature during the day. 

fref2 = Temperature to step change and prevent high gradient rate. 

fref3 = Temperature during the night.

The day and night limits were created to control the temperature in parameters near real behavior. This means that in real conditions in the field, there are higher temperatures during the day than at night. So, this affects the plants and for that reason, the programming includes the possibility for the user can set the reference temperatures to be controlled automatically in these periods daily and simulate an environment close to real scenarios.

Note to:

Between fTime2 and fTime 3 is the day; Between fTime4 and fTime1 is night; Between fTime1 and fTime2 & fTime3 and fTime4 are the steps, where the temperature should be set in a middle point to not apply high changes directly to the plants.

The heat extractors will be turned on if the temperature is 1°C above reference (tref) in that period otherwise will be turned off.

Users can also select in the dashboard if every extractor works with every tref  (day, night, step) then, the control action will use or not that specific extractor to increase or decrease the temperature.

Remember that fref1 is the temperature during the day the day and night limits can be changed in the "fTime&fRef limits control" node.

The Cooling (irrigation) flow is based on a system controlled by electro-valves which control the flow of water to irrigate the soil or the plants inside the greenhouse.

The irrigation/cooling control is connected to the pin35 in the Raspberry Pi. However, you can change it as well as you need.

- Database

You will find here:

These flows are for storing in the database.

Note that data will be stored every ten minutes. You can change this in the "Store interval" node.

Data will be saved in the table DATA (you can change this on every DataSensor node).

Remember that data must coincide with the columns in the created table.

Nodes that will help you to create your personalized MySQL table and will guide you in the process of storing data.

Nodes to download data from the MySQL table directly.

Remember that this will only download your information related to the current trial in the course selected by the user in the dashboard; This was done in that way to not overload the Raspberry Pi with downloading very heavy files.

Note that are created two files to be downloaded. One file is from the DATA database and contains all information from sensors stored. The second one is from the SETTINGS database, which stored all configurations that the user set before. Then is possible to download both files with different information but depending on the current trial name.

PLEASE MAKE SURE THAT THE TWO FILES (FROM DATA AND CONFIG) ARE NAMED CORRECTLY AND DIFFERENTLY.

We have tested Raspberry Pi under greenhouse conditions (high temperatures and humidity) and we are totally sure that these ones are robust enough to work perfectly in these hard environments. This enables the possibility to have trustable sophisticated datalogger and controllers at low-cost using IoT.

Hope this will be useful for your projects. Let´s improve the plant research process together!!!

Developed by Dpineda.

Feel free to contact me at duvanpineda09@gmail.com
