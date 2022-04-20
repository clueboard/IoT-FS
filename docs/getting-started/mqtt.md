### Understanding MQTT

MQTT is the core infrastructure you will build your IoT-FS around. It is a lightweight message transport that you will both read from and publish to. Messages sent to MQTT can be read by anyone, and you can tell the MQTT server which messages you are interested in receiving. If you want the nitty gritty details you can head over to [mqtt.org](https://mqtt.org/), but read on for a basic overview of what you need to know for now.

Messages sent to MQTT are (usually) delivered to clients in the order they are received. If your connection to the server is stable they will always be in order. If your connection is not stable it will depend on the Quality Of Service (QOS) level set- if QOS=0 a message may not get delivered, if QOS=1 the message may get delivered more than once, and if QOS=2 the message will get delivered exactly once. It's not important to understand the difference between these right now, it's enough to know that you can choose the level of reliability to match your application's needs.

When a client sends a message to MQTT it contains two important pieces of information, the `topic` and the `payload`.

### MQTT Topics

The topic is how the MQTT server routes messages. Clients subscribe to one or more topics and the MQTT server routes all matching messages to those clients. Topics are basically file paths, they are slash (`/`) separated strings that indicate unique resources. Here are some examples of topics I see on my MQTT server:

* `homeassistant/binary_sensor/Kitchen-FrontdoorDetector/contact/config`
* `weather/daily/0/temp/day_C`
* `rtl_433/7b85b6eafe6d/devices/Acurite-Tower/A/9181/temperature_C`
* `zwave/Kitchen/FrontdoorDetector/113/0/Access_Control/Door_state`

As you can see there is a structure to MQTT topics, but that structure is not always well defined.

### MQTT Payloads

To the MQTT server a payload is just an array of bytes. It passes along this payload exactly as it was received. It's up to the client to correctly decode the payload. In practice payloads are almost always ASCII or UTF-8 encoded text.

(Note: In MQTT 5 there is new metadata to indicate the payload's format and content type. We are focusing on MQTT 3 for now as it is the most widely-deployed protocol.)

Some applications use single-value payloads. These payloads are typically strings or simple ints or floats encoded as strings. Some applications encode their payload using JSON to allow for more complex data to be sent. Neither pattern is right or wrong, they simply grew out of the requirements of the application.

### MQTT Standards

As you can tell from reading the last two sections MQTT allows for a broad range of usage patterns. There have been two attempts to standardize these patterns that are worth your time to read:

* [mqtt-smarthome](https://github.com/mqtt-smarthome/mqtt-smarthome)
* [The Homie Convention](https://github.com/homieiot/convention) 

As you read these keep in mind that neither standard has been widely adopted.
