# Maxon Motor Setup

Maxon Motor Current Consumption @ 24V

0RPM = 30mA

25RPM = 32mA

87.5RPM = 32mA

287RPM = 36mA

1262RPM = 55mA

2237RPM = 80mA

3215RPM = 110mA (with 124mA surge when accelerating)

5650RPM = 194mA (with 230mA surge when accelerating)

5900RPM = 202mA (with 238mA surge when accelerating)

6150RPM = 210mA

10,000RPM = 230mA (with 260mA surge when accelerating)

Encoder Current Consumption @ 5V

0RPM 8mA

1270RPM 7mA

5900RPM 6mA

10000RPM 3mA

# Software Setup

## Download Firmware

Go to https://www.maxonmotorusa.com/maxon/view/product/control/4-Q-Servokontroller/466023

Click 'downloads'

click 'software/firmware'

download and install both of these:

•	ESCON Setup - Studio 2.2 Rev 4/Firmware 0150h/0151h (ZIP 584 MB)Version 8.1

•	ESCON Module 24/2 Firmware (ZIP 322 KB) 

The hardware manual is located in the manuals tab.

# Mechanical Setup

Make sure you mount the motor encoder fin in a vice. The fin is the green pcb that sticks out. from the motor. The motor spins very fast!

Connect controller to computer via usb and open up ESCON Studio

The Startup Wizard should pop up

Use this wizard for your firmware setup

Scanning for controller, should be found

Enter in motor characteristics (PN: 251601)

The values for motor and parameters should be set already on this controller since I worked on this controller.

Make sure these are selected:

"Digital Incremental Encoder 1024 positions"

"Speed Controller Closed Loop"

"Enable CW and CCW"

"Active High for both I/O 2 and 3"

Now PWM set value you choose. 10% pwm is 0rpm. 90% pwm is any value up to 10,000rpm.

The motor controller requires at least 10% pwm, so if you do analogWrite(PWM_PIN,<anything less than 26>); the controller will go into fault and do nothing until you feed it at least 26. It also cannot be fed more than than 230.

10% ~= 26/256

90% ~= 230/256

You can use analog output to determine actual speed averaged or actual speed (instant speed). 0V corresponds to -(maxrpm) and 3.3V corresponds to +(maxrpm) where negative means wheel spins in opposite direction that positive.

This means 1.65V is 0rpm.

Note that Teensy 3.2 only does reads up to 3.3V. Teensy 3.2 or Arduino cannot read negative voltages, so that's why the analog output voltage for speed read range is restricted. Otherwise, we would be able to do -3.3V for -(maxrpm) and +3.3V for +(maxrpm) resulting in more steps.

Before clicking finish, click wiring overview to see how to wire up the controller. I attached my wiring overview for reference.

Wire up the motor, encoder, and motor controller following the wiring overview and pin out in the motor datasheet (PN: 251601) and Encoder datasheet (PN: 462004).

Click finish, and your settings are loaded to the controller.

You can leave the controller connected to your computer so that the motor data is displayed on the ESCON Studio, but you are free to disconnect the USB if you want. You just won't have the nice display of rpm in the GUI.

If you have all wiring done, you can click auto-tuning.

If auto-tuning fails, make sure you put in valid parameters in the startup wizard.

If that still failes, I have included my parameter file which you can load to the controller by clicking the orange arrow in ESCON Studio "download parameters". This should work. You still have the ability to configure max speed, pwm control, etc.

Power +Vcc with 24V. Make sure hall sensor and encoder gets +5V. +5V for hall sensor comes from the controller. I fed the encoder +5V from the arduino since the breadboard rail from controller +5V only leaves enough room for one wire. You can figure a way to share the controllers 5V though, like soldering three wires together in a Y shape.
Make sure Arduino ground is connected to the ground of the controller, or digital inputs may not work.
