---
description: Using Docker Machine to provision hosts on cloud providers
keywords: docker, machine, amazonec2, azure, digitalocean, google, openstack, rackspace, softlayer, virtualbox, vmwarefusion, vmwarevcloudair, vmwarevsphere, exoscale
title: Docker Machine 利用によるクラウドプロバイダーへのプロビジョニング
hide_from_sitemap: true
---

{% comment %}
Docker Machine driver plugins are available for many cloud platforms, so you can
use Machine to provision cloud hosts. When you use Docker Machine for
provisioning, you create cloud hosts with Docker Engine installed on them.
{% endcomment %}
クラウドプラットフォームに向けた Docker Machine ドライバープラグインが数多くあります。
このため Docker Machine を使ったクラウドホストの提供が可能になっています。
プロビジョニングに Docker Machine を利用すると、クラウドホスト上に Docker Engine をインストールしてホスト構築を行うことができます。

{% comment %}
Install and run Docker Machine, and create an account with the
cloud provider.
{% endcomment %}
Docker Machine のインストールと起動を行って、クラウドプロバイダー上にアカウントを生成してください。

{% comment %}
Then you provide account verification, security credentials, and configuration
options for the providers as flags to `docker-machine create`. The flags are
unique for each cloud-specific driver. For instance, to pass a DigitalOcean
access token you use the `--digitalocean-access-token` flag. Take a look at the
examples below for DigitalOcean and AWS.
{% endcomment %}
Then you provide account verification, security credentials, and configuration
options for the providers as flags to `docker-machine create`. The flags are
unique for each cloud-specific driver. For instance, to pass a DigitalOcean
access token you use the `--digitalocean-access-token` flag. Take a look at the
examples below for DigitalOcean and AWS.

