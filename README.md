# RoboDomo

RoboDomo is a home automation application that links multiple cloud based services, and direct to hardware services,
into a unified interface. Without RoboDomo, or something like it, the end user would have to launch a variety of
applications on their phone/tablet/browser, to control a proprietary hub or a company's devices. For example, you would
run the Nest app to control your thermostat, the Ring app to interact with your doorbell, the (Samsung) SmartThings app
to control your light switches and ceiling fans, a WWW browser to control your pool and spa pumps and heaters, and so
on.

The RoboDomo app is all you need - it provides the user interface needed to control all the things. It is effectively
the missing "Hub of Hubs" that the IoT space is sorely missing.

Other than a few 3rd party modules, RoboDomo is written entirely in JavaScript for NodeJS on the server side and React
on the client side.

The backend for RoboDomo is a growing number of available microservices that run as Docker containers. You run only the
microservices that are appropriate to your home or office. That is, if you have an LG TV, you'd run the LGTV
microservice, and if you don't have an LG TV, you would have no need to run it.

RoboDomo supports both Hubitat (preferred) and SmartThings hubs.  Both hubs can connect to thousands of kinds of devices, both Z Wave and Zigbee. 

## Table of Contents

1. [Quick Start Guide](QuickStart.md)
2. [Features](Features.md)
3. [Hardware](Hardware.md)
4. [DNS and Routing](./Networking.md)
5. [MQTT](MQTT.md)
6. [Docker](./Docker.md)
7. [Microservice Architecture](Microservices.md)
8. [SmartThings MQTT Bridge](./MQTTBridge.md)
9. [Developing](Developing.md)

## Microservices

The [docker-scripts](https://github.com/RoboDomo/docker-scripts) repository contains scripts that automatically download or update Docker versions of the microservices and start them on your host.

The [microservice-core](https://github.com/RoboDomo/microservice-core) repository contains shared code for crafting a microservice.  It is shared among virtually all, if not all, of the RoboDomo microservices.

#### Multimedia

- [denon-microservice](https://github.com/RoboDomo/denon-microservice) - 
  Microservice to monitor and control Denon AVR (audio receivers).
- [appletv-microservice](https://github.com/RoboDomo/appletv-microservice) - 
  Microservice to monitor and control Apple TV devices.
- [bravia-microservice](https://github.com/RoboDomo/bravia-microservice) - 
  Microservice to monitor and control Sony Bravia TVs.
- [roku-microservice](https://github.com/RoboDomo/roku-microservice) - 
  Microservice to monitor and control Roku streaming devices.
- [tivo-microservice](https://github.com/RoboDomo/tivo-microservice) - 
  Microservice to monitor and control TiVo DVRs and set top boxes.
- [lgtv-microservice](https://github.com/RoboDomo/lgtv-microservice) - 
  Microservice to monitor and control LG TVs.
- [tvguide-microservice](https://github.com/RoboDomo/tvguide-microservice)
  Microservice that polls the SchedulesDirect TV Guide API for channels/logos/etc. information for your cable provider.
- [harmony-microservice](https://github.com/RoboDomo/harmony-microservice) - 
  Microservice that monitors and controls Logitech Harmony universal remote control hubs.

#### Things

- [myq-microservice](https://github.com/RoboDomo/myq-microservice) - 
  Microservice to monitor and control Chamberlain MyQ garage door openers.
- [autelis-microservice](https://github.com/RoboDomo/autelis-microservice) - 
  MIcroservice to monitor and control pool/spa equipment via an Autelis pool controller.
- [hubitat-microservice](https://github.com/RoboDomo/hubitat-microservice) - 
  Microservice to monitor and control things you connect to a Hubitat Elevation hub.  Z Wave and Zigbee devices.
- [icomfort-microservice](https://github.com/RoboDomo/icomfort-microservice) - 
  Microservice to monitor and control Lennox iComfort S30 (and compatible) HVAC systems.
- [nest-microservice](https://github.com/RoboDomo/nest-microservice)
  Microservice that uses the retired "Works with Nest" API to monitor and control Nest thermostats and Protect devices.

#### System

- [config-microservice](https://github.com/RoboDomo/config-microservice) - 
  Microservice to monitor changes to Config.js and Macros.js and broadcast MQTT message when changed.

- [presence-microservice](https://github.com/RoboDomo/presence-miicroservice) - 
  Ping phones and watches and other devices to determine if a person is present or not.

- [triggers-microservice](https://github.com/RoboDomo/triggers-microservice) - 
  Watch what's going on and process rules when things happen.  The events you wait for and routines to run when events happen are limited to what. you can code in JavaScript (e.g. no limits!).  Example: turn on garden lights at sunset and turn them off at sunrise.

- [here.com-microservice](https://github.com/RoboDomo/here.com-microservice) - 
  Microservice to monitor weather conditions for one or more location, using the here.com API.

- [macro-microservice](https://github.com/RoboDomo/macro-microservice) - 
  Microservice that executes macros on demand.  A macro is a procedure consisting of a sequence of MQTT topics/messages to send in order, and/or, execution of a sequence as in a function call. 

  



### Alexa

### Google Home

### Siri / Homekit / Homebridge

You can control your lamp with your voice by adding an Amazon Echo of Google Home.

## Continue Reading

1. [Quick Start](./QuickStart.md)
2. [Hardware](./Hardware.md)
3. [DNS and Routing](./Networking.md)
4. [Docker](./Docker.md)
5. [MQTT](./MQTT.md)
6. [SmartThings MQTT Bridge](./MQTTBridge.md)
7. [Speaker / Text to Speech](./RoboSpeak.md)
8. [Developing](./Developing.md)