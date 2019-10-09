---
description: Generic driver for machine
keywords: machine, Generic, driver
title: 汎用ドライバー
---

{% comment %}
Create machines using an existing VM/Host with SSH.
{% endcomment %}
Create machines using an existing VM/Host with SSH.

{% comment %}
This is useful if you are using a provider that Machine does not support
directly or if you would like to import an existing host to allow Docker
Machine to manage.
{% endcomment %}
This is useful if you are using a provider that Machine does not support
directly or if you would like to import an existing host to allow Docker
Machine to manage.

{% comment %}
The driver performs a list of tasks on create:
{% endcomment %}
The driver performs a list of tasks on create:

{% comment %}
-   If docker is not running on the host, it is installed automatically.
-   It updates the host packages (`apt-get update`, `yum update`...).
-   It generates certificates to secure the docker daemon.
-   If the host uses systemd, it creates /etc/systemd/system/docker.service.d/10-machine.conf
-   The docker daemon restarts, thus all running containers are stopped.
-   The hostname is updated to fit the machine name.
{% endcomment %}
-   If docker is not running on the host, it is installed automatically.
-   It updates the host packages (`apt-get update`, `yum update`...).
-   It generates certificates to secure the docker daemon.
-   If the host uses systemd, it creates /etc/systemd/system/docker.service.d/10-machine.conf
-   The docker daemon restarts, thus all running containers are stopped.
-   The hostname is updated to fit the machine name.


{% comment %}
### Example
{% endcomment %}
{: #example }
### 例

{% comment %}
To create a machine instance, specify `--driver generic`, the IP address or DNS
name of the host and the path to the SSH private key authorized to connect
to the host.
{% endcomment %}
マシンインスタンスを生成するには `--driver generic` を指定します。
またホストの IP アドレスまたは DNS 名、そしてホスト接続が認証されている SSH 秘密鍵へのパスを指定します。

    $ docker-machine create \
      --driver generic \
      --generic-ip-address=203.0.113.81 \
      --generic-ssh-key ~/.ssh/id_rsa \
      vm

{% comment %}
### Sudo privileges
{% endcomment %}
{: #sudo-privileges }
### Sudo 権限

{% comment %}
The user that is used to SSH into the host can be specified with
`--generic-ssh-user` flag. This user needs password-less sudo
privileges.
If it's not the case, you need to edit the `sudoers` file and configure the user
as a sudoer with `NOPASSWD`. See https://help.ubuntu.com/community/Sudoers.
{% endcomment %}
SSH によってホストに接続するユーザーは `--generic-ssh-user` フラグにより指定します。
このユーザーは、パスワード入力なしに sudo を実行できる必要があります。
そうなっていない場合は `sudoers` ファイルを編集して、そのユーザーを sudoer かつ `NOPASSWD` であるように設定することが必要です。
https://help.ubuntu.com/community/Sudoers を参照してください。

{% comment %}
### Options
{% endcomment %}
{: #options }
### オプション

{% comment %}
-   `--generic-engine-port`: Port to use for Docker Daemon (Note: This flag does not work with boot2docker).
-   `--generic-ip-address`: **required** IP Address of host.
-   `--generic-ssh-key`: Path to the SSH user private key.
-   `--generic-ssh-user`: SSH username used to connect.
-   `--generic-ssh-port`: Port to use for SSH.
{% endcomment %}
-   `--generic-engine-port`: Port to use for Docker Daemon (Note: This flag does not work with boot2docker).
-   `--generic-ip-address`: **required** IP Address of host.
-   `--generic-ssh-key`: Path to the SSH user private key.
-   `--generic-ssh-user`: SSH username used to connect.
-   `--generic-ssh-port`: Port to use for SSH.

{% comment %}
> **Note**: You must use a base operating system supported by Machine.
{% endcomment %}
> **Note**: You must use a base operating system supported by Machine.

{% comment %}
#### Environment variables and default values
{% endcomment %}
#### Environment variables and default values

{% comment %}
| CLI option                 | Environment variable | Default                   |
| -------------------------- | -------------------- | ------------------------- |
| `--generic-engine-port`    | `GENERIC_ENGINE_PORT`| `2376`                    |
| **`--generic-ip-address`** | `GENERIC_IP_ADDRESS` | -                         |
| `--generic-ssh-key`        | `GENERIC_SSH_KEY`    | -                         |
| `--generic-ssh-user`       | `GENERIC_SSH_USER`   | `root`                    |
| `--generic-ssh-port`       | `GENERIC_SSH_PORT`   | `22`                      |
{% endcomment %}
| CLI オプション             | 環境変数             | デフォルト                |
| -------------------------- | -------------------- | ------------------------- |
| `--generic-engine-port`    | `GENERIC_ENGINE_PORT`| `2376`                    |
| **`--generic-ip-address`** | `GENERIC_IP_ADDRESS` | -                         |
| `--generic-ssh-key`        | `GENERIC_SSH_KEY`    | -                         |
| `--generic-ssh-user`       | `GENERIC_SSH_USER`   | `root`                    |
| `--generic-ssh-port`       | `GENERIC_SSH_PORT`   | `22`                      |

{% comment %}
### Systemd settings
{% endcomment %}
{: #systemd-settings }
### Systemd 設定

{% comment %}
{% endcomment %}
For systems that use systemd, if you have an existing configuration defined in
'/etc/systemd/system/docker.service.d/' this  may conflict with the settings created by
docker-machine.  Make sure you don't have any other configuration files in this location
that override the [ExecStart] setting.

{% comment %}
Once you have confirmed any conflicting settings have been removed, run
`sudo systemctl daemon reload` followed by `sudo systemctl restart docker`
{% endcomment %}
Once you have confirmed any conflicting settings have been removed, run
`sudo systemctl daemon reload` followed by `sudo systemctl restart docker`


