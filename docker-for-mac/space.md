---
description: ディスク利用。
keywords: mac, disk
title: Docker for Mac におけるディスク利用
---

{% comment %}
Docker for Mac stores Linux containers and images in a single, large "disk image" file
in the Mac filesystem. This is different from Docker on Linux, which usually stores containers
and images in the `/var/lib/docker` directory.
{% endcomment %}
Docker for Mac では Linux コンテナーやイメージを一つの大きな「ディスクイメージ」として Mac ファイルシステム内に保存します。
これは Linux 上の Docker とは異なっていて、Linux 上ではコンテナーやイメージは通常 `/var/lib/docker` ディレクトリに保存されます。

{% comment %}
## Where is the "disk image" file?
{% endcomment %}
{: #where-is-the-disk-image-file }
## 「ディスクファイル」はどこにあるか？

{% comment %}
To locate the "disk image" file, first select the whale menu icon and then select
**Preferences...**. When the **Preferences...** window is displayed, select **Disk** and then **Reveal in Finder**:
{% endcomment %}
To locate the "disk image" file, first select the whale menu icon and then select
**Preferences...**. When the **Preferences...** window is displayed, select **Disk** and then **Reveal in Finder**:

{% comment %}
![Disk preferences](images/settings-disk.png)
{% endcomment %}
![Disk preferences](images/settings-disk.png)

{% comment %}
The **Preferences...** window shows how much actual disk space the "disk image" file is consuming.
In this example, the "disk image" file is consuming 2.4 GB out of a maximum of 64 GB.
{% endcomment %}
The **Preferences...** window shows how much actual disk space the "disk image" file is consuming.
In this example, the "disk image" file is consuming 2.4 GB out of a maximum of 64 GB.

{% comment %}
Note that other tools might display space usage of the file in terms of the maximum file size, not the actual file size.
{% endcomment %}
Note that other tools might display space usage of the file in terms of the maximum file size, not the actual file size.

{% comment %}
## If the file is too big
{% endcomment %}
{: #if-the-file-is-too-big }
## ファイルが大きすぎる場合

{% comment %}
If the file is too big, you can
- move it to a bigger drive,
- delete unnecessary containers and images, or
- reduce the maximum allowable size of the file.
{% endcomment %}
If the file is too big, you can
- move it to a bigger drive,
- delete unnecessary containers and images, or
- reduce the maximum allowable size of the file.

{% comment %}
### Move the file to a bigger drive
{% endcomment %}
{: #move-the-file-to-a-bigger-drive }
### Move the file to a bigger drive

{% comment %}
To move the file, open the **Preferences...** menu, select **Disk**  and then select
on **Move disk image**. Do not move the file directly in the finder or Docker for Mac will
lose track of it.
{% endcomment %}
To move the file, open the **Preferences...** menu, select **Disk**  and then select
on **Move disk image**. Do not move the file directly in the finder or Docker for Mac will
lose track of it.

{% comment %}
### Delete unnecessary containers and images
{% endcomment %}
{: #delete-unnecessary-containers-and-images }
### Delete unnecessary containers and images

{% comment %}
To check whether you have too many unnecessary containers and images:
{% endcomment %}
To check whether you have too many unnecessary containers and images:

{% comment %}
If your client and daemon API are version 1.25 or later (use the docker version command on the client to check your client and daemon API versions.), you can display detailed space usage information with:
{% endcomment %}
If your client and daemon API are version 1.25 or later (use the docker version command on the client to check your client and daemon API versions.), you can display detailed space usage information with:

```
docker system df -v
```

{% comment %}
Alternatively, you can list images with:
{% endcomment %}
Alternatively, you can list images with:
```bash
$ docker image ls
```
and then list containers with:
```bash
$ docker container ls -a
```

{% comment %}
If there are lots of unneeded objects, try the command
{% endcomment %}
If there are lots of unneeded objects, try the command
```bash
$ docker system prune
```
This removes all stopped containers, unused networks, dangling images, and build cache.

{% comment %}
Note that it might take a few minutes before space becomes free on the host, depending
on what format the "disk image" file is in:
{% endcomment %}
Note that it might take a few minutes before space becomes free on the host, depending
on what format the "disk image" file is in:
{% comment %}
{% endcomment %}
- If the file is named `Docker.raw`: space on the host should be reclaimed within a few
  seconds.
- If the file is named `Docker.qcow2`: space will be freed by a background process after
  a few minutes.

{% comment %}
Note that space is only freed when images are deleted. Space is not freed automatically
when files are deleted inside running containers. To trigger a space reclamation at any
point, use the command:
{% endcomment %}
Note that space is only freed when images are deleted. Space is not freed automatically
when files are deleted inside running containers. To trigger a space reclamation at any
point, use the command:

```
$ docker run --privileged --pid=host justincormack/nsenter1 /sbin/fstrim /var/lib/docker
```

{% comment %}
{% endcomment %}
Note that many tools will report the maximum file size, not the actual file size.
To query the actual size of the file on the host from a terminal, use:
```bash
$ cd ~/Library/Containers/com.docker.docker/Data
$ cd vms/0   # or com.docker.driver.amd64-linux
$ ls -klsh Docker.raw
2333548 -rw-r--r--@ 1 akim  staff    64G Dec 13 17:42 Docker.raw
```
{% comment %}
{% endcomment %}
In this example, the actual size of the disk is `2333548` KB, whereas the maximum size
of the disk is `64` GB.

{% comment %}
### Reduce the maximum size of the file
{% endcomment %}
{: #reduce-the-maximum-size-of-the-file }
### Reduce the maximum size of the file

{% comment %}
To reduce the maximum size of the file, select the whale menu icon and then select
**Preferences...**. When the **Preferences...** window is displayed, select **Disk**.
The **Disk** window contains a slider that allows the maximum disk size to be set.
**Warning**: If the maximum size is reduced, the current file will be deleted and, therefore, all
containers and images will be lost.
{% endcomment %}
To reduce the maximum size of the file, select the whale menu icon and then select
**Preferences...**. When the **Preferences...** window is displayed, select **Disk**.
The **Disk** window contains a slider that allows the maximum disk size to be set.
**Warning**: If the maximum size is reduced, the current file will be deleted and, therefore, all
containers and images will be lost.

