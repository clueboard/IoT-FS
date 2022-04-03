# Getting Started

This page will give you a broad overview of how to build your IoT-FS smart home. Our goal is to document the paths, not to recommend one over another.

## Target Audience

This guide assumes you already have a basic understand of developing software in at least one programming language. You should already be familiar with concepts like editing plain text files, writing structured files like JSON and YAML, and using HTTP or other network protocols. You don't need to know any language in particular, you can run someone else's code without knowing the programming language, and you can write your own services in whatever language you want.

If your current or desired job title is on this list, IoT-FS is an excellent way to develop your skills and build a portfolio of personal projects to show off.

* Programmer
* Software Engineer
* DevOps
* Site Reliability Engineer
* Infrastructure/Platform Engineer

## Step 0- Understanding

There are some basic concepts you will need to familiarize yourself with as you begin your journey. You don't have to dive deep, for now a surface understanding of the technology is enough.

### Step 0.0- Infrastructure

Infrastructure is the physical hardware and service management software you use to deploy your microservices. There are a lot of choices here, and what you pick will mostly depend on what you want to learn. There is a wide variety of hardware to pick from as well, such as compact PCs, server PCs, and Raspberry Pi. These choices are beyond the scope of this guide, but later we provide some examples of platforms you might want to use.

### Step 0.1- Understanding MQTT

