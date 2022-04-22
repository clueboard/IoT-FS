# Docker Compose

Docker Compose is the standard way to manage services using Docker. While it is focused on defining and running multi-container applications it also works well for managing the configuration of single-container applications. See the [official documentation](https://docs.docker.com/compose/) for a more in-depth explanation.

## Structure For Your Docker Compose Infrastructure

While it is possible to work with multiple `docker-compose.yml` files within the same directory, most people find it easier to build a directory tree to store their services and configuration. This allows for a lot of flexibility when it comes to building and managing your services. There are many ways you manage these files, but this is what we recommend for maximum flexibility and ease of use.

### Basic setup

First, pick a home for your directory tree. This can be anywhere, but in most situations we recommend `/srv/docker`. For the rest of this guide we will assume you have placed your directory tree there.

```bash
sudo mkdir -p /srv/docker-compose
sudo chown `whoami` /srv/docker-compose
```

Within this directory you will create directories for every service you want to run.

!!! note

    The term "service" here is used in a slightly different way that you may be used to. In this context a service is any collection of one or more containers. Each container runs a single daemon process, which is what you may be used to thinking of as a service.

### Create Your First Service

Let's start by setting up a service for the Mosquitto MQTT broker. This will demonstrate setting up a simple service which someone else has published a container for.

First we'll create directories for it. Notice that we have created a directory in `/srv/docker-compose` and directories for volume mounts in `/srv/mosquitto`. If you are setting up a service that doesn't need volume mounts you can skip that.

```bash
sudo mkdir -p /srv/docker-compose/mosquitto /srv/mosquitto/conf /srv/mosquitto/data /srv/mosquitto/log
```

Create the `/srv/docker-compose/mosquitto/docker-compose.yml` file:

!!! abstract "/srv/docker-compose/mosquitto/docker-compose.yml"

    ```
    version: "3"
    services:
      mosquitto:
        image: eclipse-mosquitto
        volumes:
          - ./conf:/mosquitto/conf
          - ./data:/mosquitto/data
          - ./log:/mosquitto/log
        ports:
          - 1883:1883
          - 9001:9001
    ```

Create the `mosquitto.conf` configuration file:

!!! abstract "/srv/mosquitto/conf/mosquitto.conf"

    ```
    persistence true
    persistence_location /mosquitto/data/
    log_dest file /mosquitto/log/mosquitto.log

    listener 1883
    ```

Once this is in place you can use `docker-compose` to bring it up:

```bash
cd /srv/docker-compose/mosquitto
docker-compose up --detach
```

If you want to watch the logs you can attach to them like this:

```
cd /srv/docker-compose/mosquitto
docker-compose logs
```

To bring the service down:

```bash
cd /srv/docker-compose/mosquitto
docker-compose down
```

### Custom Docker Containers

Sometimes you need to customize a container before it gets deployed. Docker provides a mechanism for doing so using `Dockerfile`. We'll demonstrate how to do that by creating a custom nginx service.

As before we need to create the docker-compose directory:

```bash
sudo mkdir -p /srv/docker-compose/nginx/nginx
```

Notice that we did not include a data directory in `/srv`, and we added an extra `/nginx` to the end of our path? The first `nginx` path component refers to the service, while the second `nginx` path component refers to the container.

Now we create the `docker-compose.yml` file:

!!! abstract "/srv/docker-compose/nginx/docker-compose.yml"

    ```
    version: '3'
    services:
      nginx:
        build: ./nginx
        ports:
          - "80:80"
    ```

This is where the second nginx directory comes into play- we are going to create a `Dockerfile` and an `index.html` inside of there.

!!! abstract "/srv/docker-compose/nginx/nginx/Dockerfile"

    ```
    FROM nginx:mainline
    COPY index.html /usr/share/nginx/html
    ```

!!! abstract "/srv/docker-compose/nginx/nginx/index.html"

    ```
    <h1>Hello, World!</h1>
    ```

With these two files in place you are ready to start nginx:

```bash
cd /srv/docker-compose/nginx
docker-compose up --detach
```

### Writing Your Own Service

Now we'll look at a more complex example- we'll implement our own service along with the infrastructure it needs. In this example we'll stub out an application that uses a redis backend. As before, we start by creating the directories we need:

```bash
sudo mkdir -p /srv/docker-compose/my_cool_app/my_cool_app
```

Create some files:

!!! abstract "/srv/docker-compose/my_cool_app/docker-compose.yml"

    ```
    version: '3'
    services:
      my_cool_app:
        build: ./nginx
        ports:
          - "80:80"
      redis:
        image: redis:latest
        ports:
          - "6379:6379"
        restart: always
    ```

!!! abstract "/srv/docker-compose/my_cool_app/my_cool_app/Dockerfile"

    ```
    FROM debian:stable
    COPY my_cool_app /
    ENTRYPOINT ["/my_cool_app"]
    ```

!!! abstract "/srv/docker-compose/my_cool_app/my_cool_app/my_cool_app"

    ```
    #!/usr/bin/env python3
    import time
    print('Hello, World!')
    while True:
        sleep(0.1)
    ```

With these files in place you're ready to run your service. There are two ways you can run it:

**Development mode**:

Use this while you are actively developing the app. Logs for all services will display on STDOUT and pressing ^C will stop all the containers.

```bash
cd /srv/docker-compose/my_cool_app
docker-compose down  # This ensures that you have a fresh start
docker-compose up
```

**Production mode**:

Use this when you want your service to run normally, even after you logout.

```bash
cd /srv/docker-compose/my_cool_app
docker-compose down  # This ensures that you have a fresh start
docker-compose up --detach
```
