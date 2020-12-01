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

## Table of Contents

- [Features](Features.md)

- [Quick Start Guide](QuickStart.md)
- [Hardware](Hardware.md)
- [DNS and Routing](./Networking.md)
- [MQTT](MQTT.md)
- [Docker](./Docker.md)
- [Microservice Architecture](Microservices.md)
- [SmartThings MQTT Bridge](./MQTTBridge.md)
- [Developing](Developing.md)

### Alexa
### Google Home
### Siri / Homekit / Homebridge

You can control your lamp with your voice by adding an Amazon Echo of Google Home.

## Continue Reading

- [Quick Start](./QuickStart.md)
- [Hardware](./Hardware.md)
- [DNS and Routing](./Networking.md)
- [Docker](./Docker.md)
- [MQTT](./MQTT.md)
- [SmartThings MQTT Bridge](./MQTTBridge.md)
- [Speaker / Text to Speech](./RoboSpeak.md)
- [Developing](./Developing.md)
