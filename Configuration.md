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

## root members

A root member is a key in the configuration object. The member might be another Object or an Array or a single value.

Not all root members are required, but if you run the [autelis-microservice](https://github.com/RoboDomo/autelis-microservice), you will need to include the autlis member/configuration.

You may add and members not defined here for your own purposes.  They will be ignored.

The examples provided here are for reference purposes.  They will likely work, but full documentation for the configuration of each microservice (and the client) is explained in depth in the documenation for the microservices and client.

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

## dashboards member

The dashboards member is a root member array.

The array contains a list of [Dashboard](Dashboards.md) Arrays, each one defining items for a tile based user interface. This interface can present multiple dashboards in the user interface - one for the Family Room, one for the Bed Room, and so on. You probably want to control the ceiling fan in the Family Room from the Family Room dashboard, but not the Bed Room dashboard.

The array of tiles specifies the specific tiles to be presented in the user interface. Each tile type Object might contain members specific to that type of tile. For example, the Clock tile needs no additional members, but the Weather tile needs a location member to determine what weather information to display (you might have weather for your home and another for your office...).

### Example

```
dashboards: [
  {
    title: "Theater",
    key: "theater",
    tiles: [
      { type: "clock" },
      { tyype: "theater", title: "Theater" },
      { type: "lock", hub: "hubitat", device: "Front Door Lock", title: ""Front Door Lock" },
      { type: "fan", hub: "hubitat", device: "Family Room Ceiling Fan" },
      { type: "switch", hub: "hubitat", device: "Family Room Ceiling Fan Switch" },
      { type: "macro",  name: "Bed Time", label "Bed Time" },
      ...
    ],
  },
  {
    title: "Bed Room",
    key: "bedroom",
    tiles: [
      ...,
      { type: "macro", name: "Good Night", label: "Good Night" },
      ...
    ],
  },
  ...
],
```

## waather member

The weather root member is an Object that contains information about the locations you want to monitor weather for.

### Example

```
weather: {
  // monitor weather in LA and SD
  locations: [
    { name: "Los Angeles, CA", device: "92010", default: true },
    { name: "San Diego, CA", device: "92109" }
  ]
},
```

## rgb member

The rbg root member is an array of RGB controllable devices (lights, light strips, etc.).

### Example

```
rgb: [
  { name: "Kitchen Cabinets Controller", label: "Kitchen Cabinet Lights", hub: "hubitat", type: "rgb" },
  ...
],
```

## autelis member

The autelis root member is an Object that defines the location and credentials of the Autelis API server, the weather location, and a deviceMap with forward and backward hash maps (Objects).  The configuration of your pool and autelis controller is unique to how your installer set up your pool when it was constructed.  The Autelis controller reports devices like aux1, which might be the spa jets in one home but pool cleaner in another.  The forward and reverse maps can be used to associate the aux names to friendly names (e.g. aux1 -> jets).

This member is used to configure the [autelis-microservice](https://github.com/RoboDomo/autelis-microservice).

### Example

```
autelis: {
  device: "autelis",
  name: "Pool Control",
  url: "http://poolcontrol",
  location: "92010",
  credentials: {
    username: "admin",
    password: "admin",
  },
  deviceMap: {
    forward: {
      pump: "pump",
      jets: "aux1", 
      ...
    },
    backward: {
      pump: "pump",
      aux1: "jets",
      ...
    },
  },
},
```

## nest member

The nest root member is an Object that defines Nest thermostats and Protect devices in your home/office.

*Warning:* The "Works with Nest" API was [retired by Google](https://developers.nest.com/).  The nest-microservice may or may not work for you.


### Example

```
nest: {
  thermostats: [
    { device: "main1", name: "Thermostat 1", }
    { device: "main2", name: "Thermostat 2", }
  ],
  protects: [
    { device: "Foyer Nest Protect", name: "Foyer", }
    { device: "Bedroom Nest Protect", name: "Bedroom", }
  ],
}
```

## icomfort member

This root member is an Object that defines the iComfort zones for your A/C and heating system.  Tha iComfort S30 is the device supported.

*Note:* You may not find S30 support in many other Home Automation systems!

### Example

```
icomfort: {
  thermostats: [
    { zone: "0", title: "Family Roomt" },
    { zone: "1", title: "Den" },
    { zone: "2", title: "Guest Bedroom" },
    { zone: "3", title: "Master Bedroom" },
  ],
},
```

## sensors member


This root member is an Array of sensor Object definitions.

These values are for server-side or client-side use.

### Example

```
sensors: [
  { name: "Bathroom Sensor", hub: "hubitat", type: "battery" },
  { name: "Bathroom Sensor", hub: "hubitat", type: "motion" },
  { name: "Sliding Door", hub: "hubitat", type: "contact" },
  { name: "Bedroom Window", hub: "hubitat", type: "contact" },
  ...
],
```

## smartthings member

This root member is an Object that defines SmartThings and Hubitat devices available to RoboDomo.

The name "smartthings" is from when RoboDomo only supported SmartThings hubs. RoboDomo now supports Hubitat, as well, so to differentiate which hub a thing is connected to, the Object definitions contain a hub member.  

The hub member is the hostname (or IP) of the hub to use for the thing.  You can have multiple hubs of each kind configured this way.

The rooms member of a thing definition defines which rooms the device is associated with.  A presence thing might be in all the rooms.

*Note* RoboDomo supports using both SmartThings and Hubitat hubs at the same time.  Some devices connected to each.

### Example

```
smartthings: {
  device: "smartthings", // default device
  name: "SmartThings Hub", // default device name
  things: [
    { name: "Ceiling Fan", type: "fan", hub: "hubitat", rooms: [ "Family Room"]},
    { name: "Ceiling Fan Light", type: "dimmer", hub: "hubitat", rooms: [ "Family Room"]},
    { name: "Ceiling Fan Light", type: "dimmer", hub: "hubitat", rooms: [ "Family Room"]},
    { name: "Front Door Lock", type: "lock", hub: "hubitat", rooms: ["Family Room", "Foyer"]},
    ...
  ],
},
```

## tvguide member

This root member is an Object that contains configuration for one or more TV Guide data to acquire and update.

The data are provided by SchedulesDirect.  A subscription costs $25/year.

To obtain your guide/device code: 
-    https://github.com/SchedulesDirect/JSON-Service/wiki/API-20141201#obtain-the-list-of-headends-in-a-postal-code

### Example

```
tvguide: {
  guides: [
    { device: "CA00053"", name:" "Beverly Hills Time Warner Cable" }
  ],
}
```

# See Also:

## Documentation

- [Microservice Architecture](Microservice.md) - About Microservices.
- [Macros](Macros.js) - How RoboDomo Macros work.

## Repositories

- The [macros-microservice](https://github.com/RoboDomo/macros-microservice) repository.
- The [config-microservice](https://github.com/RoboDomo/config-microservice) repository.
- The [presence-microservice](https://github.com/RoboDomo/presence-microservice) repository.
- The [autelis-microservice](https://github.com/RoboDomo/autelis-microservice) repository.
- The [nest-mircroservice](https://github.com/RoboDomo/nest-microservice) repository.
- The [icomfort-mircroservice](https://github.com/RoboDomo/icomfort-microservice) repository.
