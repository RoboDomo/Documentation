# Quick Start

This guide is intended to get you going as quickly as possible.  It is assumed you will run this on your development
machine, which has docker already installed.

## Clone the following repositories 
1) git clone https://github.com/RoboDomo/docker-scripts
2) git clone https://github.com/RoboDomo/config

## Set up weather
Follow the instructions in the docker-scripts/start-here.sh file.

## Configure RoboDomo for your home/office
In the config repository, copy the Config.sample.js to Config.js and edit it.  There are many comments
in the file to help guide you.

The bare minimum configuration you likely want is weather.  This will allow you to have 2 working tiles in your 
dashboard(s).  Make sure the locations in your Config.js includes only the ```WEATHER_LOCATIONS``` you set per the
start-here.sh script.

## Start microservices
In the docker-scripts directory, edit the start-all.sh script and only start the microservices you want.

The following microservices are required, so don't comment out the lines that start them:
1) start-mongodb.sh
2) start-mqtt.sh

## start the config microservice
In the config repository, run npm start.  This will load your Config.js into mongodb and start the config-microservice.

The config-microservice monitors the Config.js (and macros.js) files and if they change, they are reloaded into mongodb,
and the affected microservices and the client will restart to reflect your configuration changes.

