---
description: マシンからディレクトリをマウントします。
keywords: machine, mount, subcommand
title: docker-machine mount
hide_from_sitemap: true
---

{% comment %}
Mount directories from a machine to your local host, using `sshfs`.
{% endcomment %}
マシンからローカルホストに向けてディレクトリをマウントします。
マウントには `sshfs` が用いられます。

{% comment %}
The notation is `machinename:/path/to/dir` for the argument; you can also supply an alternative mount point (default is the same dir path).
{% endcomment %}
引数の書式は `machinename:/path/to/dir` です。
マウントポイントに別名を指定することもできます。
（デフォルトは同名のパスです。）

{% comment %}
## Example
{% endcomment %}
{: #example }
## 利用例

{% comment %}
Consider the following example:
{% endcomment %}
以下の例を取り上げます。

```bash
$ mkdir foo
$ docker-machine ssh dev mkdir foo
$ docker-machine mount dev:/home/docker/foo foo
$ touch foo/bar
$ docker-machine ssh dev ls foo
bar
```


{% comment %}
Now you can use the directory on the machine, for mounting into containers.
Any changes done in the local directory, is reflected in the machine too.
{% endcomment %}
マシン上のディレクトリは、コンテナー内にマウントしたディレクトリとして利用できます。
つまりローカルディレクトリ内で変更を行うと、それがマシン上にも反映されます。

```bash
$ eval $(docker-machine env dev)
$ docker run -v /home/docker/foo:/tmp/foo busybox ls /tmp/foo
bar
$ touch foo/baz
$ docker run -v /home/docker/foo:/tmp/foo busybox ls /tmp/foo
bar
baz
```

{% comment %}
The files are actually being transferred using `sftp` (over an ssh connection),
so this program ("sftp") needs to be present on the machine - but it usually is.
{% endcomment %}
対象のファイルは、実際には `sftp`（ssh 接続を介した）を利用して転送されます。
したがってこのプログラム（sftp）がマシン上になければなりませんが、普通は存在しているものです。


{% comment %}
To unmount the directory again, you can use the same options but the  `-u` flag.
You can also call `fuserunmount` (or `fusermount -u`) commands directly.
{% endcomment %}
ディレクトリをアンマウントするには、同じオプションに加えて `-u` フラグを使います。
コマンド `fuserunmount`（または `fusermount -u`）を直接実行してもかまいません。

```bash
$ docker-machine mount -u dev:/home/docker/foo foo
$ rmdir foo
```
{% comment %}
**Files are actually being stored on the machine, *not* on the host.**
So make sure to make a copy of any files you want to keep, before removing it!
{% endcomment %}
**ファイルが保存されるのはマシン上であって、ホスト上ではありません。**
したがってマシンを削除するなら、保存しておきたいファイルはコピーをとっておいてください。
