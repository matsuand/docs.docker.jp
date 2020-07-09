---
description: root ユーザー以外によって Docker デーモンを起動します（rootless モード）。
keywords: security, namespaces, rootless
title: root ユーザー以外による Docker デーモン起動（rootless モード）
---

{% comment %}
Rootless mode allows running the Docker daemon and containers as a non-root
user to mitigate potential vulnerabilities in the daemon and
the container runtime.
{% endcomment %}
rootless モードとは、root 以外のユーザーによって Docker デーモンやコンテナーを起動するものであり、デーモンや起動コンテナーにおける潜在的なぜい弱性を軽減します。

{% comment %}
Rootless mode does not require root privileges even during the installation of
the Docker daemon, as long as the [prerequisites](#prerequisites) are met.
{% endcomment %}
rootless モードは root 権限を必要としません。
しかも [前提条件](#prerequisites) を満たしていれば、Docker デーモンのインストール時も必要としません。

{% comment %}
Rootless mode was introduced in Docker Engine v19.03.
{% endcomment %}
rootless モードは Docker Engine v19.03 から導入されました。

{% comment %}
> **Note**
>
> Rootless mode is an experimental feature and has some limitations. For details,
> see [Known limitations](#known-limitations).
{% endcomment %}
> **メモ**
>
> rootless モードは試験的な機能であるため、制約がいくつかあります。
> 詳しくは [既知の問題](#known-limitations) を参照してください。

{% comment %}
## How it works
{% endcomment %}
{: #how-it-works }
## どのように動作するか

{% comment %}
Rootless mode executes the Docker daemon and containers inside a user namespace.
This is very similar to [`userns-remap` mode](userns-remap.md), except that
with `userns-remap` mode, the daemon itself is running with root privileges,
whereas in rootless mode, both the daemon and the container are running without
root privileges.
{% endcomment %}
rootless モードは、Docker デーモンやコンテナーをユーザー名前空間の内部で実行します。
これは [`userns-remap` モード](userns-remap.md) に非常によく似ています。
ただし `userns-remap` モードにおいては、デーモンだけは root 権限で起動します。
rootless モードの場合は、デーモンとコンテナーがそれぞれ root 権限なしに動作します。

{% comment %}
Rootless mode does not use binaries with `SETUID` bits or file capabilities,
except `newuidmap` and `newgidmap`, which are needed to allow multiple
UIDs/GIDs to be used in the user namespace.
{% endcomment %}
rootless モードは、実行モジュールの `SETUID` ビットやファイルケーパビリティーは利用しません。
ただし `newuidmap` と `newgidmap` は利用します。
これらはユーザー名前空間内において、複数の UID/GID を利用するために必要となるものです。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
-  You must install `newuidmap` and `newgidmap` on the host. These commands
  are provided by the `uidmap` package on most distros.
{% endcomment %}
-  ホスト上に `newuidmap` と `newgidmap` をインストールすることが必要です。
   このコマンドは、たいていのディストリビューションにおいて `uidmap` パッケージとして提供されています。

{% comment %}
- `/etc/subuid` and `/etc/subgid` should contain at least 65,536 subordinate
  UIDs/GIDs for the user. In the following example, the user `testuser` has
  65,536 subordinate UIDs/GIDs (231072-296607).
{% endcomment %}
- `/etc/subuid` と `/etc/subgid` では、ユーザーに対して最低でも 65,536 個のサブ UID/サブ GIDを許容しておくことが必要です。
  以下の例において `testuser` ユーザーには 65,536 個のサブ UID/サブ GID (231072-296607) が与えられています。

```console
$ id -u
1001
$ whoami
testuser
$ grep ^$(whoami): /etc/subuid
testuser:231072:65536
$ grep ^$(whoami): /etc/subgid
testuser:231072:65536
```

{% comment %}
### Distribution-specific hint
{% endcomment %}
{: #distribution-specific-hint }
### ディストリビューション固有の情報

{% comment %}
> Note: We recommend that you use the Ubuntu kernel.
{% endcomment %}
> メモ: ここでは Ubuntu カーネルの利用をお勧めします。

#### Ubuntu

{% comment %}
- No preparation is needed.
{% endcomment %}
- 事前準備するものは何もありません。

{% comment %}
- `overlay2` storage driver  is enabled by default
  ([Ubuntu-specific kernel patch](https://kernel.ubuntu.com/git/ubuntu/ubuntu-bionic.git/commit/fs/overlayfs?id=3b7da90f28fe1ed4b79ef2d994c81efbc58f1144)).
{% endcomment %}
- デフォルトで `overlay2` ストレージドライバーが有効になっています。
  ([Ubuntu 固有のカーネルパッチ](https://kernel.ubuntu.com/git/ubuntu/ubuntu-bionic.git/commit/fs/overlayfs?id=3b7da90f28fe1ed4b79ef2d994c81efbc58f1144))

{% comment %}
- Known to work on Ubuntu 16.04, 18.04, and 20.04.
{% endcomment %}
- Ubuntu 16.04、18.04、20.04 において動作します。

#### Debian GNU/Linux
{% comment %}
- Add `kernel.unprivileged_userns_clone=1` to `/etc/sysctl.conf` (or
  `/etc/sysctl.d`) and run `sudo sysctl --system`.
{% endcomment %}
- `/etc/sysctl.conf`（または `/etc/sysctl.d`）に `kernel.unprivileged_userns_clone=1` を追加して、`sudo sysctl --system` を実行してください。

{% comment %}
- To use the `overlay2` storage driver (recommended), run
  `sudo modprobe overlay permit_mounts_in_userns=1`
   ([Debian-specific kernel patch, introduced in Debian 10](https://salsa.debian.org/kernel-team/linux/blob/283390e7feb21b47779b48e0c8eb0cc409d2c815/debian/patches/debian/overlayfs-permit-mounts-in-userns.patch)).
   Add the configuration to `/etc/modprobe.d` for persistence.
{% endcomment %}
- `overlay2` ストレージドライバー（推奨）を利用するには、`sudo modprobe overlay permit_mounts_in_userns=1` を実行してください。
   ([Debian 固有のカーネルパッチ、Debian 10 において導入](https://salsa.debian.org/kernel-team/linux/blob/283390e7feb21b47779b48e0c8eb0cc409d2c815/debian/patches/debian/overlayfs-permit-mounts-in-userns.patch))
   そして `/etc/modprobe.d` に設定を追加してください。

{% comment %}
- Known to work on Debian 9 and 10.
  `overlay2` is only supported since Debian 10 and needs `modprobe`
  configuration described above.
{% endcomment %}
- Debian 9 と 10 において動作します。
  `overlay2` がサポートされるのは Debian 10 からであり、上に示した `modprobe` の設定が必要です。

#### Arch Linux

{% comment %}
- Add `kernel.unprivileged_userns_clone=1` to `/etc/sysctl.conf` (or
  `/etc/sysctl.d`) and run `sudo sysctl --system`
{% endcomment %}
- `/etc/sysctl.conf`（または `/etc/sysctl.d`）に `kernel.unprivileged_userns_clone=1` を追加して、`sudo sysctl --system` を実行してください。

#### openSUSE

{% comment %}
- `sudo modprobe ip_tables iptable_mangle iptable_nat iptable_filter` is required.
  This might be required on other distros as well depending on the configuration.
{% endcomment %}
- `sudo modprobe ip_tables iptable_mangle iptable_nat iptable_filter` を実行することが必要です。
  設定方法によっては他のディストリビューションにおいても必要なことかもしれません。

{% comment %}
- Known to work on openSUSE 15.
{% endcomment %}
- openSUSE 15 において動作します。

{% comment %}
#### Fedora 31 and later
{% endcomment %}
{: #fedora-31-and-later }
#### Fedora 31 およびそれ以降

{% comment %}
- Fedora 31 uses cgroup v2 by default, which is not yet supported by the containerd runtime.
  Run `sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"`
  to use cgroup v1.
- You might need `sudo dnf install -y iptables`.
{% endcomment %}
- Fedora 31 はデフォルトで cgroup v2 を利用しています。
  これは今のところ、コンテナー起動においてサポートされていません。
  `sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"` を実行して cgroup v1 を利用するようにしてください。
- `sudo dnf install -y iptables` が必要かもしれません。

#### CentOS 8

{% comment %}
- You might need `sudo dnf install -y iptables`.
{% endcomment %}
- `sudo dnf install -y iptables` が必要かもしれません。

#### CentOS 7

{% comment %}
- Add `user.max_user_namespaces=28633` to `/etc/sysctl.conf` (or
  `/etc/sysctl.d`) and run `sudo sysctl --system`.
{% endcomment %}
- `/etc/sysctl.conf`（または `/etc/sysctl.d`）に `user.max_user_namespaces=28633` を追加して、`sudo sysctl --system` を実行してください。

{% comment %}
- `systemctl --user` does not work by default.
  Run the daemon directly without systemd:
  `dockerd-rootless.sh --experimental --storage-driver vfs`
{% endcomment %}
- デフォルトでは `systemctl --user` は動作しません。
  デーモンは sysmted を使わずに直接起動してください。
  `dockerd-rootless.sh --experimental --storage-driver vfs` などです。

{% comment %}
- Known to work on CentOS 7.7. Older releases require additional configuration
  steps.
{% endcomment %}
- CentOS 7.7 において動作します。
  これより古いリリースにおいては、追加の設定手順が必要になります。

{% comment %}
- CentOS 7.6 and older releases require [COPR package `vbatts/shadow-utils-newxidmap`](https://copr.fedorainfracloud.org/coprs/vbatts/shadow-utils-newxidmap/) to be installed.
{% endcomment %}
- CentOS 7.6 やこれ以前のリリースにおいては、[COPR パッケージの `vbatts/shadow-utils-newxidmap`](https://copr.fedorainfracloud.org/coprs/vbatts/shadow-utils-newxidmap/) をインストールしていることが必要です。

{% comment %}
- CentOS 7.5 and older releases require running
  `sudo grubby --update-kernel=ALL --args="user_namespace.enable=1"` and a reboot following this.
{% endcomment %}
- CentOS 7.5 やこれ以前のリリースにおいては `sudo grubby --update-kernel=ALL --args="user_namespace.enable=1"` の実行と、その後にシステムを再起動することが必要です。

{% comment %}
## Known limitations
{% endcomment %}
{: #known-limitations }
## 既知の制約

{% comment %}
- Only `vfs` graphdriver is supported. However, on Ubuntu and Debian 10,
  `overlay2` and `overlay` are also supported.
- Following features are not supported:
  - Cgroups (including `docker top`, which depends on the cgroups)
  - AppArmor
  - Checkpoint
  - Overlay network
  - Exposing SCTP ports
- To use the `ping` command, see [Routing ping packets](#routing-ping-packets).
- To expose privileged TCP/UDP ports (< 1024), see [Exposing privileged ports](#exposing-privileged-ports).
- `IPAddress` shown in `docker inspect` and is namespaced inside RootlessKit's network namespace.
  This means the IP address is not reachable from the host without `nsenter`-ing into the network namespace.
- Host network (`docker run --net=host`) is also namespaced inside RootlessKit.
{% endcomment %}
- `vfs` グラフドライバーだけがサポートされます。
  ただし Ubuntu と Debian 10 の場合は `overlay2` と `overlay` もサポートされます。
- 以下の機能はサポートされません。
  - cgroups （cgroups に依存している `docker top`も同様。）
  - AppArmor
  - Checkpoint
  - Overlay ネットワーク
  - SCTP ポートの公開
- `ping` コマンドを利用するには [Routing ping packets](#routing-ping-packets) を参照してください。
- To expose privileged TCP/UDP ports (< 1024), see [Exposing privileged ports](#exposing-privileged-ports).
- `IPAddress` shown in `docker inspect` and is namespaced inside RootlessKit's network namespace.
  This means the IP address is not reachable from the host without `nsenter`-ing into the network namespace.
- Host network (`docker run --net=host`) is also namespaced inside RootlessKit.

{% comment %}
## Install
{% endcomment %}
{: #install }
## インストール

{% comment %}
The installation script is available at [https://get.docker.com/rootless](https://get.docker.com/rootless){: target="_blank" class="_" }.
{% endcomment %}
[https://get.docker.com/rootless](https://get.docker.com/rootless){: target="_blank" class="_" } から、インストールスクリプトを入手することができます。

```console
$ curl -fsSL https://get.docker.com/rootless | sh
```

{% comment %}
Make sure to run the script as a non-root user.
To install Rootless Docker as the root user, see the [Manual installation](#manual-installation) steps.
{% endcomment %}
Make sure to run the script as a non-root user.
To install Rootless Docker as the root user, see the [Manual installation](#manual-installation) steps.

{% comment %}
{% endcomment %}
The script shows environment variables that are required:

```console
$ curl -fsSL https://get.docker.com/rootless | sh
...
# Docker binaries are installed in /home/testuser/bin
# WARN: dockerd is not in your current PATH or pointing to /home/testuser/bin/dockerd
# Make sure the following environment variables are set (or add them to ~/.bashrc):

export PATH=/home/testuser/bin:$PATH
export PATH=$PATH:/sbin
export DOCKER_HOST=unix:///run/user/1001/docker.sock

#
# To control docker service run:
# systemctl --user (start|stop|restart) docker
#
```

{% comment %}
### Manual installation
{% endcomment %}
{: #manual-installation }
### 手動インストール

{% comment %}
{% endcomment %}
To install the binaries manually without using the installer, extract
`docker-rootless-extras-<version>.tar.gz` along with `docker-<version>.tar.gz`
from [https://download.docker.com/linux/static/stable/x86\_64/](https://download.docker.com/linux/static/stable/x86_64/){: target="_blank" class="_" }

{% comment %}
{% endcomment %}
If you already have the Docker daemon running as the root, you only need to
extract `docker-rootless-extras-<version>.tar.gz`. The archive can be extracted
under an arbitrary directory listed in the `$PATH`. For example, `/usr/local/bin`,
or `$HOME/bin`.

{% comment %}
{% endcomment %}
### Nightly channel

{% comment %}
{% endcomment %}
To install a nightly version of the Rootless Docker, run the installation script
using `CHANNEL="nightly"`:

```console
$ curl -fsSL https://get.docker.com/rootless | CHANNEL="nightly" sh
```

{% comment %}
{% endcomment %}
The raw binary archives are available at:
- https://master.dockerproject.org/linux/x86\_64/docker-rootless-extras.tgz
- https://master.dockerproject.org/linux/x86\_64/docker.tgz

{% comment %}
## Usage
{% endcomment %}
{: #usage }
## 利用方法

{% comment %}
### Daemon
{% endcomment %}
{: #daemon }
### デーモン

{% comment %}
Use `systemctl --user` to manage the lifecycle of the daemon:
{% endcomment %}
Use `systemctl --user` to manage the lifecycle of the daemon:

```console
$ systemctl --user start docker
```

{% comment %}
To launch the daemon on system startup, enable the systemd service and lingering:
{% endcomment %}
To launch the daemon on system startup, enable the systemd service and lingering:

```console
$ systemctl --user enable docker
$ sudo loginctl enable-linger $(whoami)
```

{% comment %}
To run the daemon directly without systemd, you need to run
`dockerd-rootless.sh` instead of `dockerd`:
{% endcomment %}
To run the daemon directly without systemd, you need to run
`dockerd-rootless.sh` instead of `dockerd`:

```console
$ dockerd-rootless.sh --experimental --storage-driver vfs
```

{% comment %}
{% endcomment %}
As Rootless mode is experimental, you need to run
`dockerd-rootless.sh` with `--experimental`.

{% comment %}
{% endcomment %}
You also need `--storage-driver vfs` unless you are using Ubuntu or Debian 10
kernel. You don't need to care about these flags if you manage the daemon using
systemd, as these flags are automatically added to the systemd unit file.

{% comment %}
{% endcomment %}
Remarks about directory paths:

{% comment %}
{% endcomment %}
- The socket path is set to `$XDG_RUNTIME_DIR/docker.sock` by default.
  `$XDG_RUNTIME_DIR` is typically set to `/run/user/$UID`.
- The data dir is set to `~/.local/share/docker` by default.
- The exec dir is set to `$XDG_RUNTIME_DIR/docker` by default.
- The daemon config dir is set to `~/.config/docker` (not `~/.docker`, which is
  used by the client) by default.

{% comment %}
{% endcomment %}
Other remarks:

{% comment %}
{% endcomment %}
- The `dockerd-rootless.sh` script executes `dockerd` in its own user, mount,
  and network namespaces. You can enter the namespaces by running
  `nsenter -U --preserve-credentials -n -m -t $(cat $XDG_RUNTIME_DIR/docker.pid)`.
- `docker info` shows `rootless` in `SecurityOptions`
- `docker info` shows `none` as `Cgroup Driver`

{% comment %}
{% endcomment %}
### Client

{% comment %}
{% endcomment %}
You need to specify the socket path explicitly.

{% comment %}
{% endcomment %}
To specify the socket path using `$DOCKER_HOST`:

```console
$ export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock
$ docker run -d -p 8080:80 nginx
```

{% comment %}
{% endcomment %}
To specify the socket path using `docker context`:

```console
$ docker context create rootless --description "for rootless mode" --docker "host=unix://$XDG_RUNTIME_DIR/docker.sock"
rootless
Successfully created context "rootless"
$ docker context use rootless
rootless
Current context is now "rootless"
$ docker run -d -p 8080:80 nginx
```

{% comment %}
{% endcomment %}
## Best practices

{% comment %}
{% endcomment %}
### Rootless Docker in Docker

{% comment %}
{% endcomment %}
To run Rootless Docker inside "rootful" Docker, use the `docker:<version>-dind-rootless`
image instead of `docker:<version>-dind`.

```console
$ docker run -d --name dind-rootless --privileged docker:19.03-dind-rootless --experimental
```

{% comment %}
{% endcomment %}
The `docker:<version>-dind-rootless` image runs as a non-root user (UID 1000).
However, `--privileged` is required for disabling seccomp, AppArmor, and mount
masks.

{% comment %}
{% endcomment %}
### Expose Docker API socket through TCP

{% comment %}
{% endcomment %}
To expose the Docker API socket through TCP, you need to launch `dockerd-rootless.sh`
with `DOCKERD_ROOTLESS_ROOTLESSKIT_FLAGS="-p 0.0.0.0:2376:2376/tcp"`.

```console
$ DOCKERD_ROOTLESS_ROOTLESSKIT_FLAGS="-p 0.0.0.0:2376:2376/tcp" \
  dockerd-rootless.sh --experimental \
  -H tcp://0.0.0.0:2376 \
  --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem
```

{% comment %}
{% endcomment %}
### Expose Docker API socket through SSH

{% comment %}
{% endcomment %}
To expose the Docker API socket through SSH, you need to make sure `$DOCKER_HOST`
is set on the remote host.

```console
$ ssh -l <REMOTEUSER> <REMOTEHOST> 'echo $DOCKER_HOST'
unix:///run/user/1001/docker.sock
$ docker -H ssh://<REMOTEUSER>@<REMOTEHOST> run ...
```

{% comment %}
{% endcomment %}
### Routing ping packets

{% comment %}
{% endcomment %}
On some distributions, `ping` does not work by default.

{% comment %}
{% endcomment %}
Add `net.ipv4.ping_group_range = 0   2147483647` to `/etc/sysctl.conf` (or
`/etc/sysctl.d`) and run `sudo sysctl --system` to allow using `ping`.

{% comment %}
{% endcomment %}
### Exposing privileged ports

{% comment %}
{% endcomment %}
To expose privileged ports (< 1024), set `CAP_NET_BIND_SERVICE` on `rootlesskit` binary.

```console
$ sudo setcap cap_net_bind_service=ep $HOME/bin/rootlesskit
```

{% comment %}
{% endcomment %}
Or add `net.ipv4.ip_unprivileged_port_start=0` to `/etc/sysctl.conf` (or
`/etc/sysctl.d`) and run `sudo sysctl --system`.

{% comment %}
{% endcomment %}
### Limiting resources

{% comment %}
{% endcomment %}
In Docker 19.03, rootless mode ignores cgroup-related `docker run` flags such as
`--cpus`, `--memory`, --pids-limit`.

{% comment %}
{% endcomment %}
However, you can still use the traditional `ulimit` and [`cpulimit`](https://github.com/opsengine/cpulimit),
though they work in process-granularity rather than in container-granularity,
and can be arbitrarily disabled by the container process.

{% comment %}
{% endcomment %}
For example:

{% comment %}
{% endcomment %}
- To limit CPU usage to 0.5 cores (similar to `docker run --cpus 0.5`):
  `docker run <IMAGE> cpulimit --limit=50 --include-children <COMMAND>`
- To limit max VSZ to 64MiB (similar to `docker run --memory 64m`):
  `docker run <IMAGE> sh -c "ulimit -v 65536; <COMMAND>"`

{% comment %}
{% endcomment %}
- To limit max number of processes to 100 per namespaced UID 2000
  (similar to `docker run --pids-limit=100`):
  `docker run --user 2000 --ulimit nproc=100 <IMAGE> <COMMAND>`

{% comment %}
{% endcomment %}
### Changing the network stack

{% comment %}
{% endcomment %}
`dockerd-rootless.sh` uses [slirp4netns](https://github.com/rootless-containers/slirp4netns)
(if installed) or [VPNKit](https://github.com/moby/vpnkit) as the network stack
by default.

{% comment %}
{% endcomment %}
These network stacks run in userspace and might have performance overhead.
See [RootlessKit documentation](https://github.com/rootless-containers/rootlesskit/tree/v0.9.5#network-drivers) for further information.

{% comment %}
{% endcomment %}
Optionally, you can use `lxc-user-nic` instead for the best performance.
To use `lxc-user-nic`, you need to edit [`/etc/lxc/lxc-usernet`](https://github.com/rootless-containers/rootlesskit/tree/v0.9.5#--netlxc-user-nic-experimental)
and set `$DOCKERD_ROOTLESS_ROOTLESSKIT_NET=lxc-user-nic`.

{% comment %}
{% endcomment %}
## Troubleshooting

{% comment %}
{% endcomment %}
### Errors when starting the Docker daemon

{% comment %}
{% endcomment %}
**[rootlesskit:parent] error: failed to start the child: fork/exec /proc/self/exe: operation not permitted**

{% comment %}
{% endcomment %}
This error occurs mostly when the value of `/proc/sys/kernel/unprivileged_userns_clone ` is set to 0:

```console
$ cat /proc/sys/kernel/unprivileged_userns_clone
0
```

{% comment %}
{% endcomment %}
To fix this issue, add  `kernel.unprivileged_userns_clone=1` to
`/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`.

{% comment %}
{% endcomment %}
**[rootlesskit:parent] error: failed to start the child: fork/exec /proc/self/exe: no space left on device**

{% comment %}
{% endcomment %}
This error occurs mostly when the value of `/proc/sys/user/max_user_namespaces` is too small:

```console
$ cat /proc/sys/user/max_user_namespaces
0
```

{% comment %}
{% endcomment %}
To fix this issue, add  `user.max_user_namespaces=28633` to
`/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`.

{% comment %}
{% endcomment %}
**[rootlesskit:parent] error: failed to setup UID/GID map: failed to compute uid/gid map: No subuid ranges found for user 1001 ("testuser")**

{% comment %}
{% endcomment %}
This error occurs when `/etc/subuid` and `/etc/subgid` are not configured. See [Prerequisites](#prerequisites).

{% comment %}
{% endcomment %}
**could not get XDG_RUNTIME_DIR**

{% comment %}
{% endcomment %}
This error occurs when `$XDG_RUNTIME_DIR` is not set.

{% comment %}
{% endcomment %}
On a non-systemd host, you need to create a directory and then set the path:

```console
$ export XDG_RUNTIME_DIR=$HOME/.docker/xrd
$ rm -rf $XDG_RUNTIME_DIR
$ mkdir -p $XDG_RUNTIME_DIR
$ dockerd-rootless.sh --experimental
```

{% comment %}
{% endcomment %}
> **Note**:
> You must remove the directory every time you log out.

{% comment %}
{% endcomment %}
On a systemd host, log into the host using `pam_systemd` (see below).
The value is automatically set to `/run/user/$UID` and cleaned up on every logout.

{% comment %}
{% endcomment %}
**`systemctl --user` fails with "Failed to connect to bus: No such file or directory"**

{% comment %}
{% endcomment %}
This error occurs mostly when you switch from the root user to an non-root user with `sudo`:

```console
# sudo -iu testuser
$ systemctl --user start docker
Failed to connect to bus: No such file or directory
```

{% comment %}
{% endcomment %}
Instead of `sudo -iu <USERNAME>`, you need to log in using `pam_systemd`. For example:

{% comment %}
{% endcomment %}
- Log in through the graphic console
- `ssh <USERNAME>@localhost`
- `machinectl shell <USERNAME>@`

{% comment %}
{% endcomment %}
**The daemon does not start up automatically**

{% comment %}
{% endcomment %}
You need `sudo loginctl enable-linger $(whoami)` to enable the daemon to start
up automatically. See [Usage](#usage).

{% comment %}
{% endcomment %}
**`dockerd` fails with "rootless mode is supported only when running in experimental mode"**

{% comment %}
{% endcomment %}
This error occurs when the daemon is launched without the `--experimental` flag.
See [Usage](#usage).

{% comment %}
{% endcomment %}
### `docker pull` errors

{% comment %}
{% endcomment %}
**docker: failed to register layer: Error processing tar file(exit status 1): lchown &lt;FILE&gt;: invalid argument**

{% comment %}
{% endcomment %}
This error occurs when the number of available entries in `/etc/subuid` or
`/etc/subgid` is not sufficient. The number of entries required vary across
images. However, 65,536 entries are sufficient for most images. See
[Prerequisites](#prerequisites).

{% comment %}
{% endcomment %}
### `docker run` errors

{% comment %}
{% endcomment %}
**`--cpus`, `--memory`, and `--pids-limit` are ignored**

{% comment %}
{% endcomment %}
This is an expected behavior in Docker 19.03. For more information, see [Limiting resources](#limiting-resources).

{% comment %}
{% endcomment %}
**Error response from daemon: cgroups: cgroup mountpoint does not exist: unknown.**

{% comment %}
{% endcomment %}
This error occurs mostly when the host is running in cgroup v2. See the section
[Fedora 31 or later](#fedora-31-or-later) for information on switching the host
to use cgroup v1.

{% comment %}
{% endcomment %}
### Networking errors

{% comment %}
{% endcomment %}
**`docker run -p` fails with `cannot expose privileged port`**

{% comment %}
{% endcomment %}
`docker run -p` fails with this error when a privileged port (< 1024) is specified as the host port.

```console
$ docker run -p 80:80 nginx:alpine
docker: Error response from daemon: driver failed programming external connectivity on endpoint focused_swanson (9e2e139a9d8fc92b37c36edfa6214a6e986fa2028c0cc359812f685173fa6df7): Error starting userland proxy: error while calling PortManager.AddPort(): cannot expose privileged port 80, you might need to add "net.ipv4.ip_unprivileged_port_start=0" (currently 1024) to /etc/sysctl.conf, or set CAP_NET_BIND_SERVICE on rootlesskit binary, or choose a larger port number (>= 1024): listen tcp 0.0.0.0:80: bind: permission denied.
```

{% comment %}
{% endcomment %}
When you experience this error, consider using an unprivileged port instead. For example, 8080 instead of 80.

```console
$ docker run -p 8080:80 nginx:alpine
```

{% comment %}
{% endcomment %}
To allow exposing privileged ports, see [Exposing privileged ports](#exposing-privileged-ports).

{% comment %}
{% endcomment %}
**ping doesn't work**

{% comment %}
{% endcomment %}
Ping does not work when `/proc/sys/net/ipv4/ping_group_range` is set to `1 0`:

```console
$ cat /proc/sys/net/ipv4/ping_group_range
1       0
```

{% comment %}
{% endcomment %}
For details, see [Routing ping packets](#routing-ping-packets).

{% comment %}
{% endcomment %}
**`IPAddress` shown in `docker inspect` is unreachable**

{% comment %}
{% endcomment %}
This is an expected behavior, as the daemon is namespaced inside RootlessKit's
network namespace. Use `docker run -p` instead.

{% comment %}
{% endcomment %}
**`--net=host` doesn't listen ports on the host network namespace**

{% comment %}
{% endcomment %}
This is an expected behavior, as the daemon is namespaced inside RootlessKit's
network namespace. Use `docker run -p` instead.
