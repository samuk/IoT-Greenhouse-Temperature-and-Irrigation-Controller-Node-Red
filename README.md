# IoT-Greenhouse-Temperature-&-Irrigation-Controller-Node-Red
*This Project was developed as part of the Alliance of Bioversity International & CIAT IoT initiatives.
**Initially developed for automatically controlling the air temperature inside the greenhouses and the irrigation of the plants for the Bean Physiology Team.
***This is one of the projects developed focused on the most important thing in agriculture research, the PLANTS!!!
***You will find control logic and specific programming based on many years of real experience working with plants of different crops (mainly Beans).
***The programming uses MQTT and MySQL connections. Developed for any Raspberry Pi use. In addition, a user-friendly dashboard was developed for all users in the world can easily interact remotely and set their own parameters according to their plants' needs.

*This project involved collaboration with many plant scientists in the Alliance of Bioversity International & CIAT. Special thanks to Drs. Milan Urban, Ph.D.; Steve Beebe, Ph.D.; for their invaluable support and funding. And Eng Harold Diaz, my colleague who began whit these implementations and developed the first version used as a basic template for this project.

The Node-Red code:
- Node-Red full programming for greenhouse IoT automation using Raspberry Pi.
- The JSON programming code is able to control the temperature in a greenhouse based on data from Wi-Fi sensors connected through MQTT (deploy after setting your personalized configuration). 
- This system was tested in Raspberry Pi 4, Zero, and Zero W. However you can deploy it on Raspberry Pi 3 also.
- The version of the Node-RED was 1.3.5. However, it is possible to use it in recent versions of Node-Red. Note to in recent versions, the variables must be declared with the type before the name (example: var name_variable;).
- Keep into account that the programming is connected to the physical output pins of the Pi. This means that must be control actuators (fans, heat extractors, etc) connected through relays to these pins to the system can execute the control action automatically.
- The same with the irrigation, some valves or pumps must be controlled with relays connected to the Pi outputs.
- There are providers such as amazon or Waveshare that offer many Hat options for Raspberry Pi with relays included. example: https://www.waveshare.com/product/raspberry-pi/hats.htm
- 
In addition by setting personalized parameters is possible to store the data in a MySQL database and download them in a CSV format.
//User Parameters Flow------------------------------
//AFter unexpected reboot the system must be initialized with previously 
//parameters set by user, this information is extracted from

set the email where alerts will be sent

send phone in case of want telegram alerts

//apply the configuration selected and extracted from MySQL after unexpected reboot
//Alarms-------------------------------
uncomment and add telegram node if you want to send it 
//Sensors & Data Integration--------------------
//In case of some sensor fails, it will take data from the other one placed close
//Get data from sensor and check if fails or not
//Calculate dew point avg & vpd
//Control--------------------------
Readme
This is based on a system controlled by 
relays connected to the Raspberry Pi and an adittional electrical power system to control motors.

These motors are working as heat extractors (extractors/fans), and will be controlled regarding to the data from sensors and references set by user in dashboard.

There are four extractors connected to the pins 29,31,33 and 36 of the Raspberry Pi. We called them:
Fan left 1 (fl1)
Fan left 2 (fl2)
Fan right 1 (fr1)
Fan right 2 (fr2)


//User can set the reference values for temperature control
//fref1 = Temperature during the day
//fref2 = Temperature to step change and prevent high gradient rate
//fref3 = Temperature during the night

//The next line is for define the day and night limits
//between fTime2 and fTime 3 is day
//between fTime4 and fTime1 is night
//between fTime1 and fTime2 & fTime3 and fTime4 are the steps,
//where the temperature should be set in a middle point to do not
//apply high changes directly to the plants

//the extractors will be turned on if temperature is 1°C above from reference in that period of time
//otherwise will be turned off
   //the nex line is for use the extractors defined by user to be used with the current temp reference
    //since user selected in dashboard if every extractor work with every tref  (day,night,step)
    //then, the control action will use or not that specific extractor to 
    //increase the temperature
    
     //the nex line is for use the extractors defined by user to be used with the current temp reference
    //since user selected in dashboard if every extractor work with every tref  (day,night,step)
    //then, the control action will use or not that specific extractor to 
    //decrease the temperature

  //remember that fref1 is the temperature during the day
//the day and nights limits can be change in "fTime&fRef limits control" node

Cooling readme

This is based on a system controlled by 
electrovalves which controls the flow of water trhough the soil inside greenhouse.

This can be used also for controlling a irrigation system for the plants based on times to open or close the valves based on the needs of the plants.

The control is connected to the pin35 in the Raspberry Pi. However you can change it as well as you need.

//Set initial state to the cooling system in false till
//information be obtained from MySQL or set by user in dashboard

//get programming of the scheduler to cooler system
//be used based on defined by user limits of day and time
//this can be extracted from dashboard or MySQL depending if
//there is an update in configurations or some unexpected reboot respectively

//Get the programming set by user for the cooler system

//Control the pin output in the raspberry to turn on or off
//the cooling system in based of the schedule set by user

//note to if user selected that cooling system must be work
//on base of temperature control.
//the cooling system will be activated if the temperature
//is 3°C above from reference in that period of time
//you can change the number 3 for more or less regarding your need

//Database---------------
These flows are for storing in the database.

Note that data will be stored every ten minutes. You can change this in the "Store interval" node.

Data will be saved in the table DATA (you can change this on every DataSensor node).

Remember that data must coincide with the columns in the created table.

Here you will find also the email sending flow for alerts notification to the user.

//Insert the data into MySQL database
//Note to these variables are related to the specific variables and parameters needed in this experiment
//And includes data from air temp & hum
//You can change this as well as you need must be the same as the table created in MySQL 

//Download data readme----------------------

These are the flows needed to download the data stored in the table DATA inside your database.

Remember that this will only download your information related to the current trial in course selected by user in dashboard.

This was done in that way to do not overload the Raspberry Pi with downloading very heavy files

Note that are created two files to be downloaded.

One file is from DATA database and contains all information from sensors stored.
The second one is from SETTINGS database, where are stored all configuration that user set before.

Then is possible to download both files with different information but depending on the current trial name.

PLEASE MAKE SURE THAT THE TWO FILES (FROM DATA AND CONFIG) ARE NAMED CORRECLTY AND DIFFERENT.


