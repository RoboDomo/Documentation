# RoboDomo Hardware

RoboDomo is effectively a "hub of hubs," so it needs a centralized server that unifies these 3rd party hubs together.

## RoboDomo Server
Rather than using Raspberry PI, we choose a low cost PC.  This device is perfectly suited for RoboDomo's purposes:
* https://www.amazon.com/gp/product/B00VBNSO8U/

It is a barebones system that has an Intel Celeron J1900 processor on the motherboard.  You only need to add RAM and a
disk of some kind.

We recommned 4G of RAM and a 2.5" SSD large enough to hold and boot the operating system and sufficient space to
download the Docker container images and the log files for those and the system.

As of the writing of this document, the cost is:
* $118 for the PC
* $18 for the 4G of RAM
* $20 for the SSD
Total cost is $156.

This server is tiny and has enough power to run a Linux (distro of your choosing) and Docker plus all the Containers
RoboDomo offers currently, as well as MongoDB and Mosca MQTT broker, and the WWW server that serves the Client app.  

The SSD is a good idea because you want the server to boot or reboot as fast as possible.  Down time means RoboDomo is
off the air.

The server has gigabit ethernet and WiFi, but we recommend using the ethernet connection for reliability.

* CPU - https://www.amazon.com/gp/product/B00VBNSO8U/
* RAM - https://www.amazon.com/Timetec-1600MHz-PC3L-12800-Unbuffered-Computer/dp/B00IV19IJ4/
* SSD - https://www.amazon.com/Kingston-120GB-Solid-SA400S37-120G/dp/B01N6JQS8C/

## Z Wave Devices
Z Wave is a radio communication protocol used by certain IoT devices.  It is a robust protocol and is widely supported.
It forms a mesh network so devices far from the central hub only need to be close to another device that is close to
another device that is connected to the hub.

There are other protocols, like Zigbee, which can also be used by RoboDomo, but we prefer sticking to Z Wave devices as
much as possible.

The Z Wave devices include wall switches, dimmers, ceiling fan controllers, motion sensors, door/window sensors,
buttons, automatic window blinds, and so on.  

There are Z Wave door locks and garage door openers, but there may be security concerns if you decide to use them.

Typically, the Z Wave devices take a few minutes to install.  They do require batteries and those need to be replaced
every few months.  Be sure to have spares of the right kinds.

* Switch - https://www.amazon.com/GE-Lighting-Required-SmartThings-14294/dp/B01MUCZA1C/
* Dimmer - https://www.amazon.com/GE-Lighting-Required-SmartThings-14294/dp/B07361Y54Z/
* Ceiling fan - https://www.amazon.com/GE-Controls-Required-SmartThings-14287/dp/B06XTKQTTV/
* Motion sensor - https://www.amazon.com/Z-wave-Detector-install-Immunity-PIRZWAVE2-5-ECO/dp/B01MQXXG0I/
* Door/window sensor - https://www.amazon.com/Z-Wave-Magnets-Window-Sensor-DWZWAVE2-5-ECO/dp/B01N5HB4U5/

## SmartThings Hub
There are a few manufacturers of Z Wave compatibly hubs.  We choose the SmartThings hub becuase it is inexpensive and
well supported.  The mobile app is easy to use.  There is a decent sized community of developers who are making 3rd
party software for it.

Each Z Wave device must be paired with the hub and given a unique name.  Once a device is paired, you can use the 
SmartThings app or RoboDomo to control it.  For the most part, the devices will remain paired indefinitely, though
sometimes one might unpair and you have to add it back.  

There is a mechanism for unpairing devices, removing them from the mesh and hub.

Samsung has partnered with many companies to provide support for non Z Wave devices.  For example, you can link your
Nest (developer) account to SmartThings and you can monitor and control your Nest Thermostat from the SmartThins app.
NOTE: Monitoring and control is somewhat limited, so RoboDomo implements a Nest microservice that communicates directly
with Nest's cloud.

## Autelis Pool Controller
Autelis makes pool controls that connect to your existing pool controller.  The Autelis controller works in tandem with
the existing controls, so the control panel for your pool will continue to work.  The Autelis controller connects to
your network via ethernet and provides a decent API that will allow software to control the various devices connected to
your pool.  For example, there is API to turn on and off pumps, set temperature targets, turn on and off the heater(s),
and monitor various statuses (like pool/spa temperatures, state of pumps).

## Direct Controls
Your home/office likely has a number of devices already on your LAN that could be controlled by RoboDomo directly.  For
example, the LGTV microservice communicates with one or more LG TVs via diretly connecting to the TV over the LAN (or
WiFi).  

## Tablet/Phone
The client app runs in a browser, on a tablet, or on a phone.  The UI adjusts accordingly.  The RoboDomo client is
designed to run on tablets that you'd mount on the wall.  An electrician would run power to where you want your tablet
to be mounted.  You power the tablet using the charger that came with the tablet.  There are a few wall mount solutions
for tablets, such as this one: https://www.amazon.com/Tablet-Dockem-Samsung-Brackets-version/dp/B00JV75FC6/.

You might also want to put a tablet in a stand on your night stand.

## Amazon Alexa/Google Home
You can control your home using Amazon Alexa or Google Home.  By adding the SmartThings hub alone, you can ask the
Echo/Home to turn on or off your Z Wave devices.  

## Speak Server
You can configure a Raspberry PI to act as a speech server so RoboDomo can talk to you, to announce various events.  The
PI simply has a USB powered speaker plugged into it and runs the RoboDomo Speak microservice.  

## See also
* [README](./README.md) 
