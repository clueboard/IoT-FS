# Infrastructure

Infrastructure is the physical hardware and service management software you use to deploy your microservices. In this section we'll cover some popular options and point you towards resources for building them. If you're just getting started and aren't sure which option to use, pick whatever you are comfortable with- microservices mean you can always migrate your setup one piece at a time to ensure continuity of service.

## Recommendations That Apply To All Solutions

You'll want to do some basic capacity planning to know whether your system can handle your planned load. Here are some things to keep in mind when doing that.

* Mosquitto consumes around 100mb RAM.
* Each python service consumes at least 15mb RAM.
* With proper log rotation my stored data is less than 1gb.
* Homebridge can consume a lot of RAM, depending on how many plugins you run. I regularly see 150mb+.
* If you add HomeAssistant, openHab, or other "all-in-one" systems your footprint will significantly increase.

## Solutions

The solutions on the following pages are ordered by ease of getting started. They also build on each other- you can get started with a simple Linux server, add Docker to it when you're ready, and if you find it would be helpful you can add Kubernetes down the road. That's one of the benefits of building this way- migrations can be done a piece at a time, rather than forklifted.
