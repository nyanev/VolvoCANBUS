# VolvoCANBUS
JAVA project to interact with a P2 Volvo S60 Canbus

An adaptation/interface onto can-utils (https://github.com/linux-can/can-utils)

The reason I started this version is because of the DPF (soot filter) and regeneration issues. The goal is to provide the driver with a simple feedback whenever the car is regenerating its DPF. The driver can then make the decision to keep on driving and let the process finish, or to cut the process short and run the risk of eventually getting messages such as:
"Emissions problem. Service required"
"DPF full. Regeneration required"

See also my other repository where I am basing this off of an Arduino board.

The changes in this version are specific to my Volvo S60 (MY2009), aka P2 facelift model.

You are welcome to use and expand on this software ON YOUR OWN RISK!

Connecting to the HS-CAN means you are possibly interfering with "under the bonnet" components (ECM, BCM, TCM) that might possibly affect the configuration and safety of your car.
IF YOU DO NOT KNOW WHAT YOU ARE DOING, DO NOT ATTEMPT TO MODIFY YOUR CAR!

SETUP:
------
* 1x Raspberry PI 3B, with a default install of Raspbian LITE
* 1x CANBUS PiCAN2  
* 1x OBD -> serial female connector (DB9F)  

SETTING UP THE HARDWARE:
-------------------------
Connect the PiCAN2 board to the Raspberry as per instructions.
Connect the OBD connector onto the serial connector on the PiCAN2 board.
Make sure that your car's ignition is at least in the 'II'-setting.


LIBRARIES:
----------
You will need the following libraries to succesfully run this project:
 1. JAVA, this requires an additional install on raspberry (apt install oracle-java8-jdk)
 2. cansend and candump utilities, please place them in the same folder as this jar

CONFIGURATION:
--------------
In your Raspberry, enable the PiCAN2 board by editing /boot/config.txt:

`dtparam=spi=on` <br/> 
`dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25`<br/> 
`dtoverlay=spi-bcm2835-overlay`<br/>

Use the following command to bring the can0 interface up:<br/>


`sudo /sbin/ip link set can0 up type can bitrate 500000`

Look up the proper communication speeds for your modelyear car. Mine, a P2 S60 of model year 2009, uses
* 125kbps for the low-speed CANBUS and
* 500kbps for the high-speed CANBUS.

Set the correct speed for this program, noting that it will default to the above values.
This HS-CAN is what you will need for the important stuff: DPF temperature monitoring.

Between different model years, this speed changes. Configuring an incorrect speed will definitely upset your
setup and could possible even DAMAGE YOUR CAR. Always check and double-check your vehicle's communication speeds
before moving on.


Verify you have a can0 (or something like that) interface present on your system. This is the main mode of
communication for the utils mentioned, so it should be up and running.

