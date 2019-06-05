# SmartThings MQTT Bridge

A user on GitHub, St. John Johnson, created a Smartthings to MQTT bridge.  This software communicates with Samsung's
SmartThings cloud and provides status messages for your Things connected to your SmartThings Hub.  Also, you will get
some status for some things that work with SmartThings (that you have configured in the SmartThings app).  You can also
send messages to the bridge to control your devices.

The bridge software was developed for Home Assistant, but works perfectly well for RoboDomo.
* https://www.home-assistant.io/
* https://github.com/stjohnjohnson/smartthings-mqtt-bridge

The README in the GitHub repo for the smartthings-mqtt-bridge describes the installation and configuration for the
software.  Fortunately, the developer also provides a ready made Docker image, which is ideal for use with RoboDomo.

You will need to add some Groovy code to the SmartThings IDE WWW site (free registration required).  The instructions for
this is in the README as well.  Once this is set up, you will not need to mess with the SmartThings IDE.

_Note: when there is a power failure, you may have to reset the SmartThings hub._  The hub can get into a state where it
refuses connections.  See https://github.com/stjohnjohnson/smartthings-mqtt-bridge/issues/129.

An example of the yaml configuration file for the bridge:

```
---
mqtt:
    # Specify your MQTT Broker's hostname or IP address here
    host: robodomo
    # Preface for the topics $PREFACE/$DEVICE_NAME/$PROPERTY
    preface: smartthings

    # The write and read suffixes need to be different to be able to differentiate when state comes from SmartThings or when its coming from the physical device/application

    # Suffix for the topics that receive state from SmartThings $PREFACE/$DEVICE_NAME/$PROPERTY/$STATE_READ_SUFFIX
    # Your physical device or application should subscribe to this topic to get updated status from SmartThings
    # state_read_suffix: state

    # Suffix for the topics to send state back to SmartThings $PREFACE/$DEVICE_NAME/$PROPERTY/$STATE_WRITE_SUFFIX
    # your physical device or application should write to this topic to update the state of SmartThings devices that support setStatus
    state_write_suffix: set

    # Suffix for the command topics $PREFACE/$DEVICE_NAME/$PROPERTY/$COMMAND_SUFFIX
    # command_suffix: cmd

    # Other optional settings from https://www.npmjs.com/package/mqtt#mqttclientstreambuilder-options
    # username: AzureDiamond
    # password: hunter2

# Port number to listen on
port: 8080
```

A sh script to start the MQTT bridge software:
```
#!/bin/bash
SERVICE=mqtt-bridge

echo "stopping $SERVICE"
docker stop $SERVICE

echo "removing old $SERVICE"
docker rm $SERVICE

echo "pulling $SERVICE"
docker pull stjohnjohnson/smartthings-mqtt-bridge

echo "starting new $SERVICE"
docker run \
    -d \
    --restart always \
    -v /opt/mqtt-bridge:/config \
    -p 8080:8080 \
    --name $SERVICE \
    stjohnjohnson/smartthings-mqtt-bridge
```

## Continue Reading

* [Hardware](./Hardware.md)
* [DNS and Routing](./Networking.md)
* [Docker](./Docker.md)
* [MQTT](./MQTT.md)
* [SmartThings MQTT Bridge](./MQTTBridge.md)
* [Speaker / Text to Speech](./RoboSpeak.md)
* [Developing](./Developing.md)

