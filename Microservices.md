# Microservice Architecture

RoboDomo uses a Docker-based Microservice Architecture strategy.  Rather than one big monolithic back end that knows how to interact with all the things and cloud services you need to control your smart home, a simple microservice is implemented for each kind of thing or cloud service.

For example, the [denon-microservice](https://github.com/Robodomo/denon-microservice) is a simple program consisting of 167 lines (at the time of this writing) of JavaScript for Node JS.  This includes several lines of comments.  The focus of this program is simply controlling and monitoring Denon AVR (audio recweiver) devices.  It doesn't have to know about Ring doorbell API or how to turn on lights.  It only needs to know how to change the input on the AVR, change the volume, monitor the volume, and so on.