MQTT is the core infrastructure you will build your IoT-FS around. It is a lightweight message transport that you will both read from and publish to. Messages sent to MQTT can be read by anyone, and you can tell the MQTT server which messages you are interested in receiving. If you want the nitty gritty details you can head over to [mqtt.org](https://mqtt.org/), but read on for a basic overview of what you need to know for now.

Messages sent to MQTT are (usually) delivered to clients in the order they are received. If your connection to the server is stable they will always be in order. If your connection is not stable it will depend on the Quality Of Service (QOS) level set- if QOS=0 a message may not get delivered, if QOS=1 the message may get delivered more than once, and if QOS=2 the message will get delivered exactly once. It's not important to understand the difference between these right now, it's enough to know that you can choose the level of reliability to match your application's needs.

When a client sends a message to MQTT it contains two important pieces of information, the `topic` and the `payload`.

### Step 0.1.1- MQTT Topics

The topic is how the MQTT server routes messages. Clients subscribe to one or more topics and the MQTT server routes all matching messages to those clients. Topics are basically file paths, they are slash (`/`) separated strings that indicate unique resources. Here are some examples of topics I see on my MQTT server:

* `homeassistant/binary_sensor/Kitchen-FrontdoorDetector/contact/config`
* `weather/daily/0/temp/day_C`
* `rtl_433/7b85b6eafe6d/devices/Acurite-Tower/A/9181/temperature_C`
* `zwave/Kitchen/FrontdoorDetector/113/0/Access_Control/Door_state`

As you can see there is a structure to MQTT topics, but that structure is not always well defined.

### Step 0.1.1- MQTT Payloads

To the MQTT server a payload is just an array of bytes. It passes along this payload exactly as it was received. It's up to the client to correctly decode the payload. In practice payloads are almost always ASCII or UTF-8 encoded text.

(Note: In MQTT 5 there is new metadata to indicate the payload's format and content type. We are focusing on MQTT 3 for now as it is the most widelyl-deployed protocol.)

Some applications use single-value payloads. These payloads are typically strings or simple ints or floats encoded as strings. Some applications encode their payload using JSON to allow for more complex data to be sent. Neither pattern is right or wrong, they simply grew out of the requirements of the application.

### Step 0.1.2- MQTT Standards

As you can tell from reading the last two sections MQTT allows for a broad range of usage patterns. There have been two attempts to standardize these patterns that are worth your time to read:

* [mqtt-smarthome](https://github.com/mqtt-smarthome/mqtt-smarthome)
* [The Homie Convention](https://github.com/homieiot/convention) 

As you read these keep in mind that neither standard has been widely adopted.

### Step 1- Build Your Infrastructure

Before you can deploy anything you need infrastructure to deploy it to. What you use and how your deployments work is up to you, here are some common choices:

* Linux and [systemd](https://systemd.io/)
    * This is probably the simplest to get started with, and in many cases is all you need. You can use any Linux Distribution you want, such as [Arch](https://archlinux.org/), [Debian](https://www.debian.org/), [Fedora](https://getfedora.org/), [Mint](https://linuxmint.com/), [Raspian](https://www.raspberrypi.com/software/), [Ubuntu](https://ubuntu.com/), etc.
    * Resource Overhead: This has the least overhead of the 4 options. Use this when you need to maximise limited hardware.
    * Administration Overhead: Since everything is on a single server you might have problems getting two microservices to play nice with each other. Expect to spend time sorting out conflicts and dealing with large upgrade sets.
* Linux and Docker
    * There are a wide variety of options for running Docker, including on your workstation, server, NAS, or cluster.
    * Resource Overhead: While docker containers have low CPU overhead expect to use more disk space and more memory.
    * Administration Overhead: It varies. You don't have the conflicts to deal with like the systemd case, but you may have to deal with docker getting into weird states.
* Linux and Kubernetes
    * [Kubernetes](https://kubernetes.io/) (K8s) is a production-grade orchestration service. It builds on top of docker to provide a self-healing container infrastructure. Typically used when you have multiple computers you want to run in a cluster. While it can be a lot to setup and manage it provides unparalleled reliability.
    * Resource Overhead: More than Docker, you have a set of services that are constantly monitoring the cluster.
    * Administration Overhead: High. You have to learn a lot to setup the environment and keep it running. However, you almost never have to deal with ongoing maintenance, the overhead only happens when making changes.
* Rancher
    * [Rancher](https://rancher.com/) builds on top of Kubernetes, providing a nice GUI and infrastructure to make managing K8s easier. 
    * Resource Overhead: Very high. The UI needs 2GB just to start, and more as your services grow.
    * Administration Overhead: Lower than K8s. It handles a lot for you and the GUI means you can figure a lot out without tearing your hair out reading docs.

### Step 2- Deploy MQTT

Literally everything you want to deploy will depond on this infrastructure. Give it the attention its due on setup and you'll never have to worry about it again.

You'll need to pick a broker. There are [a lot of them available](https://github.com/hobbyquaker/awesome-mqtt#broker). However, almost everyone uses [Mosquitto](http://mosquitto.org/), and you should too unless you have a very good reason.

Follow this checklist to make sure you've done everything:

<!-- FIXME: Flesh this out with instructions -->
* Install Mosquitto
* Test to make sure it works
* Setup authentication
* Setup SSL/TLS

### Step 3- Metrics and Visualization

Optional but very helpful.

Find yourself a graphing system you like and deploy it. There are lots to choose from.

* graphite- easy to get going, outdated UI, can be hard to get some features working
* influxdb- successor to graphite, v2 is harder to setup and less open source friendly
* prometheus- docker/k8s darling

### Step 4- Logs

Optional but very helpful.

You'll want to be able to look at logs, and there are systems that can make searching them across your cluster easier. If you only have a single machine it's very managable to only use the Docker and Systemd logging facilities, but as your infrastructure grows these systems will grow with you:

* ELK
* Graylog
* Fluentd

### Step 5- Deploy your first app

This is where you'll deploy your first microservice. You probably have some hardware you're looking at using, here are some good choices for first applications to deploy:

* zwavejs2mqtt
* zigbee2mqtt
* homebridge
* esphome
* tasmota

### Step 6- Deploy more apps

Now that you've deployed one application it's time to build on your foundation. Deploy more apps until all the hardware you want to monitor and/or control is working on MQTT.

### Step 7- Iterate

At this point you should have a good handle on where to go from here. Iterate until you're happy with your system, if you can ever be happy. :)