{% comment %}
## Examples
{% endcomment %}
{: #examples }
## 利用例

### DigitalOcean

{% comment %}
For DigitalOcean, this command creates a Droplet (cloud host) called
"docker-sandbox".
{% endcomment %}
For DigitalOcean, this command creates a Droplet (cloud host) called
"docker-sandbox".

```shell
$ docker-machine create --driver digitalocean --digitalocean-access-token xxxxx docker-sandbox
```

{% comment %}
For a step-by-step guide on using Machine to create Docker hosts on Digital
Ocean, see the [DigitalOcean Example](examples/ocean.md).
{% endcomment %}
For a step-by-step guide on using Machine to create Docker hosts on Digital
Ocean, see the [DigitalOcean Example](examples/ocean.md).

{% comment %}
### Amazon Web Services (AWS)
{% endcomment %}
{: #amazon-web-services-aws }
### Amazon ウェブサービス (Amazon Web Services; AWS)

{% comment %}
For AWS EC2, this command creates an instance called "aws-sandbox":
{% endcomment %}
AWS EC2 に対しては、以下のコマンドにより「aws-sandbox」と呼ばれるインスタンスが生成されます。

```shell
$ docker-machine create --driver amazonec2 --amazonec2-access-key AKI******* --amazonec2-secret-key 8T93C*******  aws-sandbox
```

{% comment %}
For a step-by-step guide on using Machine to create Dockerized AWS instances,
see the [Amazon Web Services (AWS) example](examples/aws.md).
{% endcomment %}
For a step-by-step guide on using Machine to create Dockerized AWS instances,
see the [Amazon Web Services (AWS) example](examples/aws.md).

{% comment %}
## The docker-machine create command
{% endcomment %}
{: #the-docker-machine-create-command }
## docker-machine create コマンド

{% comment %}
The `docker-machine create` command typically requires that you specify, at a
minimum:
{% endcomment %}
The `docker-machine create` command typically requires that you specify, at a
minimum:

{% comment %}
* `--driver` - to indicate the provider on which to create the
machine (VirtualBox, DigitalOcean, AWS, and so on)
{% endcomment %}
* `--driver` - to indicate the provider on which to create the
machine (VirtualBox, DigitalOcean, AWS, and so on)

{% comment %}
* Account verification and security credentials (for cloud providers),
specific to the cloud service you are using
{% endcomment %}
* Account verification and security credentials (for cloud providers),
specific to the cloud service you are using

{% comment %}
* `<machine>` - name of the host you want to create
{% endcomment %}
* `<machine>` - name of the host you want to create

{% comment %}
For convenience, `docker-machine` uses sensible defaults for choosing
settings such as the image that the server is based on, but you override the
defaults using the respective flags, such as `--digitalocean-image`. This is
useful if, for example, you want to create a cloud server with a lot of memory
and CPUs, rather than the default behavior of creating smaller servers.
{% endcomment %}
For convenience, `docker-machine` uses sensible defaults for choosing
settings such as the image that the server is based on, but you override the
defaults using the respective flags, such as `--digitalocean-image`. This is
useful if, for example, you want to create a cloud server with a lot of memory
and CPUs, rather than the default behavior of creating smaller servers.

{% comment %}
For a full list of the flags/settings available and their defaults, see the
output of `docker-machine create -h` at the command line, the
[create](reference/create.md){: target="_blank" class="_" } command in
the Machine [command line reference](reference/index.md){:
target="_blank" class="_" }, and [driver options and operating system defaults](drivers/os-base.md){: target="_blank" class="_" }
in the Machine driver reference.
{% endcomment %}
For a full list of the flags/settings available and their defaults, see the
output of `docker-machine create -h` at the command line, the
[create](reference/create.md){: target="_blank" class="_" } command in
the Machine [command line reference](reference/index.md){:
target="_blank" class="_" }, and [driver options and operating system defaults](drivers/os-base.md){: target="_blank" class="_" }
in the Machine driver reference.

{% comment %}
## Drivers for cloud providers
{% endcomment %}
## Drivers for cloud providers

{% comment %}
When you install Docker Machine, you get a set of drivers for various cloud
providers (like Amazon Web Services, DigitalOcean, or Microsoft Azure) and
local providers (like Oracle VirtualBox, VMWare Fusion, or Microsoft Hyper-V).
{% endcomment %}
When you install Docker Machine, you get a set of drivers for various cloud
providers (like Amazon Web Services, DigitalOcean, or Microsoft Azure) and
local providers (like Oracle VirtualBox, VMWare Fusion, or Microsoft Hyper-V).

{% comment %}
See [Docker Machine driver reference](drivers/index.md){:target="_blank" class="_"}
for details on the drivers, including required flags and configuration options
(which vary by provider).
{% endcomment %}
See [Docker Machine driver reference](drivers/index.md){:target="_blank" class="_"}
for details on the drivers, including required flags and configuration options
(which vary by provider).

{% comment %}
## 3rd-party driver plugins
{% endcomment %}
{: #3rd-party-driver-plugins }
## サードパーティ製のドライバープラグイン

{% comment %}
Several Docker Machine driver plugins for use with other cloud platforms are
available from 3rd party contributors. These are use-at-your-own-risk plugins,
not maintained by or formally associated with Docker.
{% endcomment %}
Several Docker Machine driver plugins for use with other cloud platforms are
available from 3rd party contributors. These are use-at-your-own-risk plugins,
not maintained by or formally associated with Docker.

{% comment %}
See [Available driver plugins](https://github.com/docker/docker.github.io/blob/master/machine/AVAILABLE_DRIVER_PLUGINS.md){:target="_blank" class="_"}.
{% endcomment %}
See [Available driver plugins](https://github.com/docker/docker.github.io/blob/master/machine/AVAILABLE_DRIVER_PLUGINS.md){:target="_blank" class="_"}.

{% comment %}
## Add a host without a driver
{% endcomment %}
## Add a host without a driver

{% comment %}
You can register an already existing docker host by passing the daemon url. With that, you can have the same workflow as on a host provisioned by docker-machine.
{% endcomment %}
You can register an already existing docker host by passing the daemon url. With that, you can have the same workflow as on a host provisioned by docker-machine.

    $ docker-machine create --driver none --url=tcp://50.134.234.20:2376 custombox
    $ docker-machine ls
    NAME        ACTIVE   DRIVER    STATE     URL
    custombox   *        none      Running   tcp://50.134.234.20:2376

{% comment %}
## Use Machine to provision Docker Swarm clusters
{% endcomment %}
## Use Machine to provision Docker Swarm clusters

{% comment %}
> Swarm mode supersedes Docker Machine provisioning of swarm clusters
>
> In previous releases, Docker Machine was used to provision swarm
> clusters, but this is legacy. [Swarm mode](../engine/swarm/index.md), built
> into Docker Engine, supersedes Machine provisioning of swarm clusters. The
> topics below show you how to get started with the new swarm mode.
{: .important}
{% endcomment %}
> Swarm mode supersedes Docker Machine provisioning of swarm clusters
>
> In previous releases, Docker Machine was used to provision swarm
> clusters, but this is legacy. [Swarm mode](../engine/swarm/index.md), built
> into Docker Engine, supersedes Machine provisioning of swarm clusters. The
> topics below show you how to get started with the new swarm mode.
{: .important}

{% comment %}
You can use Docker Machine to create local virtual hosts on which to deploy
and test [swarm mode](../engine/swarm/index.md) clusters.
{% endcomment %}
You can use Docker Machine to create local virtual hosts on which to deploy
and test [swarm mode](../engine/swarm/index.md) clusters.

{% comment %}
Good places to start working with Docker Machine and swarm mode are these
tutorials:
{% endcomment %}
Good places to start working with Docker Machine and swarm mode are these
tutorials:

{% comment %}
* [Get started with Docker](../get-started/index.md)
{% endcomment %}
* [Get started with Docker](../get-started/index.md)

{% comment %}
* [Getting started with swarm mode](../engine/swarm/swarm-tutorial/index.md)
{% endcomment %}
* [Getting started with swarm mode](../engine/swarm/swarm-tutorial/index.md)


{% comment %}
## Where to go next
{% endcomment %}
## Where to go next
{% comment %}
-   Example: Provision Dockerized [DigitalOcean Droplets](examples/ocean.md)
-   Example: Provision Dockerized [AWS EC2 Instances](examples/aws.md)
-   [Understand Machine concepts](concepts.md)
-   [Docker Machine driver reference](drivers/index.md)
-   [Docker Machine subcommand reference](reference/index.md)
-   [Provision a Docker Swarm cluster with Docker Machine](../swarm/provision-with-machine.md)
{% endcomment %}
-   Example: Provision Dockerized [DigitalOcean Droplets](examples/ocean.md)
-   Example: Provision Dockerized [AWS EC2 Instances](examples/aws.md)
-   [Understand Machine concepts](concepts.md)
-   [Docker Machine driver reference](drivers/index.md)
-   [Docker Machine subcommand reference](reference/index.md)
-   [Provision a Docker Swarm cluster with Docker Machine](../swarm/provision-with-machine.md)
