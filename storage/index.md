---
description: コンテナー内にデータを保持するための概要
title: Docker におけるデータ管理
keywords: storage, persistence, data persistence, volumes, mounts, bind mounts
redirect_from:
- engine/admin/volumes/
---

{% comment %}
By default all files created inside a container are stored on a writable
container layer. This means that:
{% endcomment %}
コンテナー内に生成されたファイルは、デフォルトではすべて書き込み可能なコンテナーレイヤーに保存されます。
これは以下を意味することになります。

{% comment %}
- The data doesn't persist when that container no longer exists, and it can be
  difficult to get the data out of the container if another process needs it.
- A container's writable layer is tightly coupled to the host machine
  where the container is running. You can't easily move the data somewhere else.
- Writing into a container's writable layer requires a
  [storage driver](/storage/storagedriver/) to manage the
  filesystem. The storage driver provides a union filesystem, using the Linux
  kernel. This extra abstraction reduces performance as compared to using
  _data volumes_, which write directly to the host filesystem.
{% endcomment %}
- コンテナーが存在しなくなると、そのデータを保持しておくことはできなくなります。
  そしてそのデータをコンテナーの外部から取得したいと思っても、外部プロセスがこれを行うことは極めて困難になります。
- コンテナーの書き込み可能レイヤーは、コンテナーが稼動しているホストマシンに強く結び付けられています。
  したがってその中のデータをどこかに移動させることは容易ではありません。
- コンテナーの書き込み可能レイヤーにデータを書き込むためには、ファイルシステムを管理する [ストレージドライバー](/storage/storagedriver/) が必要になります。
  このストレージドライバーは、Linux カーネルを利用してユニオンファイルシステム（union filesystem）を提供します。
  この特別な抽象ファイルシステムは **データボリューム** に比べると性能が劣ります。
  データボリュームであれば、ホストのファイルシステムに直接データを書き込むことができます。

{% comment %}
Docker has two options for containers to store files in the host machine, so
that the files are persisted even after the container stops: _volumes_, and
_bind mounts_. If you're running Docker on Linux you can also use a _tmpfs mount_.
If you're running Docker on Windows you can also use a _named pipe_.
{% endcomment %}
Docker コンテナーにおけるファイルをホストマシン上に保存する方法は 2 つあります。
これを行えば、コンテナーが停止した後にもデータを維持していくことができます。
その 2 つの方法とは **ボリューム** あるいは **バインドマウント** です。
Linux 上において Docker を稼動していれば、さらに **tmpfs マウント** を用いることもできます。
Windows 上において Docker を稼動していれば、**名前つきパイプ**（named pipe）を用いることもできます。

{% comment %}
Keep reading for more information about these two ways of persisting data.
{% endcomment %}
データを保持するためのこの 2 つの方法について、さらに具体的に読み進めてください。

