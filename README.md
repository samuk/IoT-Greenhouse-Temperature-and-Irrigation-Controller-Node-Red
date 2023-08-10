# IoT-Mqtt-Temperature-Controller-Node-Red
Node-Red full programming for greenhouse IoT automation using Raspberry Pi.

A JSON programming code (deploy after setting your personalized configuration) to control the temperature in a greenhouse based on data from Wi-Fi sensors connected through MQTT. 

In addition by setting personalized parameters is possible to store the data in a MySQL database and download them in a CSV format.
//Experiment Flow------------------------------
//AFter unexpected reboot the system must be initialized with previously 
//parameters set by user, this information is extracted from

set the email where alerts will be sent

send phone in case of want telegram alerts

//apply the configuration selected and extracted from MySQL after unexpected reboot
//Alarms-------------------------------
uncomment and add telegram node if you want to send it 
//Environment--------------------
//In case of some sensor fails, it will take data from the other one placed close
//Get data from sensor and check if fails or not
//Calculate dew point avg & vpd
//Ventilation
//User can set the reference values for temperature control
//fref1 = Temperature during the day
//fref2 = Temperature to step change and prevent high gradient rate
//fref3 = Temperature during the night


