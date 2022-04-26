# Hardware

You'll need at least one computer to run your IoT-FS infrastructure on. The base requirements to get started are surprisingly small. You may also choose to have multiple computers for performance or redundancy purposes. There's really a wide range of possibilities, and what you choose will depend a lot on your needs.

## Capacity Planning

You'll want to do some basic capacity planning to know whether your system can handle your planned load. Here are some things to keep in mind when doing that.

* Mosquitto consumes around 100mb RAM.
* Each python service consumes at least 15mb RAM, but more likely consumes closer to 50mb of RAM.
* With proper log rotation my stored data is less than 1gb.
* Homebridge can consume a lot of RAM, depending on how many plugins you run. I regularly see 150mb+.
* If you add HomeAssistant, openHab, or other "all-in-one" systems your footprint will significantly increase.
* If you want to record video, especially with motion detection, your footprint will dramatically increase.

In general if your system has at least 2gb of RAM and 64gb of storage you will be just fine, but list out your planned services and the expected RAM and Disk usage to sanity check this assumption.

## Hardware Choices

Almost any hardware is usable for IoT-FS, from a lowly Raspberry Pi running at 1GHz all the way up to a multi-processor Xeon running at 3.5GHz. As such this is what we recommend you use, ordered from strongest to weakest suggestion.

1. Whatever you have laying around
    * The main benefit of choosing this solution is that you can use anything you have available, such as a Raspberry Pi, old computer, etc.
    * All you have to do is install the Linux distribution of your choice.
2. The computer you're using right now
    * Docker is available for all major OSes
3. A [Raspberry Pi](https://www.raspberrypi.com/)
    * At only $15 a Pi Zero is the least expensive way to get started
    * The value of a $35 Pi 4 is hard to beat
4. A PC built to your specifications
    * There are PCs available at nearly every price point, you can source or build one that meets your exact needs.

## Reliability Considerations

The reliability of your system starts with hardware. You want a system that won't randomly lock up due to hardware faults, and you want to protect your data in the event of drive failures. While not strictly necessary, consider using multiple smaller drives over one larger drive, and prefer RAID1 and RAID5/6. Never use RAID0! You are doubling the chance of losing your data with each new drive you add.

### Raspberry Pi and other SD Card systems

You never want to run your IoT-FS on SD cards, they do not hold up to the repeated writes. If you plan to use a system that boots from an SD card consider the following recommendations:

* [Mount /var/log and other locations that are written to regularly to a RAM disk](https://hackaday.com/2019/04/08/give-your-raspberry-pi-sd-card-a-break-log-to-ram/)
* Install fast USB3 SSDs and run the system off that instead
* Deploy multiple systems and use a clustered FS like [Gluster](https://www.gluster.org/) or [Ceph](https://ceph.com/)
