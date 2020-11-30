# Microservice Architecture

RoboDomo uses a Docker-based Microservice Architecture strategy.  Rather than one big monolithic back end that knows how to interact with all the things and cloud services you need to control your smart home, a simple microservice is implemented for each kind of thing or cloud service.

For example, the [denon-microservice](https://github.com/Robodomo/denon-microservice) is a simple program consisting of 167 lines (at the time of this writing) of JavaScript for Node JS.  This includes several lines of comments.  The focus of this program is simply controlling and monitoring Denon AVR (audio recweiver) devices.  It doesn't have to know about Ring doorbell API or how to turn on lights.  It only needs to know how to change the input on the AVR, change the volume, monitor the volume, and so on.

THe microservices are designed to run in individual Docker containers.  You can run any or all of the microservices on a single host.  A robust deployment of Robodomo using all the microservices can trivially run on a celeron micro computer with 4GB of RAM.

You typically start only the microservices that you need.  You don't need to start the [autelis-microservice](https://github.com/Robodomo/autelis-microservice) if you don't have a pool and/or spa, or don't have an Autelis controller for your pool and/or spa.

The microservices post status messages via [MQTT](MQTT.md), and process command messages via MQTT as well.  See the Robodomo MQTT documetation for more details.

The microservices architecture allows for separation of concerns betwen client/WWW user interfaces.  If you want to roll your own client, you only need to send and receive MQTT messages for the topics/devices your interface presents.


## microservice-core

Microservices have a lot of functionality in common.  Rather than duplicate/implement all this functionality in each microservice repository, we add the [microservice-core](https://github.com/Robodomo/microservice-core) Node JS module via npm install:

``` npm install --save https://github.com/Robodomo/microservice-core ```

A single microservice typically monitors one or more devicves.  For example, the [denon-microservice](https://github.com/Robodomo/denon-microservice) might need to monitor the AVR in your family room and another in your bedroom.  The microservice (index.js) implements a Host class that extends the HostBase class in the microservice-core module.  The denon-microservice instantiates a DenonHost class instance for each Denon AVR you configure (via ENV variables or via a configuration MQTT message).  Each instance focuses its logic on controlling a single AVR.  

The HostBase class provides automatic MQTT message formatting and sending.  HostBase  has a setter method for "state" that when values in state change in the instances internal state.  WHen a change happens, an MQTT message is sent.

The HostBase constructor takes three arguments: 

- host - the MQTT connect string
- topic - the base topic used to subscribe and publish MQTT
- custom (optional) - if true, the parent will be handling its own message sending/receiving.

