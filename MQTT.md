# MQTT

MQTT (Message Queuing Telemetry Transport) is a publish-subscribe based messaging protocol.  At the center is an MQTT
broker, a server that handles inbound topics/messages and broadcasts the messages to the subscribers of the topic.  You
might think of the MQTT architecture as a chat server - the channels are topics and the messages are what the users post
to the channel.  All the other users in the channel receive the messages posted by the other users.  Messages to a
different channel are not posted in the channel.

To "join a chat room,"" a client subscribes to the topic.  From that point on, the client will receive topic/message
whenever a new one is posted to the topic.  A client may subscribe to as many topics as the logic requires.  

The MQTT broker may retain topics/messages and some of the topics/messages will not be retained.  This is controlled by
the client software, which posts topic/message and whether the broker should retain the message.

Upon subscription to a topic, all of the retained messages are sent to the client for that topic.

## MQTT Topics in RoboDomo
RoboDomo microservices use a consistent formula for devising topics for status and control messages.  The derivation of
the topics are trivial to use in the client code.

Status messages topics will be of the form: ```service/device/qualitfier/[status]``` with message containing the actual status.  For
example, ```smartthings/Kitchen Light/switch/``` and message ```on```.

Command messages topics will be of the form ```service/device/set``` with message containing the new state.  For
example, ```smartthings/Kitchen Lights/switch/set``` and message ```on```.

The Mosca npm package should be installed globally on your Docker host:
``` 
$ npm install mosca -g
```

Then you can use the mqtt command to post messages to topics or to view status messages.  You can use the informaton in
the display to choose topics to which your client should subscribe and or publish.

Example of Denon microservice messages.  The first denon is the name of the microservice.  The denon-s910w is the HOST
NAME of the actual AVR.  The status string simply indicates the topic is for status.  The uppercase string after the
"status/" is the feature of the AVR the status is about.  The string in quotes is the message body.  So 
```denon/denon-s910w/status/MU "OFF"``` indicates MUTE is off.  To turn MUTE on, use "set" instead of "status" in that
topic and pass "ON" as the message to the set topic.

The client code might only want to subscribe to MV (master volume) and MU (mute) to control and monitor just those
things.  No need to subscribe to any more than is needed.
```
$ mqtt subscribe -t 'denon/#' -v 
denon/denon-s910w/status/CVC "50"
denon/denon-s910w/status/CVFL "50"
denon/denon-s910w/status/CVFR "50"
denon/denon-s910w/status/CVSBL "50"
denon/denon-s910w/status/CVSBR "50"
denon/denon-s910w/status/CVSL "50"
denon/denon-s910w/status/CVSR "50"
denon/denon-s910w/status/CVSW "50"
denon/denon-s910w/status/DC "AUTO"
denon/denon-s910w/status/MNMEN "OFF"
denon/denon-s910w/status/MS "DOLBY SURROUND"
denon/denon-s910w/status/MU "OFF"
denon/denon-s910w/status/MV "267"
```

The SmartThings MQTT bridge has its own scheme for topics.  Caveat programmer!

Note that dimmers and ceiling fans have two statuses: switch on/off and level 0-100%.  Some devices have a number of
topics - a multisensor might have motion, temperature, and luminocity.

```
$ mqtt subscribe -t 'smartthings/#' -v 
smartthings/Back Room Fan/level 14
smartthings/Back Room Fan/switch off
smartthings/Back Room Light/level 35
smartthings/Back Room Light/switch off
smartthings/Back Room Switch/switch off
smartthings/Bathroom Light/level 100
smartthings/Bathroom Light/switch off
smartthings/Bathroom Sensor/motion inactive
```

## Continue Reading

* [Quick Start](./QuickStart.md)
* [Hardware](./Hardware.md)
* [DNS and Routing](./Networking.md)
* [Docker](./Docker.md)
* [MQTT](./MQTT.md)
* [SmartThings MQTT Bridge](./MQTTBridge.md)
* [Speaker / Text to Speech](./RoboSpeak.md)
* [Developing](./Developing.md)

## See also:
* [README](./README.md)
* Mosca - https://www.mosca.com/en-en/
* MQTT - http://mqtt.org/
