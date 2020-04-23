---
description: Getting Started
keywords: windows, edge, tutorial, run, docker, local, machine
redirect_from:
- /winkit/getting-started/
- /winkit/
- /windows/
- /windows/started/
- /docker-for-windows/started/
- /installation/windows/
- /engine/installation/windows/
- /docker-for-windows/index/
title: Get started with Docker for Windows
toc_min: 1
toc_max: 2
---

{% comment %}
{% endcomment %}
Welcome to Docker Desktop!

{% comment %}
{% endcomment %}
The _Docker Desktop for Windows_ section contains information about the Docker Desktop Community Stable release. For information about features available in Edge releases, see the [Edge release notes](edge-release-notes/). For information about Docker Desktop Enterprise (DDE) releases, see [Docker Desktop Enterprise](/desktop/enterprise/).

{% comment %}
{% endcomment %}
Docker is a full development platform to build, run, and share containerized applications. Docker Desktop is the best way to get started with Docker _on Windows_.

{% comment %}
{% endcomment %}
See [Install Docker Desktop](install.md){: target="_blank" class="_"} for download information, system requirements, and installation instructions.

{% comment %}
{% endcomment %}
## Test your installation

{% comment %}
{% endcomment %}
1.  Open a terminal window (Command Prompt or PowerShell, _but not_ PowerShell ISE).

{% comment %}
{% endcomment %}
2.  Run `docker --version` to ensure that you have a supported version of Docker:

    ```shell
    > docker --version

    Docker version 19.03.1
    ```

