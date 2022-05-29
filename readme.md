# Contents

---

- [Contents](#contents)
- [Containers](#containers)
- [Docker](#docker)
  - [Overview](#overview)
  - [The Docker Platform](#the-docker-platform)
  - [Docker Architecture](#docker-architecture)
    - [The Docker daemon](#the-docker-daemon)
    - [The Docker client](#the-docker-client)
    - [Docker Desktop](#docker-desktop)
    - [Docker registries](#docker-registries)
    - [Docker objects](#docker-objects)
      - [Images](#images)
      - [Containers](#containers-1)
  - [Hands on Docker Containers](#hands-on-docker-containers)
    - [Basic Docker Commands](#basic-docker-commands)

---

# Containers

-   Containers are an **operating system virtualization** technology used to package applications and their dependencies and run them in isolated environments.
-   A single container might be used to run anything from a small microservice or software process to a larger application.
-   They provide a lightweight method of packaging and deploying applications in a standardized way across many different types of infrastructure.

# Docker

## Overview

-   Docker is an open platform for developing, shipping, and running applications.
-   It enables us to separate our applications from our infrastructure so we can deliver software quickly.
-   With Docker, we can manage our infrastructure in the same ways we manage our applications.
-   By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, we can significantly reduce the delay between writing code and running it in production.

## The Docker Platform

-   Docker provides the ability to package and run an application in a loosely isolated environment called a **container**.
-   The isolation and security allows to run many containers simultaneously on a given host.
-   Containers are lightweight and contain everything needed to run the application, so we do not need to rely on what is currently installed on the host.
    <br>

-   Docker provides tooling and a platform to manage the lifecycle of our containers :
    -   Develop application and its supporting components using containers.
    -   The container becomes the unit for distributing and testing the application.
    -   Deploy application into production environment, as a container or an orchestrated service. This works the same whether production environment is a local data center, a cloud provider, or a hybrid of the two.

## Docker Architecture

-   Docker uses a **client-server architecture**.
-   The Docker _client_ talks to the Docker _daemon_, which does the heavy lifting of building, running, and distributing Docker containers.
-   The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon.
-   The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface.
-   Another Docker client is **Docker Compose**, that lets you work with applications consisting of a set of containers.

<div align='center'>

![architecture](./images/architecture.svg)

</div>

### The Docker daemon

-   The **Docker daemon** (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes.
-   A daemon can also communicate with other daemons to manage Docker services.

### The Docker client

-   The **Docker client** (`docker`) is the primary way that many Docker users interact with Docker.
-   When we use commands such as `docker run`, the client sends these commands to `dockerd`, which carries them out.
-   The `docker` command uses the **Docker API**.
-   The Docker client can communicate with more than one daemon.

### Docker Desktop

-   **Docker Desktop** is an easy-to-install application for Mac or Windows environment that enables to build and share containerized applications and microservices.
-   **Docker Desktop** includes the **Docker daemon** (`dockerd`), the **Docker client** (`docker`), **Docker Compose**, **Docker Content Trust**, **Kubernetes**, and **Credential Helper**.

### Docker registries

-   A **Docker registry** stores **Docker images**.
-   **Docker Hub** is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default.
-   When we use the `docker pull` or `docker run` commands, the required images are pulled from the configured registry.
-   When we use the `docker push` command, our image is pushed to our configured registry.

### Docker objects

-   When we use Docker, we are creating and using images, containers, networks, volumes, plugins, and other objects.

#### Images

-   An _image_ is a read-only template with instructions for creating a Docker container.

#### Containers

-   A container is a runnable instance of an image.
-   We can create, start, stop, move, or delete a container using the Docker API or CLI.
-   We can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.
    <br>

-   By default, a container is relatively well isolated from other containers and its host machine.

## Hands on Docker Containers

-   Copy the contents of the `docker/Vagrantfile` file and run `vagrant up`.
-   The `Vagrantfile` is configured to provision the installation of Docker and run the service.
-   Now ssh to the VM and run the commands below :
-   ```
    $ vagrant ssh
    $ sudo -i
    $ systemctl status docker     # Check the status of docker service
      ● docker.service - Docker Application Container Engine
          Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
          Active: active (running) since Sun 2022-05-29 18:44:02 UTC; 2min 26s ago
      TriggeredBy: ● docker.socket
          Docs: https://docs.docker.com
      Main PID: 3112 (dockerd)
          Tasks: 8
          Memory: 30.1M
          CGroup: /system.slice/docker.service
                  └─3112 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

    $ docker run hello-world      # test docker by downloading and running a container

      Unable to find image 'hello-world:latest' locally
      latest: Pulling from library/hello-world
      2db29710123e: Pull complete
      Digest: sha256:80f31da1ac7b312ba29d65080fddf797dd76acfb870e677f390d5acba9741b17
      Status: Downloaded newer image for hello-world:latest

      Hello from Docker!
      This message shows that your installation appears to be working correctly.

      To generate this message, Docker took the following steps:
      1. The Docker client contacted the Docker daemon.
      2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
          (amd64)
      3. The Docker daemon created a new container from that image which runs the
          executable that produces the output you are currently reading.
      4. The Docker daemon streamed that output to the Docker client, which sent it
          to your terminal.

      To try something more ambitious, you can run an Ubuntu container with:
      $ docker run -it ubuntu bash

      Share images, automate workflows, and more with a free Docker ID:
      https://hub.docker.com/

      For more examples and ideas, visit:
      https://docs.docker.com/get-started/
    ```

### Basic Docker Commands

-   `docker images` : Shows the available images on the system.
-   `docker ps` : Shows the running containers.
-   `docker ps -a` : Shows all the containers.
-   `docker run --name web01 -dp 9080:80 nginx` :
    -   `-d` : Runs in detached mode (in background)
    -   `-p` : Specify port mapping b/w host and container port (9080 for host and 80 for container here)
    -   `--name` : Name for the container
    -   `nginx` : Image name
-   `docker inspect web01` : Shows the IP Address and other relevant info.
-   `docker stop web01 heuristic_hugle` : Stops the specified containers.
-   `docker rm web01 heuristic_hugle` : Removes the specified containers.
-   `docker rmi <image_id>` : Removes the images specified
