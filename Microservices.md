# Microservice Architecture

RoboDomo uses a Docker-based Microservice Architecture strategy.  Rather than one big monolithic back end that knows how to interact with all the things and cloud services you need to control your smart home, a simple microservice is implemented for each kind of thing or cloud service.

For example, the [denon-microservice](https://github.com/Robodomo/denon-microservice) is a simple program consisting of 167 lines (at the time of this writing) of JavaScript for Node JS.  This includes several lines of comments.  The focus of this program is simply controlling and monitoring Denon AVR (audio recweiver) devices.  It doesn't have to know about Ring doorbell API or how to turn on lights.  It only needs to know how to change the input on the AVR, change the volume, monitor the volume, and so on.

THe microservices are designed to run in individual Docker containers.  You can run any or all of the microservices on a single host.  A robust deployment of Robodomo using all the microservices can trivially run on a celeron micro computer with 4GB of RAM.

You typically start only the microservices that you need.  You don't need to start the [autelis-microservice](https://github.com/Robodomo/autelis-microservice) if you don't have a pool and/or spa, or don't have an Autelis controller for your pool and/or spa.

## microservice-core

Microservices have a lot of functionality in common.  Rather than duplicate/implement all this functionality in each microservice repository, 
