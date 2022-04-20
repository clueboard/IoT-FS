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
