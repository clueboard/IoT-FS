# Server(s) Without Any Virtualization

Once upon a time this was the most popular setup for most business and home servers. You just install all the software you need and use systemd, launchd, or similar to start it and keep it running. If you are just experimenting over a weekend this is the fastest and easiest way to get started building your IoT infrastructure.

## Hardware Recommendations

This section outlines the available options for hardware, ordered from strongest to weakest recommendation. Remember that you are not locked into this choice, if you need to migrate to a new choice later you can do that.

1. Whatever you have laying around
    * The main benefit of choosing this solution is that you can use anything you have available, such as a Raspberry Pi, old computer, etc.
    * All you have to do is install the Linux distribution of your choice.
2. The computer you're using right now
    * If it's Linux or macOS you can just start installing software.
    * On Windows you may want to consider [WSL](https://docs.microsoft.com/en-us/windows/wsl/about) or [MSYS2](https://www.msys2.org/).
3. A [Raspberry Pi](https://www.raspberrypi.com/)
    * At only $15 a Pi Zero is the least expensive way to get started
    * The value of a $35 Pi 4 is hard to beat
4. Any PC
    * There are PCs available at nearly every price point, you can source or build one that meets your exact needs.

## What Linux Distribution Should I Use?

If you're just starting out and have no idea what to use, pick the latest [Ubuntu LTS](https://ubuntu.com/blog/what-is-an-ubuntu-lts-release) release. Ubuntu is popular and well known, and you will be able to find lots of help for it.

If you have a favorite Linux distribution already use that. Likewise, if your employer has a distribution they use pick that one instead. This will help you learn the tools you use at work.
