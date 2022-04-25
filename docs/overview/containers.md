# Containers

In recent years containers have become the preferred way to deploy software at scale. Docker Containers are the most widely used, but all container technology shares the same basic premise- the userland of the OS is the deployable unit. When you launch a container it does not run in the host OS, but in a self-contained and isolated environment. Every tool, library, and configuration file available to your service is bundled together and deployed as a single unit.

This decouples your applications from one another, and from the host OS. You will be able to install applications with conflicting dependencies side by side, because each application will only its own files. You can still share files and other resources between applications if necessary, but those applications can be running on wildly different software stacks.
