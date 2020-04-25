---
description: Introduction and Overview of Machine
keywords: docker, machine, amazonec2, azure, digitalocean, google, openstack, rackspace, softlayer, virtualbox, vmwarefusion, vmwarevcloudair, vmwarevsphere, exoscale
title: Docker Machine 概要
hide_from_sitemap: true
---

{% comment %}
You can use Docker Machine to:
{% endcomment %}
Docker Machine を用いて以下を行います。

{% comment %}
* Install and run Docker on Mac or Windows
* Provision and manage multiple remote Docker hosts
* Provision Swarm clusters
{% endcomment %}
* Mac や Windows 上に Docker をインストールして実行します。
* 複数のリモート Docker ホストを導入し管理します。
* Swarm クラスターを導入します。

{% comment %}
## What is Docker Machine?
{% endcomment %}
{: #what-is-docker-machine }
## Docker Machine とは？

{% comment %}
Docker Machine is a tool that lets you install Docker Engine on virtual hosts,
and manage the hosts with `docker-machine` commands. You can use Machine to
create Docker hosts on your local Mac or Windows box, on your company network,
in your data center, or on cloud providers like Azure, AWS, or DigitalOcean.
{% endcomment %}
Docker Machine is a tool that lets you install Docker Engine on virtual hosts,
and manage the hosts with `docker-machine` commands. You can use Machine to
create Docker hosts on your local Mac or Windows box, on your company network,
in your data center, or on cloud providers like Azure, AWS, or DigitalOcean.

{% comment %}
Using `docker-machine` commands, you can start, inspect, stop, and restart a
managed host, upgrade the Docker client and daemon, and configure a Docker
client to talk to your host.
{% endcomment %}
Using `docker-machine` commands, you can start, inspect, stop, and restart a
managed host, upgrade the Docker client and daemon, and configure a Docker
client to talk to your host.

{% comment %}
Point the Machine CLI at a running, managed host, and you can run `docker`
commands directly on that host. For example, run `docker-machine env default` to
point to a host called `default`, follow on-screen instructions to complete
`env` setup, and run `docker ps`, `docker run hello-world`, and so forth.
{% endcomment %}
Point the Machine CLI at a running, managed host, and you can run `docker`
commands directly on that host. For example, run `docker-machine env default` to
point to a host called `default`, follow on-screen instructions to complete
`env` setup, and run `docker ps`, `docker run hello-world`, and so forth.

{% comment %}
Machine _was_ the _only_ way to run Docker on Mac or Windows previous to Docker
v1.12. Starting with the beta program and Docker v1.12,
[Docker Desktop for Mac](../docker-for-mac/index.md) and
[Docker Desktop for Windows](../docker-for-windows/index.md) are available as native apps and the
better choice for this use case on newer desktops and laptops. We encourage you
to try out these new apps. The installers for Docker Desktop for Mac and Docker Desktop for
Windows include Docker Machine, along with Docker Compose.
{% endcomment %}
Machine _was_ the _only_ way to run Docker on Mac or Windows previous to Docker
v1.12. Starting with the beta program and Docker v1.12,
[Docker Desktop for Mac](../docker-for-mac/index.md) and
[Docker Desktop for Windows](../docker-for-windows/index.md) are available as native apps and the
better choice for this use case on newer desktops and laptops. We encourage you
to try out these new apps. The installers for Docker Desktop for Mac and Docker Desktop for
Windows include Docker Machine, along with Docker Compose.

{% comment %}
If you aren't sure where to begin, see [Get Started with Docker](../get-started/index.md),
which guides you through a brief end-to-end tutorial on Docker.
{% endcomment %}
If you aren't sure where to begin, see [Get Started with Docker](../get-started/index.md),
which guides you through a brief end-to-end tutorial on Docker.

{% comment %}
## Why should I use it?
{% endcomment %}
## Why should I use it?

{% comment %}
Docker Machine enables you to provision multiple remote Docker hosts on various
flavors of Linux.
{% endcomment %}
Docker Machine enables you to provision multiple remote Docker hosts on various
flavors of Linux.

{% comment %}
Additionally, Machine allows you to run Docker on older Mac or Windows systems,
as described in the previous topic.
{% endcomment %}
Additionally, Machine allows you to run Docker on older Mac or Windows systems,
as described in the previous topic.

{% comment %}
Docker Machine has these two broad use cases.
{% endcomment %}
Docker Machine has these two broad use cases.

{% comment %}
* **I have an older desktop system and want to run Docker on Mac or Windows**
{% endcomment %}
* **I have an older desktop system and want to run Docker on Mac or Windows**

  {% comment %}
  ![Docker Machine on Mac and Windows](img/machine-mac-win.png){: .white-bg}
  {% endcomment %}
  ![Docker Machine on Mac and Windows](img/machine-mac-win.png){: .white-bg}

  {% comment %}
  If you work primarily on an older Mac or Windows laptop or desktop that doesn't meet the requirements for the new [Docker Desktop for Mac](../docker-for-mac/index.md) and [Docker Desktop for Windows](../docker-for-windows/index.md) apps, then you need Docker Machine to run Docker Engine locally. Installing Docker Machine on a Mac or Windows box with the [Docker Toolbox](../toolbox/overview.md) installer provisions a local virtual machine with Docker Engine, gives you the ability to connect it, and run `docker` commands.
  {% endcomment %}
  If you work primarily on an older Mac or Windows laptop or desktop that doesn't meet the requirements for the new [Docker Desktop for Mac](../docker-for-mac/index.md) and [Docker Desktop for Windows](../docker-for-windows/index.md) apps, then you need Docker Machine to run Docker Engine locally. Installing Docker Machine on a Mac or Windows box with the [Docker Toolbox](../toolbox/overview.md) installer provisions a local virtual machine with Docker Engine, gives you the ability to connect it, and run `docker` commands.

{% comment %}
*  **I want to provision Docker hosts on remote systems**
{% endcomment %}
*  **I want to provision Docker hosts on remote systems**

  {% comment %}
  ![Docker Machine for provisioning multiple systems](img/provision-use-case.png){: .white-bg}
  {% endcomment %}
  ![Docker Machine for provisioning multiple systems](img/provision-use-case.png){: .white-bg}

  {% comment %}
  Docker Engine runs natively on Linux systems. If you have a Linux box as your
  primary system, and want to run `docker` commands, all you need to do is
  download and install Docker Engine. However, if you want an efficient way to
  provision multiple Docker hosts on a network, in the cloud or even locally,
  you need Docker Machine.
  {% endcomment %}
  Docker Engine runs natively on Linux systems. If you have a Linux box as your
  primary system, and want to run `docker` commands, all you need to do is
  download and install Docker Engine. However, if you want an efficient way to
  provision multiple Docker hosts on a network, in the cloud or even locally,
  you need Docker Machine.

  {% comment %}
  Whether your primary system is Mac, Windows, or Linux, you can install Docker
  Machine on it and use `docker-machine` commands to provision and manage large
  numbers of Docker hosts. It automatically creates hosts, installs Docker
  Engine on them, then configures the `docker` clients. Each managed host
  ("**_machine_**") is the combination of a Docker host and a configured client.
  {% endcomment %}
  Whether your primary system is Mac, Windows, or Linux, you can install Docker
  Machine on it and use `docker-machine` commands to provision and manage large
  numbers of Docker hosts. It automatically creates hosts, installs Docker
  Engine on them, then configures the `docker` clients. Each managed host
  ("**_machine_**") is the combination of a Docker host and a configured client.

{% comment %}
## What's the difference between Docker Engine and Docker Machine?
{% endcomment %}
## What's the difference between Docker Engine and Docker Machine?

{% comment %}
When people say "Docker" they typically mean **Docker Engine**, the
client-server application made up of the Docker daemon, a REST API that
specifies interfaces for interacting with the daemon, and a command line
interface (CLI) client that talks to the daemon (through the REST API wrapper).
Docker Engine accepts `docker` commands from the CLI, such as
`docker run <image>`, `docker ps` to list running containers, `docker image ls`
to list images, and so on.
{% endcomment %}
When people say "Docker" they typically mean **Docker Engine**, the
client-server application made up of the Docker daemon, a REST API that
specifies interfaces for interacting with the daemon, and a command line
interface (CLI) client that talks to the daemon (through the REST API wrapper).
Docker Engine accepts `docker` commands from the CLI, such as
`docker run <image>`, `docker ps` to list running containers, `docker image ls`
to list images, and so on.

{% comment %}
{% endcomment %}
![Docker Engine](img/engine.png)

{% comment %}
**Docker Machine** is a tool for provisioning and managing your Dockerized hosts
(hosts with Docker Engine on them). Typically, you install Docker Machine on
your local system. Docker Machine has its own command line client
`docker-machine` and the Docker Engine client, `docker`. You can use Machine to
install Docker Engine on one or more virtual systems. These virtual systems can
be local (as when you use Machine to install and run Docker Engine in VirtualBox
on Mac or Windows) or remote (as when you use Machine to provision Dockerized
hosts on cloud providers). The Dockerized hosts themselves can be thought of,
and are sometimes referred to as, managed "**_machines_**".
{% endcomment %}
**Docker Machine** is a tool for provisioning and managing your Dockerized hosts
(hosts with Docker Engine on them). Typically, you install Docker Machine on
your local system. Docker Machine has its own command line client
`docker-machine` and the Docker Engine client, `docker`. You can use Machine to
install Docker Engine on one or more virtual systems. These virtual systems can
be local (as when you use Machine to install and run Docker Engine in VirtualBox
on Mac or Windows) or remote (as when you use Machine to provision Dockerized
hosts on cloud providers). The Dockerized hosts themselves can be thought of,
and are sometimes referred to as, managed "**_machines_**".

{% comment %}
{% endcomment %}
![Docker Machine](img/machine.png){: .white-bg}

{% comment %}
## Where to go next
{% endcomment %}
## Where to go next

{% comment %}
- [Install Docker Machine](install-machine.md)
- Create and run a Docker host on your [local system using VirtualBox](get-started.md)
- Provision multiple Docker hosts [on your cloud provider](get-started-cloud.md)
- [Provision a Docker Swarm cluster with Docker Machine](../swarm/provision-with-machine.md) (Legacy Swarm)
- [Getting started with swarm mode](../engine/swarm/swarm-tutorial/) (Docker Engine 1.12 and above)
- [Understand Machine concepts](concepts.md)
- [Docker Machine driver reference](drivers/index.md)
- [Docker Machine subcommand reference](reference/index.md)
- [Migrate from Boot2Docker to Docker Machine](migrate-to-machine.md)
{% endcomment %}
- [Install Docker Machine](install-machine.md)
- Create and run a Docker host on your [local system using VirtualBox](get-started.md)
- Provision multiple Docker hosts [on your cloud provider](get-started-cloud.md)
- [Provision a Docker Swarm cluster with Docker Machine](../swarm/provision-with-machine.md) (Legacy Swarm)
- [Getting started with swarm mode](../engine/swarm/swarm-tutorial/) (Docker Engine 1.12 and above)
- [Understand Machine concepts](concepts.md)
- [Docker Machine driver reference](drivers/index.md)
- [Docker Machine subcommand reference](reference/index.md)
- [Migrate from Boot2Docker to Docker Machine](migrate-to-machine.md)
