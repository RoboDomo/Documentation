# Microservice Architecture

RoboDomo uses a Docker-based Microservice Architecture strategy. Rather than one big monolithic back end that knows how to interact with all the things and cloud services you need to control your smart home, a simple microservice is implemented for each kind of thing or cloud service.

For example, the [denon-microservice](https://github.com/RoboDomo/denon-microservice) is a simple program consisting of 167 lines (at the time of this writing) of JavaScript for Node JS. This includes several lines of comments. The focus of this program is simply controlling and monitoring Denon AVR (audio recweiver) devices. It doesn't have to know about Ring doorbell API or how to turn on lights. It only needs to know how to change the input on the AVR, change the volume, monitor the volume, and so on.

THe microservices are designed to run in individual Docker containers. You can run any or all of the microservices on a single host. A robust deployment of RoboDomo using all the microservices can trivially run on a celeron micro computer with 4GB of RAM.

You typically start only the microservices that you need. You don't need to start the [autelis-microservice](https://github.com/RoboDomo/autelis-microservice) if you don't have a pool and/or spa, or don't have an Autelis controller for your pool and/or spa.

The microservices post status messages via [MQTT](MQTT.md), and process command messages via MQTT as well. See the RoboDomo MQTT documetation for more details.

The microservices architecture allows for separation of concerns betwen client/WWW user interfaces. If you want to roll your own client, you only need to send and receive MQTT messages for the topics/devices your interface presents.

## microservice-core

Microservices have a lot of functionality in common. Rather than duplicate/implement all this functionality in each microservice repository, we add the [microservice-core](https://github.com/RoboDomo/microservice-core) Node JS module via npm install:

`npm install --save https://github.com/RoboDomo/microservice-core`

A single microservice typically monitors one or more devicves. For example, the [denon-microservice](https://github.com/RoboDomo/denon-microservice) might need to monitor the AVR in your family room and another in your bedroom. The microservice (index.js) implements a Host class that extends the HostBase class in the microservice-core module. The denon-microservice instantiates a DenonHost class instance for each Denon AVR you configure (via ENV variables or via a configuration MQTT message). Each instance focuses its logic on controlling a single AVR.

### StatefulEmitter class
StatefulEmitter is a deluxe EventEmitter (it extends EventEmitter).  While providing all the functionality of EventEmitter, StatefulEmitter provides a concept of "state".  The state is a "private" member variable (one per instance).  The state(value) setter method combines the passed Object (value) with the internal state and fires a "statechange" event.

The state() getter just returns the overall state (all the combined state) as an Object.

StatefulEmitter provides another method that is quite useful: async wait(time).  This method allows a microservice to implement its polling in a loop and call this wait method to assure polling is done at a desired rate.


### HostBase class

The HostBase class provides automatic MQTT message formatting and sending. 

HostBase extends StatefulEmitter which has a setter method for "state" that when values in state change in the instances internal state. HostBase listens for the statechange evnt and when a change happens, an MQTT message is sent.

The HostBase constructor takes three arguments:

- host - the MQTT connect string
- topic - the base topic used to subscribe and publish MQTT
- custom (optional) - if true, the parent will be handling its own message sending/receiving.

An example - the topic will be unique for each instance of the DenonHost. When the DenonHost instance detects a volume change, it will post a topic/message for the volume change that is easily identifiable as to which AVR is involved.

HostBase provides some handy methods:

- publish(key, value) - sends value (converted to JSON) to the HostBase's unique topic plus key.
- alert(title, ...message) - sends an alert MQTT message with type "alert" with the given message.
- warn(title, ...message) - sends an alert MQTT message with type "warn" with the given message.
- exception(e) - sends an exception alert MQTT message.
- abort(...message) - prints message to stdout (docker log) and exits.
- exit(...message) - prints message to stdout and exits.

- getSetting(setting) - gets a record the MongoDB database with the key "setting".
- putSetting(setting, value) - sets a record in the database using the key setting.

- config() - returns the global config.json Object, which is stored in the MongoDB database.

# See Also

- [Configuration](Configuration.md) - how to configure your microservices and client.
- [MQTT](MQTT.md) - how MQTT is used in RoboDomo
- [microservice-core repository](https://github.com/RoboDomo/microservice-core)