{% comment %}
{% endcomment %}
3.  Pull the [hello-world image](https://hub.docker.com/r/library/hello-world/) from Docker Hub and run a container:

    ```shell
    > docker run hello-world

    docker : Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    1b930d010525: Pull complete
    Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    ...

    ```

{% comment %}
{% endcomment %}
4.  List the `hello-world` _image_ that was downloaded from Docker Hub:

    ```shell
    > docker image ls
    ```

{% comment %}
{% endcomment %}
5.  List the `hello-world` _container_ (that exited after displaying "Hello from Docker!"):

    ```shell
    > docker container ls --all
    ```

{% comment %}
{% endcomment %}
6.  Explore the Docker help pages by running some help commands:

    ```shell
    > docker --help
    > docker container --help
    > docker container ls --help
    > docker run --help
    ```

{% comment %}
{% endcomment %}
## Explore the application

{% comment %}
{% endcomment %}
In this section, we demonstrate the ease and power of Dockerized applications by
running something more complex, such as an OS and a webserver.

{% comment %}
{% endcomment %}
1. Pull an image of the [Ubuntu OS](https://hub.docker.com/r/_/ubuntu/) and run an interactive terminal inside the spawned container:

    ```shell
    > docker run --interactive --tty ubuntu bash

    docker : Unable to find image 'ubuntu:latest' locally
    latest: Pulling from library/ubuntu
    22e816666fd6: Pull complete
    079b6d2a1e53: Pull complete
    11048ebae908: Pull complete
    c58094023a2e: Pull complete
    Digest: sha256:a7b8b7b33e44b123d7f997bd4d3d0a59fafc63e203d17efedf09ff3f6f516152
    Status: Downloaded newer image for ubuntu:latest
    ```

    {% comment %}
    {% endcomment %}
    > Do not use PowerShell ISE
    >
    > Interactive terminals do not work in PowerShell ISE (but they do in PowerShell). See [docker/for-win/issues/223](https://github.com/docker/for-win/issues/223).

{% comment %}
{% endcomment %}
2. You are in the container. At the root `#` prompt, check the `hostname` of the container:

    ```shell
    root@8aea0acb7423:/# hostname
    8aea0acb7423
    ```

    {% comment %}
    {% endcomment %}
    Notice that the hostname is assigned as the container ID (and is also used in the prompt).

{% comment %}
{% endcomment %}
3. Exit the shell with the `exit` command (which also stops the container):

    ```shell
    root@8aea0acb7423:/# exit
    >
    ```

{% comment %}
{% endcomment %}
4. List containers with the `--all` option (because no containers are running).

    {% comment %}
    {% endcomment %}
    The `hello-world` container (randomly named, `relaxed_sammet`) stopped after displaying its message. The `ubuntu` container (randomly named, `laughing_kowalevski`) stopped when you exited the container.

    ```shell
    > docker container ls --all

    CONTAINER ID    IMAGE          COMMAND     CREATED          STATUS                      PORTS    NAMES
    8aea0acb7423    ubuntu         "bash"      2 minutes ago    Exited (0) 2 minutes ago             laughing_kowalevski
    45f77eb48e78    hello-world    "/hello"    3 minutes ago    Exited (0) 3 minutes ago             relaxed_sammet
    ```

{% comment %}
{% endcomment %}
5. Pull and run a Dockerized [nginx](https://hub.docker.com/_/nginx/) web server that we name, `webserver`:

    ```shell
    > docker run --detach --publish 80:80 --name webserver nginx

    Unable to find image 'nginx:latest' locally
    latest: Pulling from library/nginx

    fdd5d7827f33: Pull complete
    a3ed95caeb02: Pull complete
    716f7a5f3082: Pull complete
    7b10f03a0309: Pull complete
    Digest: sha256:f6a001272d5d324c4c9f3f183e1b69e9e0ff12debeb7a092730d638c33e0de3e
    Status: Downloaded newer image for nginx:latest
    dfe13c68b3b86f01951af617df02be4897184cbf7a8b4d5caf1c3c5bd3fc267f
    ```

{% comment %}
{% endcomment %}
6. Point your web browser at `http://localhost` to display the nginx start page. (You don't need to append `:80` because you specified the default HTTP port in the `docker` command.)

    {% comment %}
    {% endcomment %}
    ![Run nginx edge](images/nginx-homepage.png)

{% comment %}
{% endcomment %}
7. List only your _running_ containers:

    ```shell
    > docker container ls

    CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                 NAMES
    0e788d8e4dfd    nginx    "nginx -g 'daemon of…"    2 minutes ago    Up 2 minutes    0.0.0.0:80->80/tcp    webserver
    ```

{% comment %}
{% endcomment %}
8. Stop the running nginx container by the name we assigned it, `webserver`:

    ```shell
    >  docker container stop webserver
    ```

{% comment %}
{% endcomment %}
9. Remove all three containers by their names -- the latter two names will differ for you:

    ```shell
    > docker container rm webserver laughing_kowalevski relaxed_sammet
    ```

{% comment %}
{% endcomment %}
## Docker Settings dialog

{% comment %}
{% endcomment %}
The **Docker Desktop** menu allows you to configure your Docker settings such as installation, updates, version channels, Docker Hub login,
and more.

{% comment %}
{% endcomment %}
This section explains the configuration options accessible from the **Settings** dialog.

{% comment %}
{% endcomment %}
1. Open the Docker Desktop menu by clicking the Docker icon in the Notifications area (or System tray):

    {% comment %}
    {% endcomment %}
    ![Showing hidden apps in the taskbar](images/whale-icon-systray-hidden.png){:width="250px"}

{% comment %}
{% endcomment %}
2. Select **Settings** to open the Settings dialog:

    {% comment %}
    {% endcomment %}
    ![Docker Desktop popup menu](images/docker-menu-settings.png){:width="300px"}

{% comment %}
{% endcomment %}
### General

{% comment %}
{% endcomment %}
On the **General** tab of the Settings dialog, you can configure when to start and update Docker.

{% comment %}
{% endcomment %}
![Settings](images/settings-general.png){:width="750px"}

{% comment %}
{% endcomment %}
* **Start Docker when you log in** - Automatically start Docker Desktop upon Windows system login.

{% comment %}
{% endcomment %}
* **Automatically check for updates** - By default, Docker Desktop automatically checks for updates and notifies you when an update is available.
Click **OK** to accept and install updates (or cancel to keep the current
version). You can manually update by choosing **Check for Updates** from the
main Docker menu.

{% comment %}
{% endcomment %}
* **Expose daemon on tcp://localhost:2375 without TLS** - Click this option to enable legacy clients to connect to the Docker daemon. You must use this option with caution as exposing the daemon without TLS can result in remote code execution attacks.

{% comment %}
{% endcomment %}
* **Send usage statistics** - By default, Docker Desktop sends diagnostics,
crash reports, and usage data. This information helps Docker improve and
troubleshoot the application. Clear the check box to opt out. Docker may periodically prompt you for more information.

{% comment %}
{% endcomment %}
### Resources

{% comment %}
{% endcomment %}
The **Resources** tab allows you to configure CPU, memory, disk, proxies, network, and other resources.

{% comment %}
{% endcomment %}
![Resources](images/settings-resources.png){:width="750px"}

{% comment %}
{% endcomment %}
#### Advanced

{% comment %}
{% endcomment %}
Use the **Advanced** tab to limit resources available to Docker.

{% comment %}
{% endcomment %}
**CPUs**: By default, Docker Desktop is set to use half the number of processors
available on the host machine. To increase processing power, set this to a
higher number; to decrease, lower the number.

{% comment %}
{% endcomment %}
**Memory**: By default, Docker Desktop is set to use `2` GB runtime memory,
allocated from the total available memory on your machine. To increase the RAM, set this to a higher number. To decrease it, lower the number.

{% comment %}
{% endcomment %}
**Swap**: Configure swap file size as needed. The default is 1 GB.

{% comment %}
{% endcomment %}
**Disk image size**: Specify the size of the disk image.

{% comment %}
{% endcomment %}
**Disk image location**: Specify the location of the Linux volume where containers and images are stored.

{% comment %}
{% endcomment %}
You can also move the disk image to a different location. If you attempt to move a disk image to a location that already has one, you get a prompt asking if you want to use the existing image or replace it.

{% comment %}
{% endcomment %}
#### File sharing

{% comment %}
{% endcomment %}
Use File sharing to allow local drives on Windows to be shared with Linux containers.
This is especially useful for
editing source code in an IDE on the host while running and testing the code in a container.
Note that configuring file sharing is not necessary for Windows containers, only [Linux containers](#switch-between-windows-and-linux-containers).
 If a drive is not shared with a Linux container you may get `file not found` or `cannot start service` errors at runtime. See [Volume mounting requires shared drives for Linux containers](troubleshoot.md#volume-mounting-requires-shared-drives-for-linux-containers).

{% comment %}
{% endcomment %}
**Apply & Restart** makes the drives available to containers using Docker's bind mount (`-v`) feature.

{% comment %}
{% endcomment %}
> Tips on shared drives, permissions, and volume mounts
>
 * Shared drives are designed to allow application code to be edited on the host while being executed in containers. For non-code items
 such as cache directories or databases, the performance will be much better if they are stored in
 the Linux VM, using a [data volume](/engine/tutorials/dockervolumes.md#data-volumes)
 (named volume) or [data container](/engine/tutorials/dockervolumes.md#creating-and-mounting-a-data-volume-container).
>
 * Docker Desktop sets permissions to read/write/execute for users, groups and others [0777 or a+rwx](http://permissions-calculator.org/decode/0777/).
   This is not configurable. See [Permissions errors on data directories for shared volumes](troubleshoot.md#permissions-errors-on-data-directories-for-shared-volumes).
>
 * Windows presents a case-insensitive view of the filesystem to applications while Linux is case-sensitive. On Linux it is possible to create 2 separate files: `test` and `Test`, while on Windows these filenames would actually refer to the same underlying file. This can lead to problems where an app works correctly on a developer Windows machine (where the file contents are shared) but fails when run in Linux in production (where the file contents are distinct). To avoid this, Docker Desktop insists that all shared files are accessed as their original case. Therefore if a file is created called `test`, it must be opened as `test`. Attempts to open `Test` will fail with "No such file or directory". Similarly once a file called `test` is created, attempts to create a second file called `Test` will fail.

{% comment %}
{% endcomment %}
#### Shared drives on demand

{% comment %}
{% endcomment %}
You can share a drive "on demand" the first time a particular mount is requested.

{% comment %}
{% endcomment %}
If you run a Docker command from a shell with a volume mount (as shown in the
example below) or kick off a Compose file that includes volume mounts, you get a
popup asking if you want to share the specified drive.

{% comment %}
{% endcomment %}
You can select to **Share it**, in which case it is added your Docker Desktop [Shared Drives list](index.md#shared-drives) and available to
containers. Alternatively, you can opt not to share it by selecting **Cancel**.

{% comment %}
{% endcomment %}
![Shared drive on demand](images/shared-drive-on-demand.png){:width="600px"}

{% comment %}
{% endcomment %}
#### Proxies

{% comment %}
{% endcomment %}
Docker Desktop lets you configure HTTP/HTTPS Proxy Settings and
automatically propagates these to Docker and to your containers.  For example,
if you set your proxy settings to `http://proxy.example.com`, Docker uses this
proxy when pulling containers.

{% comment %}
{% endcomment %}
When you start a container, your proxy settings propagate into the containers. For example:

```ps
> docker run alpine env

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=b7edf988b2b5
TERM=xterm
HOME=/root
HTTP_PROXY=http://proxy.example.com:3128
http_proxy=http://proxy.example.com:3128
no_proxy=*.local, 169.254/16
```

{% comment %}
{% endcomment %}
In the output above, the `HTTP_PROXY`, `http_proxy`, and `no_proxy` environment
variables are set. When your proxy configuration changes, Docker restarts
automatically to pick up the new settings. If you have containers that you wish
to keep running across restarts, you should consider using
[restart policies](/engine/reference/run/#restart-policies-restart).

{% comment %}
{% endcomment %}
#### Network

{% comment %}
{% endcomment %}
You can configure Docker Desktop networking to work on a virtual private network (VPN). Specify a network address translation (NAT) prefix and subnet mask to enable Internet connectivity.

{% comment %}
{% endcomment %}
**DNS Server**: You can configure the DNS server to use dynamic or static IP addressing.

{% comment %}
{% endcomment %}
> **Note**: Some users reported problems connecting to Docker Hub on Docker Desktop Stable version. This would manifest as an error when trying to run
> `docker` commands that pull images from Docker Hub that are not already
> downloaded, such as a first time run of `docker run hello-world`. If you
> encounter this, reset the DNS server to use the Google DNS fixed address:
> `8.8.8.8`. For more information, see
> [Networking issues](troubleshoot.md#networking-issues) in Troubleshooting.

{% comment %}
{% endcomment %}
Updating these settings requires a reconfiguration and reboot of the Linux VM.

{% comment %}
{% endcomment %}
### Docker Engine

{% comment %}
{% endcomment %}
The Docker Engine page allows you to configure the Docker daemon to determine how your containers run.

{% comment %}
{% endcomment %}
Type a JSON configuration file in the box to configure the daemon settings. For a full list of options, see the Docker Engine
[dockerd commandline reference](/engine/reference/commandline/dockerd.md){:target="_blank"
class="_"}.

{% comment %}
{% endcomment %}
Click **Apply & Restart** to save your settings and restart Docker Desktop.

{% comment %}
{% endcomment %}
### Command Line

{% comment %}
{% endcomment %}
On the Command Line page, you can specify whether or not to enable experimental features.

{% comment %}
{% endcomment %}
On both Docker Desktop Edge and Stable releases, you can toggle the experimental features on and off. If you toggle the experimental features off, Docker Desktop uses the current generally available release of Docker Engine.

{% comment %}
{% endcomment %}
#### Experimental features

{% comment %}
{% endcomment %}
Docker Desktop Edge releases have the experimental version
of Docker Engine enabled by default, described in the [Docker Experimental Features README](https://github.com/docker/cli/blob/master/experimental/README.md) on GitHub.

{% include experimental.md %}

{% comment %}
{% endcomment %}
Run `docker version` to verify whether you have enabled experimental features. Experimental mode
is listed under `Server` data. If `Experimental` is `true`, then Docker is
running in experimental mode, as shown here:

```shell
> docker version

Client: Docker Engine - Community
 Version:           19.03.1
 API version:       1.40
 Go version:        go1.12.5
 Git commit:        74b1e89
 Built:             Thu Jul 25 21:17:08 2019
 OS/Arch:           windows/amd64
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          19.03.1
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.5
  Git commit:       74b1e89
  Built:            Thu Jul 25 21:17:52 2019
  OS/Arch:          linux/amd64
  Experimental:     true
 containerd:
  Version:          v1.2.6
  GitCommit:        894b81a4b802e4eb2a91d1ce216b8817763c29fb
 runc:
  Version:          1.0.0-rc8
  GitCommit:        425e105d5a03fabd737a126ad93d62a9eeede87f
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

{% comment %}
{% endcomment %}
### Kubernetes

{% comment %}
{% endcomment %}
Docker Desktop includes a standalone Kubernetes server that runs on your Windows host, so that you can test deploying your Docker workloads on Kubernetes.

{% comment %}
{% endcomment %}
![Enable Kubernetes](images/settings-kubernetes.png){:width="750px"}

{% comment %}
{% endcomment %}
The Kubernetes client command, `kubectl`, is included and configured to connect
to the local Kubernetes server. If you have `kubectl` already installed and
pointing to some other environment, such as `minikube` or a GKE cluster, be sure
to change context so that `kubectl` is pointing to `docker-desktop`:

```bash
> kubectl config get-contexts
> kubectl config use-context docker-desktop
```

{% comment %}
{% endcomment %}
 To enable Kubernetes support and install a standalone instance of Kubernetes
  running as a Docker container, select **Enable Kubernetes**.

{% comment %}
{% endcomment %}
To set Kubernetes as the
  [default orchestrator](/docker-for-mac/kubernetes/#override-the-default-orchestrator), select **Deploy Docker Stacks to Kubernetes by default**.

{% comment %}
{% endcomment %}
By default, Kubernetes containers are hidden from commands like `docker
service ls`, because managing them manually is not supported. To make them
visible, select **Show system containers (advanced)**. Most users do not need this option.

{% comment %}
{% endcomment %}
Click **Apply & Restart** to save the settings. This instantiates images required to run the Kubernetes server as containers, and installs the `kubectl.exe` command in the path.

{% comment %}
{% endcomment %}
- When Kubernetes is enabled and running, an additional status bar item displays
at the bottom right of the Docker Desktop Settings dialog. The status of Kubernetes shows in the Docker menu and the context points to
  `docker-desktop`.

{% comment %}
{% endcomment %}
- To disable Kubernetes support at any time, clear the **Enable Kubernetes** check box.
  The Kubernetes containers are stopped and removed, and the
  `/usr/local/bin/kubectl` command is removed.

{% comment %}
{% endcomment %}
- To delete all stacks and Kubernetes resources, select **Reset Kubernetes Cluster**.

{% comment %}
{% endcomment %}
- If you installed `kubectl` by another method, and
experience conflicts, remove it.

  {% comment %}
  {% endcomment %}
  For more information on using the Kubernetes integration with Docker Desktop, see [Deploy on Kubernetes](kubernetes.md).

{% comment %}
{% endcomment %}
### Reset

{% comment %}
{% endcomment %}
The **Restart Docker Desktop** and **Reset to factory defaults** options are now available on the **Troubleshoot** menu. For information, see [Logs and Troubleshooting](troubleshoot.md).

{% comment %}
{% endcomment %}
### Troubleshoot

{% comment %}
{% endcomment %}
Visit our [Logs and Troubleshooting](troubleshoot.md) guide for more details.

{% comment %}
{% endcomment %}
Log on to our [Docker Desktop for Windows forum](https://forums.docker.com/c/docker-for-windows) to get help from the community, review current user topics, or join a discussion.

{% comment %}
{% endcomment %}
Log on to [Docker Desktop for Windows issues on GitHub](https://github.com/docker/for-win/issues) to report bugs or problems and review community reported issues.

{% comment %}
{% endcomment %}
For information about providing feedback on the documentation or update it yourself, see [Contribute to documentation](/opensource/).

{% comment %}
{% endcomment %}
## Switch between Windows and Linux containers

{% comment %}
{% endcomment %}
From the Docker Desktop menu, you can toggle which daemon (Linux or Windows)
the Docker CLI talks to. Select **Switch to Windows containers** to use Windows
containers, or select **Switch to Linux containers** to use Linux containers
(the default).

{% comment %}
{% endcomment %}
For more information on Windows containers, refer to the following documentation:

{% comment %}
{% endcomment %}
- Microsoft documentation on [Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/index).

{% comment %}
{% endcomment %}
- [Build and Run Your First Windows Server Container (Blog Post)](https://blog.docker.com/2016/09/build-your-first-docker-windows-server-container/)
  gives a quick tour of how to build and run native Docker Windows containers on Windows 10 and Windows Server 2016 evaluation releases.

{% comment %}
{% endcomment %}
- [Getting Started with Windows Containers (Lab)](https://github.com/docker/labs/blob/master/windows/windows-containers/README.md)
  shows you how to use the [MusicStore](https://github.com/aspnet/MusicStore/blob/dev/README.md)
  application with Windows containers. The MusicStore is a standard .NET application and,
  [forked here to use containers](https://github.com/friism/MusicStore), is a good example of a multi-container application.

{% comment %}
{% endcomment %}
- To understand how to connect to Windows containers from the local host, see
  [Limitations of Windows containers for `localhost` and published ports](troubleshoot.md#limitations-of-windows-containers-for-localhost-and-published-ports)

{% comment %}
{% endcomment %}
> Settings dialog changes with Windows containers
>
> When you switch to Windows containers, the Settings dialog only shows those tabs that are active and apply to your Windows containers:
>

  {% comment %}
  {% endcomment %}
  * [General](#general)
  * [Proxies](#proxies)
  * [Daemon](#docker-daemon)
  * [Reset](#reset)

{% comment %}
{% endcomment %}
If you set proxies or daemon configuration in Windows containers mode, these
apply only on Windows containers. If you switch back to Linux containers,
proxies and daemon configurations return to what you had set for Linux
containers. Your Windows container settings are retained and become available
again when you switch back.

{% comment %}
{% endcomment %}
## Dashboard

{% comment %}
{% endcomment %}
The Docker Desktop Dashboard enables you to interact with containers and applications and manage the lifecycle of your applications directly from your machine. The Dashboard UI shows all running, stopped, and started containers with their state. It provides an intuitive interface to perform common actions to inspect and manage containers and Docker Compose applications. For more information, see [Docker Desktop Dashboard](dashboard.md).

{% comment %}
{% endcomment %}
## Docker Hub

{% comment %}
{% endcomment %}
Select **Sign in /Create Docker ID** from the Docker Desktop menu to access your [Docker Hub](https://hub.docker.com/){: target="_blank" class="_" } account. Once logged in, you can access your Docker Hub repositories directly from the Docker Desktop menu.

{% comment %}
{% endcomment %}
For more information, refer to the following [Docker Hub topics](/docker-hub/index.md){: target="_blank" class="_" }:

{% comment %}
{% endcomment %}
* [Organizations and Teams in Docker Hub](/docker-hub/orgs.md){: target="_blank" class="_" }
* [Builds and Images](/docker-cloud/builds/index.md){: target="_blank" class="_" }

{% comment %}
{% endcomment %}
### Two-factor authentication

{% comment %}
{% endcomment %}
Docker Desktop enables you to sign into Docker Hub using two-factor authentication. Two-factor authentication provides an extra layer of security when accessing your Docker Hub account.

{% comment %}
{% endcomment %}
You must enable two-factor authentication in Docker Hub before signing into your Docker Hub account through Docker Desktop. For instructions, see [Enable two-factor authentication for Docker Hub](/docker-hub/2fa/).

{% comment %}
{% endcomment %}
After you have enabled two-factor authentication:

{% comment %}
{% endcomment %}
1. Go to the Docker Desktop menu and then select **Sign in / Create Docker ID**.

{% comment %}
{% endcomment %}
2. Enter your Docker ID and password and click **Sign in**.

{% comment %}
{% endcomment %}
3. After you have successfully signed in, Docker Desktop prompts you to enter the authentication code. Enter the six-digit code from your phone and then click **Verify**.

{% comment %}
{% endcomment %}
![Docker Desktop 2FA](images/desktop-win-2fa.png){:width="500px"}

{% comment %}
{% endcomment %}
After you have successfully authenticated, you can access your organizations and repositories directly from the Docker Desktop menu.

{% comment %}
{% endcomment %}
## Adding TLS certificates

{% comment %}
{% endcomment %}
You can add trusted **Certificate Authorities (CAs)** to your Docker daemon to verify registry server
certificates, and **client certificates**, to authenticate to registries. For more information, see [How do I add custom CA certificates?](faqs.md#how-do-i-add-custom-ca-certificates)
and [How do I add client certificates?](faqs.md#how-do-i-add-client-certificates)
in the FAQs.

{% comment %}
{% endcomment %}
### How do I add custom CA certificates?

{% comment %}
{% endcomment %}
Docker Desktop supports all trusted Certificate Authorities (CAs) (root or
intermediate). Docker recognizes certs stored under Trust Root
Certification Authorities or Intermediate Certification Authorities.

{% comment %}
{% endcomment %}
Docker Desktop creates a certificate bundle of all user-trusted CAs based on
the Windows certificate store, and appends it to Moby trusted certificates. Therefore, if an enterprise SSL certificate is trusted by the user on the host, it is trusted by Docker Desktop.

{% comment %}
{% endcomment %}
To learn more about how to install a CA root certificate for the registry, see
[Verify repository client with certificates](/engine/security/certificates)
in the Docker Engine topics.

{% comment %}
{% endcomment %}
### How do I add client certificates?

{% comment %}
{% endcomment %}
You can add your client certificates
in `~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` and
`~/.docker/certs.d/<MyRegistry>:<Port>/client.key`. You do not need to push your certificates with `git` commands.

{% comment %}
{% endcomment %}
When the Docker Desktop application starts, it copies the
`~/.docker/certs.d` folder on your Windows system to the `/etc/docker/certs.d`
directory on Moby (the Docker Desktop virtual machine running on Hyper-V).

{% comment %}
{% endcomment %}
You need to restart Docker Desktop after making any changes to the keychain
or to the `~/.docker/certs.d` directory in order for the changes to take effect.

{% comment %}
{% endcomment %}
The registry cannot be listed as an _insecure registry_ (see
[Docker Daemon](/docker-for-windows#daemon)). Docker Desktop ignores
certificates listed under insecure registries, and does not send client
certificates. Commands like `docker run` that attempt to pull from the registry
produce error messages on the command line, as well as on the registry.

{% comment %}
{% endcomment %}
To learn more about how to set the client TLS certificate for verification, see
[Verify repository client with certificates](/engine/security/certificates)
in the Docker Engine topics.

{% comment %}
{% endcomment %}
## Where to go next

{% comment %}
{% endcomment %}
* Try out the walkthrough at [Get Started](/get-started/){: target="_blank" class="_"}.

{% comment %}
{% endcomment %}
* Dig in deeper with [Docker Labs](https://github.com/docker/labs/) example walkthroughs and source code.

{% comment %}
{% endcomment %}
* Refer to the [Docker CLI Reference Guide](/engine/api.md){: target="_blank" class="_"}.
