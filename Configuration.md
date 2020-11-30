# Configuring RoboDomo

RoboDomo configuration is done via a single Config.js file and the [config-microservice](https://github.com/RoboDomo/config-microservice).

The config-microsevice monitors changes to a Config.js file and/or Macros.js in its ./config directory and exits on change. The Docker container running the microservice will restart the config-microservice which will cause the updated Config.js and/or Macros.js to be loaded.

On startup, the Config.js and Macros.js are generated with default values if those files do not exist. If the files do exist, they are loaded and stored in the MongoDB database. The config Object is also sent as JSON to the MQTT topic "settings/status". Client programs or other microservices can restart of update their configuration when they receive this message.

When you start the config-microservice, you will want to create a Docker volume that puts the ./config directory in a subdirectory on the host. This allows you to edit the Config.js and/or the Macros.js and have the microservice automatically restart, reload the configuration and macros files, etc.

There is a Config.sample.js file provided in the [config-microservice](https://github.com/RoboDomo/config-microservice) repository. You can copy this to Config.js and edit it.

Likewise, ther is a Macros.sample.js file provided to get you started with your own custom Macros.js.

# Why .js files and not .json or YAML?

We choose .js files because the creation/declaration of JavaScript Objects is free form, and you can add comments to your .js file and even comment out definitions in the .js file. JavaScript also provides the ability to define constants at the top of the file that can be used throughout the exported Config object member definitions. You can use the backtick string substition to dynamically set values in the Config object. You can use require(), define functions to call, etc.

JSON does not support any of the power of JavaScript.

YAML is not JavaScript. Why complicate things by using yet anotheer definition language to RoboDomo?

# Config.js

The Config.js file exports a single JavaScript Object (not JSON, not JSON stringified). The Object contains members that are used by the client software and the microservices to configure themselves.

## root member

These root members are useful:

- name: the name of the system ("RoboDomo Home Automation System")
- version: the version of teh software
- location: where this RoboDomo is deployed

## mqtt member

The mqtt member is a root member Object. It contains keys for host and port of the MQTT server, plus contants for the various MQTT topics.

### Example

```
mqtt: {
  host: "mqtt-broker",
  port: 80,

  // topics roughly one per microservice
  appletv: "appletv",
  denon: "denon",
  ...
},
```

## microservices member

The microservices member is a root member Object.

It contains a types key, which is an array of classifications (strings) that clients can use to organize the microservices for display.

It contains a services key, which contains an array of microservice information Objects. This array should contain only the microservices you intend to start/use.

### Example

```
microservices: {
  types: [ "General", "System", "Multimedia" ],
  services: [
  { title: "Sony Bravia TVs", mqtt: "bravia", description: "Sony Bravia TV controls", name: "brava-microservice", type: "Multimedia""},
  { title: "LG TVs", mqtt: "lgtv", description: "LG TV controls", name: "lgtv-microservice", type: "Multimedia""},
  { title: "Here Weather", mqtt: "here", description: "Montior weather conditions via here.com", name: "here.com-microservice", type: "General""},
  { title: "Configuration"", mqtt: "settings", description: "Monitor and update COnfiguration files", name: "config-microservice", type: "Stystem""},
  ...
  ],
},
```

## presence member

The presence member is a root member Array.

The presence array contains one Object for each person/device combination used to determine presence. It is used to configure the [presence-microservice](https://github.com/RoboDomo/presence-microservice).

### Example

```
presence: [
  { person: "me", device: "my-iphone" },
  { person: "spouse", device "spouse-iphone"},
],
```

## locks member

The locks member is a root member Array.

It contains an Object for each controllable (door/window/etc.) lock that can be controlled.

### Example

```
locks: [
  { device: "Front Door Lock", hub: "hubitat", name: "Front Door Lock", room: "Entry Way" },
  ...
],
```

## theaters member

The theaters member is a root member Array.

The Array contains an Object per "theater" in your home/office. A theater is a collection of devices that work together, such as a TV, AVR, and TiVo set top box. You might have a theater for your family room and another for your bedroom. There is no limit to the number of theaters.

RoboDomo is designed to operate as a universal remote. A theater has an array of devices and an array of activities.

An activity is something like "Watch TV" or "Watch Apple TV". The activies member of a theater Object contains an Object per activiy. The Object defines the name of the activity, the default device for that activity (e.g. watching TV activity might have the TiVo as the default device), the inputs for the TV and AVR to set up or detect the activity is active, and a macro name to run when the activity is started.

There should be an activity for "All Off" that turns off the devices in the theater.

### Example

```
theaters: [
  // The family room theater
  {
    title: "Family Room",
    key: "family-room",
    guide: <SchedulesDIrect tv guide code>,
    devices: [
      { name: "LG TV", title: "OLED TV", type: "lgtv", favorites: lgtvFavorites, device: "OLEDE8P" },
      { name: "AVR", title: "Denon AVR", type: denon", device: "denon-avr" },
      { name: "TiVo", title: "TiVo Bolt", type: "tivo", favorites: tivoFavorites, device: "tivo-bolt", guide: <SchedulesDirect tv guide code>"},
      { name: "Apple TV", title: "Apple TV 4K", type: "appletv", device: "family-room-appletv" },
	  ...
    ],
    activities: [
      { name: "Watch TV", defaultDevice: "TiVo", inputs: { tv: "hdmi1", avr: "TV"}, macro: "Famly Room Watch TV" },
      { name: "Watch Apple TV", defaultDevice: "Apple TV", inputs: { tv: "hdmi2", avr: "MPLAY"}, macro: "Famly Room Watch Apple TV" },
      ...
    ],
  },
  // Theater for the bed room
  {
    title: "Bed Room",
    key: "bed-room",
    guide: <SchedulesDIrect tv guide code>,
    devices: [
      ...
    ],
    activites: [
      ...
    ],
  },
  ...
],
```

# See Also:

## Documentation

- [Microservice Architecture](Microservice.md) - About Microservices.
- [Macros](Macros.js) - How RoboDomo Macros work.

## Repositories

- The [macros-microservice](https://github.com/RoboDomo/macros-microservice) repository.
- The [config-microservice](https://github.com/RoboDomo/config-microservice) repository.
- The [presence-microservice](https://github.com/RoboDomo/presence-microservice) repository.
