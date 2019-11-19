---
advisory: machine
description: Using Docker Machine to provision hosts on DigitalOcean
keywords: docker, machine, cloud, digitalocean
title: DigitalOcean 利用例
---

{% comment %}
Follow along with this example to create a Dockerized [DigitalOcean](https://digitalocean.com) Droplet (cloud host).
{% endcomment %}
Follow along with this example to create a Dockerized [DigitalOcean](https://digitalocean.com) Droplet (cloud host).

{% comment %}
### Step 1. Create a DigitalOcean account
{% endcomment %}
{: #step-1-create-a-digitalocean-account }
### ステップ 1. DigitalOcean アカウントの生成

{% comment %}
If you have not done so already, go to [DigitalOcean](https://digitalocean.com), create an account, and log in.
{% endcomment %}
If you have not done so already, go to [DigitalOcean](https://digitalocean.com), create an account, and log in.

{% comment %}
### Step 2. Generate a personal access token
{% endcomment %}
{: #step-2-generate-a-personal-access-token }
### ステップ 2. Generate a personal access token

{% comment %}
To generate your access token:
{% endcomment %}
アクセストークンは以下により生成します。

{% comment %}
1.  Go to the DigitalOcean administrator console and click **API** in the header.
{% endcomment %}
1.  DigitalOcean の管理コンソールにアクセスし、ヘッダー部分の **API** をクリックします。

    {% comment %}
    ![Click API in DigitalOcean console](../img/ocean_click_api.png)
    {% endcomment %}
    ![DigitalOcean コンソールでの API クリック](../img/ocean_click_api.png)

{% comment %}
2.  Click **Generate new token** to get to the token generator.
{% endcomment %}
2.  **Generate new token** をクリックして、トークン生成画面にアクセスします。

    {% comment %}
    ![Generate token](../img/ocean_gen_token.png)
    {% endcomment %}
    ![トークン生成](../img/ocean_gen_token.png)

{% comment %}
3.  Give the token a descriptive name, make sure the **Write (Optional)** checkbox is checked, and click **Generate Token**.
{% endcomment %}
3.  Give the token a descriptive name, make sure the **Write (Optional)** checkbox is checked, and click **Generate Token**.

    {% comment %}
    ![Name and generate token](../img/ocean_token_create.png)
    {% endcomment %}
    ![Name and generate token](../img/ocean_token_create.png)

{% comment %}
4.  Grab (copy to clipboard) the generated big long hex string and store it somewhere safe.
{% endcomment %}
4.  Grab (copy to clipboard) the generated big long hex string and store it somewhere safe.

    {% comment %}
    ![Copy and save personal access token](../img/ocean_save_token.png)
    {% endcomment %}
    ![Copy and save personal access token](../img/ocean_save_token.png)

    {% comment %}
    This is the personal access token used in the next step to create your cloud server.
    {% endcomment %}
    This is the personal access token used in the next step to create your cloud server.

{% comment %}
### Step 3. Use Machine to create the Droplet
{% endcomment %}
{: #step-3-use-machine-to-create-the-droplet }
### ステップ 3. Machine を利用した Droplet の生成


{% comment %}
1.  Run `docker-machine create` with the `digitalocean` driver and pass your key to the `--digitalocean-access-token` flag, along with a name for the new cloud server.
{% endcomment %}
1.  Run `docker-machine create` with the `digitalocean` driver and pass your key to the `--digitalocean-access-token` flag, along with a name for the new cloud server.

    {% comment %}
    For this example, the new Droplet is called `docker-sandbox`:
    {% endcomment %}
    ここでの例では、新たな Droplet の名称を `docker-sandbox` とします。

    ```none
    $ docker-machine create --driver digitalocean --digitalocean-access-token xxxxx docker-sandbox
    Running pre-create checks...
    Creating machine...
    (docker-sandbox) OUT | Creating SSH key...
    (docker-sandbox) OUT | Creating Digital Ocean droplet...
    (docker-sandbox) OUT | Waiting for IP address to be assigned to the Droplet...
    Waiting for machine to be running, this may take a few minutes...
    Machine is running, waiting for SSH to be available...
    Detecting operating system of created instance...
    Detecting the provisioner...
    Provisioning created instance...
    Copying certs to the local machine directory...
    Copying certs to the remote machine...
    Setting Docker configuration on the remote daemon...
    To see how to connect Docker to this machine, run: docker-machine env docker-sandbox
    ```

      {% comment %}
      When the Droplet is created, Docker generates a unique SSH key and stores it on your local system in `~/.docker/machines`. Initially, this is used to provision the host. Later, it's used under the hood to access the Droplet directly with the `docker-machine ssh` command. Docker Engine is installed on the cloud server and the daemon is configured to accept remote connections over TCP using TLS for authentication.
      {% endcomment %}
      When the Droplet is created, Docker generates a unique SSH key and stores it on your local system in `~/.docker/machines`. Initially, this is used to provision the host. Later, it's used under the hood to access the Droplet directly with the `docker-machine ssh` command. Docker Engine is installed on the cloud server and the daemon is configured to accept remote connections over TCP using TLS for authentication.

{% comment %}
2. Go to the DigitalOcean console to view the new Droplet.
{% endcomment %}
2. DigitalOcean コンソールにアクセスし、新たな Droplet を確認します。

    {% comment %}
    ![Droplet in DigitalOcean created with Machine](../img/ocean_droplet.png)
    {% endcomment %}
    ![Droplet in DigitalOcean created with Machine](../img/ocean_droplet.png)

{% comment %}
3. At the command terminal, run `docker-machine ls`.
{% endcomment %}
3. コマンドターミナルから `docker-machine ls` を実行します。

        $ docker-machine ls
        NAME             ACTIVE   DRIVER         STATE     URL                         SWARM
        default          -        virtualbox     Running   tcp://192.168.99.100:2376
        docker-sandbox   *        digitalocean   Running   tcp://45.55.139.48:2376

    {% comment %}
    The new `docker-sandbox` machine is running, and it is the active host as
    indicated by the asterisk (\*). When you create a new machine, your command
    shell automatically connects to it. If for some reason your new machine is
    not the active host, run `docker-machine env docker-sandbox`, followed by
    `eval $(docker-machine env docker-sandbox)` to connect to it.
    {% endcomment %}
    The new `docker-sandbox` machine is running, and it is the active host as
    indicated by the asterisk (\*). When you create a new machine, your command
    shell automatically connects to it. If for some reason your new machine is
    not the active host, run `docker-machine env docker-sandbox`, followed by
    `eval $(docker-machine env docker-sandbox)` to connect to it.

{% comment %}
### Step 4. Run Docker commands on the Droplet
{% endcomment %}
{: #step-4-run-docker-commands-on-the-droplet }
### ステップ 4. Run Docker commands on the Droplet

{% comment %}
1. Run some `docker-machine` commands to inspect the remote host. For example, `docker-machine ip <machine>` gets the host IP address and `docker-machine inspect <machine>` lists all the details.
{% endcomment %}
1. Run some `docker-machine` commands to inspect the remote host. For example, `docker-machine ip <machine>` gets the host IP address and `docker-machine inspect <machine>` lists all the details.

        $ docker-machine ip docker-sandbox
        104.131.43.236

        $ docker-machine inspect docker-sandbox
        {
            "ConfigVersion": 3,
            "Driver": {
            "IPAddress": "104.131.43.236",
            "MachineName": "docker-sandbox",
            "SSHUser": "root",
            "SSHPort": 22,
            "SSHKeyPath": "/Users/samanthastevens/.docker/machine/machines/docker-sandbox/id_rsa",
            "StorePath": "/Users/samanthastevens/.docker/machine",
            "SwarmMaster": false,
            "SwarmHost": "tcp://0.0.0.0:3376",
            "SwarmDiscovery": "",
            ...

{% comment %}
2. Verify Docker Engine is installed correctly by running `docker` commands.
{% endcomment %}
2. Verify Docker Engine is installed correctly by running `docker` commands.

    {% comment %}
    Start with something basic like `docker run hello-world`, or for a more interesting test, run a Dockerized webserver on your new remote machine.
    {% endcomment %}
    Start with something basic like `docker run hello-world`, or for a more interesting test, run a Dockerized webserver on your new remote machine.

    {% comment %}
    In this example, the `-p` option is used to expose port 80 from the `nginx` container and make it accessible on port `8000` of the `docker-sandbox` host.
    {% endcomment %}
    In this example, the `-p` option is used to expose port 80 from the `nginx` container and make it accessible on port `8000` of the `docker-sandbox` host.

        $ docker run -d -p 8000:80 --name webserver kitematic/hello-world-nginx
        Unable to find image 'kitematic/hello-world-nginx:latest' locally
        latest: Pulling from kitematic/hello-world-nginx
        a285d7f063ea: Pull complete
        2d7baf27389b: Pull complete
        ...
        Digest: sha256:ec0ca6dcb034916784c988b4f2432716e2e92b995ac606e080c7a54b52b87066
        Status: Downloaded newer image for kitematic/hello-world-nginx:latest
        942dfb4a0eaae75bf26c9785ade4ff47ceb2ec2a152be82b9d7960e8b5777e65

    {% comment %}
    In a web browser, go to `http://<host_ip>:8000` to bring up the webserver home page. You got the `<host_ip>` from the output of the `docker-machine ip <machine>` command you ran in a previous step. Use the port you exposed in the `docker run` command.
    {% endcomment %}
    In a web browser, go to `http://<host_ip>:8000` to bring up the webserver home page. You got the `<host_ip>` from the output of the `docker-machine ip <machine>` command you ran in a previous step. Use the port you exposed in the `docker run` command.

    {% comment %}
    ![nginx webserver](../img/nginx-webserver.png)
    {% endcomment %}
    ![nginx ウェブサーバー](../img/nginx-webserver.png)

{% comment %}
### Step 5. Use Machine to remove the Droplet
{% endcomment %}
{: #step-5-use-machine-to-remove-the-droplet }
### ステップ 5. Machine を利用した Droplet の削除

{% comment %}
To remove a host and all of its containers and images, first stop the machine, then use `docker-machine rm`:
{% endcomment %}
To remove a host and all of its containers and images, first stop the machine, then use `docker-machine rm`:

    $ docker-machine stop docker-sandbox
    $ docker-machine rm docker-sandbox
    Do you really want to remove "docker-sandbox"? (y/n): y
    Successfully removed docker-sandbox

    $ docker-machine ls
    NAME      ACTIVE   DRIVER       STATE     URL                         SWARM
    default   *        virtualbox   Running   tcp:////xxx.xxx.xx.xxx:xxxx

{% comment %}
If you monitor the DigitalOcean console while you run these commands, notice
that it updates first to reflect that the Droplet was stopped, and then removed.
{% endcomment %}
If you monitor the DigitalOcean console while you run these commands, notice
that it updates first to reflect that the Droplet was stopped, and then removed.

{% comment %}
If you create a host with Docker Machine, but remove it through the cloud
provider console, Machine loses track of the server status. Use the
`docker-machine rm` command for hosts you create with `docker-machine create`.
{% endcomment %}
If you create a host with Docker Machine, but remove it through the cloud
provider console, Machine loses track of the server status. Use the
`docker-machine rm` command for hosts you create with `docker-machine create`.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
-   [Understand Machine concepts](../concepts.md)
-   [Docker Machine driver reference](../drivers/index.md)
-   [Docker Machine subcommand reference](../reference/index.md)
-   [Create containers for your Docker Machine](../../get-started/part2.md)
-   [Provision a Docker Swarm cluster with Docker Machine](/swarm/provision-with-machine.md)
{% endcomment %}
-   [Understand Machine concepts](../concepts.md)
-   [Docker Machine driver reference](../drivers/index.md)
-   [Docker Machine subcommand reference](../reference/index.md)
-   [Create containers for your Docker Machine](../../get-started/part2.md)
-   [Provision a Docker Swarm cluster with Docker Machine](/swarm/provision-with-machine.md)
