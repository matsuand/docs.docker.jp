---
description: Docker for AWS におけるアプリのデプロイ。
keywords: aws, amazon, iaas, deploy
title: Docker for AWS におけるアプリのデプロイ
---

{% comment %}
## Connect to your manager nodes
{% endcomment %}
{: #connect-to-your-manager-nodes }
## マネージャーノードへの接続

{% comment %}
This section walks you through connecting to your installation and deploying
applications. Instructions are included for both AWS and Azure, so be sure to
follow the instructions for the cloud provider of your choice in each section.
{% endcomment %}
This section walks you through connecting to your installation and deploying
applications. Instructions are included for both AWS and Azure, so be sure to
follow the instructions for the cloud provider of your choice in each section.

{% comment %}
First, you obtain the public IP address for a manager node. Any manager
node can be used for administrating the swarm.
{% endcomment %}
First, you obtain the public IP address for a manager node. Any manager
node can be used for administrating the swarm.

{% comment %}
### Manager Public IP on AWS
{% endcomment %}
### Manager Public IP on AWS

{% comment %}
Once you've deployed Docker on AWS, go to the "Outputs" tab for the stack in
CloudFormation.
{% endcomment %}
Once you've deployed Docker on AWS, go to the "Outputs" tab for the stack in
CloudFormation.

{% comment %}
The "Managers" output is a URL you can use to see the available manager nodes of
the swarm in your AWS console. Once present on this page, you can see the
"Public IP" of each manager node in the table and/or "Description" tab if you
click on the instance.
{% endcomment %}
The "Managers" output is a URL you can use to see the available manager nodes of
the swarm in your AWS console. Once present on this page, you can see the
"Public IP" of each manager node in the table and/or "Description" tab if you
click on the instance.

{% comment %}
![managers](img/managers.png)
{% endcomment %}
![マネージャー](img/managers.png)

{% comment %}
## Connect via SSH
{% endcomment %}
## Connect via SSH

{% comment %}
### Manager nodes
{% endcomment %}
### Manager nodes

{% comment %}
Obtain the public IP and/or port for the manager node as instructed above and
using the provided SSH key to begin administrating your swarm:
{% endcomment %}
Obtain the public IP and/or port for the manager node as instructed above and
using the provided SSH key to begin administrating your swarm:

```bash
$ ssh -i <path-to-ssh-key> docker@<ssh-host>

Welcome to Docker!
```

{% comment %}
Once you are logged into the container you can run Docker commands on the swarm:
{% endcomment %}
Once you are logged into the container you can run Docker commands on the swarm:

```bash
$ docker info

$ docker node ls
```

{% comment %}
You can also tunnel the Docker socket over SSH to remotely run commands on the cluster (requires [OpenSSH 6.7](https://lwn.net/Articles/609321/) or later):
{% endcomment %}
You can also tunnel the Docker socket over SSH to remotely run commands on the cluster (requires [OpenSSH 6.7](https://lwn.net/Articles/609321/) or later):

```bash
$ ssh -i <path-to-ssh-key> -NL localhost:2374:/var/run/docker.sock docker@<ssh-host> &

$ docker -H localhost:2374 info
```

{% comment %}
{% endcomment %}
If you don't want to pass `-H` when using the tunnel, you can set the `DOCKER_HOST` environment variable to point to the localhost tunnel opening.

{% comment %}
{% endcomment %}
### Worker nodes

{% comment %}
{% endcomment %}
As of Beta 13, the worker nodes also have SSH enabled when connecting from
manager nodes. SSH access is not possible to the worker nodes from the public
Internet. To access the worker nodes, you need to first connect to a
manager node (see above).

{% comment %}
{% endcomment %}
On the manager node you can then `ssh` to the worker node, over the private
network. Make sure you have SSH agent forwarding enabled (see below). If you run
the `docker node ls` command you can see the full list of nodes in your swarm.
You can then `ssh docker@<worker-host>` to get access to that node.

#### AWS

{% comment %}
{% endcomment %}
Use the `HOSTNAME` reported in `docker node ls` directly.

```
$ docker node ls
ID                           HOSTNAME                                     STATUS  AVAILABILITY  MANAGER STATUS
a3d4vdn9b277p7bszd0lz8grp *  ip-172-31-31-40.us-east-2.compute.internal   Ready   Active        Reachable
...

$ ssh docker@ip-172-31-31-40.us-east-2.compute.internal
```

{% comment %}
{% endcomment %}
#### Use SSH agent forwarding

{% comment %}
{% endcomment %}
SSH agent forwarding allows you to forward along your ssh keys when connecting
from one node to another. This eliminates the need for installing your private
key on all nodes you might want to connect from.

{% comment %}
{% endcomment %}
You can use this feature to SSH into worker nodes from a manager node without
installing keys directly on the manager.

{% comment %}
{% endcomment %}
If your haven't added your ssh key to the `ssh-agent` you also need to do
this first.

{% comment %}
{% endcomment %}
To see the keys in the agent already, run:

```bash
$ ssh-add -L
```

{% comment %}
{% endcomment %}
If you don't see your key, add it like this.

```bash
$ ssh-add ~/.ssh/your_key
```

{% comment %}
{% endcomment %}
On macOS, the `ssh-agent` forgets this key on restart. But
you can import your SSH key into your Keychain so that your key survives
restarts.

```bash
$ ssh-add -K ~/.ssh/your_key
```

{% comment %}
{% endcomment %}
You can then enable SSH forwarding per-session using the `-A` flag for the ssh
command.

{% comment %}
{% endcomment %}
Connect to the Manager.

```bash
$ ssh -A docker@<manager ip>
```

{% comment %}
{% endcomment %}
To always have it turned on for a given host, you can edit your ssh config file
(`/etc/ssh_config`, `~/.ssh/config`, etc) to add the `ForwardAgent yes` option.

{% comment %}
{% endcomment %}
Example configuration:

```conf
Host manager0
  HostName <manager ip>
  ForwardAgent yes
```

{% comment %}
{% endcomment %}
To SSH in to the manager with the above settings:

```bash
$ ssh docker@manager0
```

{% comment %}
{% endcomment %}
## Run apps

{% comment %}
{% endcomment %}
You can now start creating containers and services.

    $ docker run hello-world

{% comment %}
{% endcomment %}
You can run websites too. Ports exposed with `--publish` are automatically exposed
through the platform load balancer:

    $ docker service create --name nginx --publish published=80,target=80 nginx

{% comment %}
{% endcomment %}
Once up, find the `DefaultDNSTarget` output in either the AWS or Azure portals
to access the site.

{% comment %}
{% endcomment %}
### Execute docker commands in all swarm nodes

{% comment %}
{% endcomment %}
There are cases (such as installing a volume plugin) wherein a docker command may need to be executed in all the nodes across the cluster. You can use the `swarm-exec` tool to achieve that.

{% comment %}
{% endcomment %}
Usage : `swarm-exec {Docker command}`

{% comment %}
{% endcomment %}
The following installs a test plugin in all the nodes in the cluster.

{% comment %}
{% endcomment %}
Example : `swarm-exec docker plugin install --grant-all-permissions
mavenugo/test-docker-netplugin`

{% comment %}
{% endcomment %}
This tool internally makes use of docker global-mode service that runs a task on
each of the nodes in the cluster. This task in turn executes your docker
command. The global-mode service also guarantees that when a new node is added
to the cluster or during upgrades, a new task is executed on that node and hence
the docker command is automatically executed.

{% comment %}
{% endcomment %}
### Docker Stack deployment

{% comment %}
To deploy complex multi-container apps, you can use the `docker stack deploy` command. You can either deploy a docker compose file on your machine over an SSH tunnel, or copy the `docker-compose.yml` file to a manager node via `scp` for example. You can then SSH into the manager node and run `docker stack deploy` with the `--compose-file` or `-c` option. See [docker stack deploy options](/engine/reference/commandline/stack_deploy/#options) for the list of different options. If you have multiple manager nodes, make sure you are logged in to the one with the stack file copy.
{% endcomment %}
To deploy complex multi-container apps, you can use the `docker stack deploy` command. You can either deploy a docker compose file on your machine over an SSH tunnel, or copy the `docker-compose.yml` file to a manager node via `scp` for example. You can then SSH into the manager node and run `docker stack deploy` with the `--compose-file` or `-c` option. See [docker stack deploy options](/engine/reference/commandline/stack_deploy/#options) for the list of different options. If you have multiple manager nodes, make sure you are logged in to the one with the stack file copy.

{% comment %}
{% endcomment %}
For example:

```bash
docker stack deploy --compose-file docker-compose.yml myapp
```

{% comment %}
{% endcomment %}
See [Docker voting app](https://github.com/docker/example-voting-app) for a good sample app to test stack deployments.

{% comment %}
{% endcomment %}
By default, apps deployed with stacks do not have ports publicly exposed. Update port mappings for services, and Docker automatically wires up the underlying platform load balancers:

    docker service update --publish-add published=80,target=80 <example-service>

{% comment %}
{% endcomment %}
### Images in private repos

{% comment %}
{% endcomment %}
To create swarm services using images in private repos, first make sure you're
authenticated and have access to the private repo, then create the service with
the `--with-registry-auth` flag (the example below assumes you're using Docker
Hub):

    docker login
    ...
    docker service create --with-registry-auth user/private-repo
    ...

{% comment %}
{% endcomment %}
This causes the swarm to cache and use the cached registry credentials when creating containers for the service.