{% comment %}
## Choose the right type of mount
{% endcomment %}
{: #choose-the-right-type-of-mount }
## 適切なマウント方法の選定

{% comment %}
No matter which type of mount you choose to use, the data looks the same from
within the container. It is exposed as either a directory or an individual file
in the container's filesystem.
{% endcomment %}
どの種類のマウント方法を選んだとしても、コンテナー内部からのデータの見え方は同じです。
そのデータはコンテナー内のファイルシステム上において、ディレクトリとして見えるか、あるいは個別のファイルとして見えます。

{% comment %}
An easy way to visualize the difference among volumes, bind mounts, and `tmpfs`
mounts is to think about where the data lives on the Docker host.
{% endcomment %}
ボリューム、バインドマウント、`tmpfs` マウントにどのような違いがあるのかは、そのデータが Docker ホスト上のどこに保存されるかを考えてみると、わかりやすくなります。

{% comment %}
![types of mounts and where they live on the Docker host](images/types-of-mounts.png)
{% endcomment %}
![マウントの種類と Docker ホスト上でのデータ保存場所](images/types-of-mounts.png)

{% comment %}
- **Volumes** are stored in a part of the host filesystem which is _managed by
  Docker_ (`/var/lib/docker/volumes/` on Linux). Non-Docker processes should not
  modify this part of the filesystem. Volumes are the best way to persist data
  in Docker.
{% endcomment %}
- **ボリューム** はホストのファイルシステムの一部としてデータが保存されます。
  そしてこれは **Docker によって管理されます**。
  （Linux であれば `/var/lib/docker/volumes/` に保存されます。）
  Docker 以外のプロセスは、このファイルシステム上の保存場所への変更操作を行うことはできません。
  ボリュームは Docker においてデータを維持するための最良の方法です。

{% comment %}
- **Bind mounts** may be stored *anywhere* on the host system. They may even be
  important system files or directories. Non-Docker processes on the Docker host
  or a Docker container can modify them at any time.
{% endcomment %}
- **Bind mounts** may be stored *anywhere* on the host system. They may even be
  important system files or directories. Non-Docker processes on the Docker host
  or a Docker container can modify them at any time.

{% comment %}
- **`tmpfs` mounts** are stored in the host system's memory only, and are never
  written to the host system's filesystem.
{% endcomment %}
- **`tmpfs` mounts** are stored in the host system's memory only, and are never
  written to the host system's filesystem.

{% comment %}
### More details about mount types
{% endcomment %}
### More details about mount types

{% comment %}
- **[Volumes](volumes.md)**: Created and managed by Docker. You can create a
  volume explicitly using the `docker volume create` command, or Docker can
  create a volume during container or service creation.
{% endcomment %}
- **[Volumes](volumes.md)**: Created and managed by Docker. You can create a
  volume explicitly using the `docker volume create` command, or Docker can
  create a volume during container or service creation.

{% comment %}
  When you create a volume, it is stored within a directory on the Docker
  host. When you mount the volume into a container, this directory is what is
  mounted into the container. This is similar to the way that bind mounts work,
  except that volumes are managed by Docker and are isolated from the core
  functionality of the host machine.
{% endcomment %}
  When you create a volume, it is stored within a directory on the Docker
  host. When you mount the volume into a container, this directory is what is
  mounted into the container. This is similar to the way that bind mounts work,
  except that volumes are managed by Docker and are isolated from the core
  functionality of the host machine.

{% comment %}
  A given volume can be mounted into multiple containers simultaneously. When no
  running container is using a volume, the volume is still available to Docker
  and is not removed automatically. You can remove unused volumes using `docker
  volume prune`.
{% endcomment %}
  A given volume can be mounted into multiple containers simultaneously. When no
  running container is using a volume, the volume is still available to Docker
  and is not removed automatically. You can remove unused volumes using `docker
  volume prune`.

{% comment %}
  When you mount a volume, it may be **named** or **anonymous**. Anonymous
  volumes are not given an explicit name when they are first mounted into a
  container, so Docker gives them a random name that is guaranteed to be unique
  within a given Docker host. Besides the name, named and anonymous volumes
  behave in the same ways.
{% endcomment %}
  When you mount a volume, it may be **named** or **anonymous**. Anonymous
  volumes are not given an explicit name when they are first mounted into a
  container, so Docker gives them a random name that is guaranteed to be unique
  within a given Docker host. Besides the name, named and anonymous volumes
  behave in the same ways.

{% comment %}
  Volumes also support the use of _volume drivers_, which allow you to store
  your data on remote hosts or cloud providers, among other possibilities.
{% endcomment %}
  Volumes also support the use of _volume drivers_, which allow you to store
  your data on remote hosts or cloud providers, among other possibilities.

{% comment %}
- **[Bind mounts](bind-mounts.md)**: Available since the early days of Docker.
  Bind mounts have limited functionality compared to volumes. When you use a
  bind mount, a file or directory on the _host machine_ is mounted into a
  container. The file or directory is referenced by its full path on the host
  machine. The file or directory does not need to exist on the Docker host
  already. It is created on demand if it does not yet exist. Bind mounts are
  very performant, but they rely on the host machine's filesystem having a
  specific directory structure available. If you are developing new Docker
  applications, consider using named volumes instead. You can't use
  Docker CLI commands to directly manage bind mounts.
{% endcomment %}
- **[Bind mounts](bind-mounts.md)**: Available since the early days of Docker.
  Bind mounts have limited functionality compared to volumes. When you use a
  bind mount, a file or directory on the _host machine_ is mounted into a
  container. The file or directory is referenced by its full path on the host
  machine. The file or directory does not need to exist on the Docker host
  already. It is created on demand if it does not yet exist. Bind mounts are
  very performant, but they rely on the host machine's filesystem having a
  specific directory structure available. If you are developing new Docker
  applications, consider using named volumes instead. You can't use
  Docker CLI commands to directly manage bind mounts.

  {% comment %}
  {% endcomment %}
  > Bind mounts allow access to sensitive files
  >
  > One side effect of using bind mounts, for better or for worse,
  > is that you can change the **host** filesystem via processes running in a
  > **container**, including creating, modifying, or deleting important system
  > files or directories. This is a powerful ability which can have security
  > implications, including impacting non-Docker processes on the host system.
  {: .important }

{% comment %}
{% endcomment %}
- **[tmpfs mounts](tmpfs.md)**: A `tmpfs` mount is not persisted on disk, either
  on the Docker host or within a container. It can be used by a container during
  the lifetime of the container, to store non-persistent state or sensitive
  information. For instance, internally, swarm services use `tmpfs` mounts to
  mount [secrets](../engine/swarm/secrets.md) into a service's containers.

- **[named pipes](https://docs.microsoft.com/en-us/windows/desktop/ipc/named-pipes)**: An `npipe`
  mount can be used for communication between the Docker host and a container. Common use case is
  to run a third-party tool inside of a container and connect to the Docker Engine API using a named pipe.

{% comment %}
{% endcomment %}
Bind mounts and volumes can both be mounted into containers using the `-v` or
`--volume` flag, but the syntax for each is slightly different. For `tmpfs`
mounts, you can use the `--tmpfs` flag. However, in Docker 17.06 and higher,
we recommend using the `--mount` flag for both containers and services, for
bind mounts, volumes, or `tmpfs` mounts, as the syntax is more clear.

{% comment %}
{% endcomment %}
## Good use cases for volumes

{% comment %}
{% endcomment %}
Volumes are the preferred way to persist data in Docker containers and services.
Some use cases for volumes include:

{% comment %}
{% endcomment %}
- Sharing data among multiple running containers. If you don't explicitly create
  it, a volume is created the first time it is mounted into a container. When
  that container stops or is removed, the volume still exists. Multiple
  containers can mount the same volume simultaneously, either read-write or
  read-only. Volumes are only removed when you explicitly remove them.

{% comment %}
{% endcomment %}
- When the Docker host is not guaranteed to have a given directory or file
  structure. Volumes help you decouple the configuration of the Docker host
  from the container runtime.

{% comment %}
{% endcomment %}
- When you want to store your container's data on a remote host or a cloud
  provider, rather than locally.

{% comment %}
{% endcomment %}
- When you need to back up, restore, or migrate data from one Docker
  host to another, volumes are a better choice. You can stop containers using
  the volume, then back up the volume's directory
  (such as `/var/lib/docker/volumes/<volume-name>`).

{% comment %}
{% endcomment %}
## Good use cases for bind mounts

{% comment %}
{% endcomment %}
In general, you should use volumes where possible. Bind mounts are appropriate
for the following types of use case:

{% comment %}
{% endcomment %}
- Sharing configuration files from the host machine to containers. This is how
  Docker provides DNS resolution to containers by default, by mounting
  `/etc/resolv.conf` from the host machine into each container.

{% comment %}
{% endcomment %}
- Sharing source code or build artifacts between a development environment on
  the Docker host and a container. For instance, you may mount a Maven `target/`
  directory into a container, and each time you build the Maven project on the
  Docker host, the container gets access to the rebuilt artifacts.

  {% comment %}
  {% endcomment %}
  If you use Docker for development this way, your production Dockerfile would
  copy the production-ready artifacts directly into the image, rather than
  relying on a bind mount.

{% comment %}
{% endcomment %}
- When the file or directory structure of the Docker host is guaranteed to be
  consistent with the bind mounts the containers require.

{% comment %}
{% endcomment %}
## Good use cases for tmpfs mounts

{% comment %}
{% endcomment %}
`tmpfs` mounts are best used for cases when you do not want the data to persist
either on the host machine or within the container. This may be for security
reasons or to protect the performance of the container when your application
needs to write a large volume of non-persistent state data.

{% comment %}
{% endcomment %}
## Tips for using bind mounts or volumes

{% comment %}
If you use either bind mounts or volumes, keep the following in mind:
{% endcomment %}
If you use either bind mounts or volumes, keep the following in mind:

{% comment %}
- If you mount an **empty volume** into a directory in the container in which files
  or directories exist, these files or directories are propagated (copied)
  into the volume. Similarly, if you start a container and specify a volume which
  does not already exist, an empty volume is created for you.
  This is a good way to pre-populate data that another container needs.
{% endcomment %}
- If you mount an **empty volume** into a directory in the container in which files
  or directories exist, these files or directories are propagated (copied)
  into the volume. Similarly, if you start a container and specify a volume which
  does not already exist, an empty volume is created for you.
  This is a good way to pre-populate data that another container needs.

{% comment %}
- If you mount a **bind mount or non-empty volume** into a directory in the container
  in which some files or directories exist, these files or directories are
  obscured by the mount, just as if you saved files into `/mnt` on a Linux host
  and then mounted a USB drive into `/mnt`. The contents of `/mnt` would be
  obscured by the contents of the USB drive until the USB drive were unmounted.
  The obscured files are not removed or altered, but are not accessible while the
  bind mount or volume is mounted.
{% endcomment %}
- If you mount a **bind mount or non-empty volume** into a directory in the container
  in which some files or directories exist, these files or directories are
  obscured by the mount, just as if you saved files into `/mnt` on a Linux host
  and then mounted a USB drive into `/mnt`. The contents of `/mnt` would be
  obscured by the contents of the USB drive until the USB drive were unmounted.
  The obscured files are not removed or altered, but are not accessible while the
  bind mount or volume is mounted.

{% comment %}
## Next steps
{% endcomment %}
{: #next-steps }
## 次のステップ

{% comment %}
- Learn more about [volumes](volumes.md).
- Learn more about [bind mounts](bind-mounts.md).
- Learn more about [tmpfs mounts](tmpfs.md).
- Learn more about [storage drivers](/storage/storagedriver/), which
  are not related to bind mounts or volumes, but allow you to store data in a
  container's writable layer.
{% endcomment %}
- Learn more about [volumes](volumes.md).
- Learn more about [bind mounts](bind-mounts.md).
- Learn more about [tmpfs mounts](tmpfs.md).
- Learn more about [storage drivers](/storage/storagedriver/), which
  are not related to bind mounts or volumes, but allow you to store data in a
  container's writable layer.
