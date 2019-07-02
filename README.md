# RoboDomo

RoboDomo is a home automation application that links multiple cloud based services, and direct to hardware services,
into a unified interface.  Without RoboDomo, or something like it, the end user would have to launch a variety of
applications on their phone/tablet/browser, to control a proprietary hub or a company's devices.  For example, you would
run the Nest app to control your thermostat, the Ring app to interact with your doorbell, the (Samsung) SmartThings app
to control your light switches and ceiling fans, a WWW browser to control your pool and spa pumps and heaters, and so
on.  

The RoboDomo app is all you need - it provides the user interface needed to control all the things.  It is effectively
the missing "Hub of Hubs" that the IoT space is sorely missing.

Other than a few 3rd party modules, RoboDomo is written entirely in JavaScript for NodeJS on the server side and React
on the client side.  

The backend for RoboDomo is a growing number of available microservices that run as Docker containers.  You run only the
microservices that are appropriate to your home or office.  That is, if you have an LG TV, you'd run the LGTV
microservice, and if you don't have an LG TV, you would have no need to run it.

## RoboDomo puts the "Smart" in Smart Home
RoboDomo features a Macros microservice and a Triggers microservice.  These are two powerful concepts that enable the
implementation of scripts and rules; your smart home becomes more than just several Things that you can remotely control

The Macros microservice listens for the name of a macro and publishes an arbitrarily long list of MQTT topics/messages.
A handy macro might be to turn off all the devices in your home (e.g. at bedtime) except for the TV in your bedroom, and
sets the thermostat to 72.  The "Good Night" macro described controls multiple devices across multiple providers (e.g.
SmartThings to turn off lights, Logitech to control the TVs, Nest to set the thermostat temperature).  A corresponding
Web UI component might be a pushbutton that simply sends the macro topic/message.  Thus you can have a "Good Night"
button in your UI.

You are only limited by your imagination as to what you can do with Macros.  Another example are "TV Break" and
corresponding "TV Resume"  macros.  TV Break turns on the light and pauses the TV, so you can go get a drink from the
fridge or have a bathroom break.  The TV Resume macro does the reverse - turns off the light and resumes the TV playing.

Triggers are intelligent rules, based upon events.  There is an MQTT command line program that you can send any topic
and message.  So if you want to turn on a specific light at 3PM and turn it off at 5PM, you can set up a simple cron
job.  However, you really want to monitor the state of the light switch.  When you turn it on at 3PM, did it really turn
on?  If not, you want to re-send the MQTT message periodically until you get a status message that the light is on.

Triggers might subscribe to a handful of topics and act upon the state of more than one device.  Examples of Triggers:

1) At sunset, turn on the garden lights, and at sunrise, turn them off.  The time for sunset and sunrise are provided by
the Weather microservice.  

2) If the soil moisture sensor indicates the ground is wet, do not turn on the sprinklers today.

3) If the prediected high temperature is 100 degrees or more, run the pool filter pump for 10 hours, otherwise run it
for 8 hours.

4) When the spa is turned on, monitor the temperature until it reaches 90 degrees F and then announce the spa is warm
enough to comfortably enter and turn on the jets.  When the temperature hits the target temperature, announce that and 
stop monitoring.  If the user turns off the spa, stop monitoring.

5) If the temperature in the home office reaches 80 degrees, turn on the ceiling fan.  If the temperature remains 80 or
increases, bump up the fan speed.

6) If the motion sensor in the bathroom is triggered, turn on the bathroom lights.  But if it's after 10PM and before
6AM, turn on the light but set the brighness to 10%.

7) When the doorbell senses motion, turn on the porch light.  When the doorbell is pressed, pause the TV and turn on 
the entryway lights, (but only when it's not between sunset and sunrise!).

8) When a button on the wall opposite the light switches is pressed, turn on the lights.  If the lights are already on,
turn them off.

9) When a button on the wall by the door to the backyard/pool is pressed, turn on the spa, the spa heater, and the
outside light (if it's dark outside).  

## Getting Started

Before you start doing contruction (e.g. replacing light switches in the walls), you can get a feel for how home
automation and remote controls with a small investment in hardware.

This is a fairly cheap barebones PC that works perfectly fine as the hub of hubs for RoboDomo:
* https://www.amazon.com/gp/product/B00KR0QHXW/
You will need a cheap SSD and 4G of RAM.  This is a much better alternative than trying to make the backend run on a
Raspberry PI.  This PC runs any Linux distro, kept up to date as distros typically are.

To control switches and dimmers and fans and the like, you will need a SmartThings hub.  
* https://www.amazon.com/Samsung-SmartThings-Smart-Home-Hub/dp/B010NZV0GE
The more you work with IoT, the more obvious it becomes that the hardware manufacturers want to sell you a hub as well
as the Things the hub controls.  This SmartThings hub works with Z Wave devices (Z Wave is a radio protocol), Zigbee
(another radio protocol), bluetooth, and some Things that are on your WiFi or LAN via direct IP control.

You can plug one of these smart plugs into the wall and a lamp into the plug and conrol it via SmartThings.  Set up for
all this is just a few minutes.
https://www.amazon.com/GE-Appliances-Required-SmartThings-28169/dp/B004AMB3CI/

You can control your lamp with your voice by adding an Amazon Echo of Google Home.

## Continue Reading

* [Quick Start](./QuickStart.md)
* [Hardware](./Hardware.md)
* [DNS and Routing](./Networking.md)
* [Docker](./Docker.md)
* [MQTT](./MQTT.md)
* [SmartThings MQTT Bridge](./MQTTBridge.md)
* [Speaker / Text to Speech](./RoboSpeak.md)
* [Developing](./Developing.md)

