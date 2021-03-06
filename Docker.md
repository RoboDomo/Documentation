# Docker/Microservice Architecture

Docker is a software system that provides a form of virtualization.  Unlike virtual machines (VirtualBox, et al), Docker
containers are lightweight and even a low powered server (like we use with RoboDomo) can trivially run multiple
containers.  

One of the main advantages of Docker is that you don't have to install software on the host.  For example, one container
might use NodeJS version 9 while another might use NodeJS version 11.  The separation of concerns among the containers
makes it so there is no conflict in the versions.  Each container uses the version it is supposed to, and the host
running Docker doesn't need NodeJS installed at all.

RoboDomo leverages Docker to implement microservices.  According to WikiPedia, "Microservices are a software development
technique- a variant of the service-oriented architecture (SOA) architectural style that structures an application as a
colleciton of loosely coupled services.  In a microservices architecture, services are fine-grained and the protocols
are lightweight."

RoboDomo implements microservices to directly control devices and to provide infrastructure (e.g. MongoDB, MQTT
broker...).  The code and strategy to control a Sony TV is implemented in a Sony TV microservice.  The code and strategy 
to directly control a TiVo DVR or set top box is similarly implemented as a microservice.  And so on.

The one thing that is consistent and roughly orthagonal about RoboDomo's microservices is that control and status is
provided by MQTT topics/messages.  The client software or other microservices do not have to know about the internals of
how to control a Sony TV - it only needs to know the MQTT topics and messages required to control the TV and monitor the 
TV status.

You will choose from some or all of the RoboDomo microservices to run on your RoboDomo server.  If you don't have a
swimming pool (or Autelis controller for it), you don't need to run the autelis microservice.

If you have some device that is not currently supported, you might want to implement a microservice for it (please share
with us!).

The [microservice-core](https://github.com/RoboDomo/microservice-core) provides a small library of server-side
components/classes that make implementing RoboDomo style microservices with a consistent API.

A typical microservice is on the order of 100 lines of JavaScript.  While the code is short and sweet, the trick to
implementing a good microservice is in the error handling.  While you are developing, you will see good results from
whatever device or service you are supporting.  When the power goes out or the service you are polling is having issues,
your microservice is going to get in states that you may not have anticipated.  Ideally, your microservice will run for
very long periods - weeks or months between restarts.

## RoboDomo Microservices
A list of RoboDomo microservices with short descriptions can be found on the main [README](README.md) page.

## Continue Reading

* [Quick Start](./QuickStart.md)
* [Hardware](./Hardware.md)
* [DNS and Routing](./Networking.md)
* [Docker](./Docker.md)
* [MQTT](./MQTT.md)
* [SmartThings MQTT Bridge](./MQTTBridge.md)
* [Speaker / Text to Speech](./RoboSpeak.md)
* [Developing](./Developing.md)

## See also

* [Docker](https://www.docker.com)

