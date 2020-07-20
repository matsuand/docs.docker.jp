---
description: Enabling AppArmor in Docker
keywords: AppArmor, security, docker, documentation
title: Docker 向け AppArmor セキュリティプロファイル
---

{% comment %}
AppArmor (Application Armor) is a Linux security module that protects an
operating system and its applications from security threats. To use it, a system
administrator associates an AppArmor security profile with each program. Docker
expects to find an AppArmor policy loaded and enforced.
{% endcomment %}
AppArmor (Application Armor) は Linux におけるモジュールの一つであり、オペレーティングシステムやアプリケーションをセキュリティの脅威から保護するものです。
これを利用するには、システム管理者が各プログラムに対して AppArmor セキュリティプロファイルを関連づけます。
Docker は、AppArmor ポリシーがロードされ適用されているかどうかを調べます。

{% comment %}
Docker automatically generates and loads a default profile for containers named
`docker-default`. On Docker versions `1.13.0` and later, the Docker binary generates
this profile in `tmpfs` and then loads it into the kernel. On Docker versions
earlier than `1.13.0`, this profile is generated in `/etc/apparmor.d/docker`
instead.
{% endcomment %}
Docker はコンテナーに対するデフォルトプロファイルとして `docker-default` というものを、自動的に生成してロードします。
Docker バージョン `1.13.0` およびそれ以降においては、Docker 実行モジュールが `tmpfs` にこのプロファイルを生成してカーネルにロードします。
`1.13.0` よりも古いバージョンでは、`/etc/apparmor.d/docker` にプロファイルが生成されます。

{% comment %}
> **Note**: This profile is used on containers, _not_ on the Docker Daemon.
{% endcomment %}
> **メモ**: このプロファイルはコンテナーが利用するものであって、Docker デーモンが利用するものでは **ありません**。

{% comment %}
A profile for the Docker Engine daemon exists but it is not currently installed
with the `deb` packages. If you are interested in the source for the daemon
profile, it is located in
[contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor)
in the Docker Engine source repository.
{% endcomment %}
Docker Engine のデーモン用のプロファイルというものが存在していますが、それは現時点において `deb` パッケージからはインストールされません。
デーモン用のプロファイルソースに興味のある方は、Docker Engine のソースディレクトリ内の [contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor) にあるので参照してください。

{% comment %}
## Understand the policies
{% endcomment %}
{: #understand-the-policies }
## ポリシーの理解

{% comment %}
The `docker-default` profile is the default for running containers. It is
moderately protective while providing wide application compatibility. The
profile is generated from the following
[template](https://github.com/moby/moby/blob/master/profiles/apparmor/template.go).
{% endcomment %}
`docker-default` プロファイルは、コンテナーを起動させるためのデフォルトとなるものです。
これは幅広くアプリケーションとの互換性を提供しつつ、適度なセキュリティ保護を実現します。
このプロファイルは [Go 言語のテンプレート](https://github.com/moby/moby/blob/master/profiles/apparmor/template.go) から生成されます。

{% comment %}
When you run a container, it uses the `docker-default` policy unless you
override it with the `security-opt` option. For example, the following
explicitly specifies the default policy:
{% endcomment %}
コンテナーを起動するとき、通常は `docker-default` ポリシーが適用されます。
ただし `security-opt` オプションを指定すれば、それがオーバーライドされま
たとえば以下に示すのは、明示的にデフォルトポリシーを指定する例です。

```bash
$ docker run --rm -it --security-opt apparmor=docker-default hello-world
```

{% comment %}
## Load and unload profiles
{% endcomment %}
{: #load-and-unload-profiles }
## プロファイルのロード、アンロード

{% comment %}
To load a new profile into AppArmor for use with containers:
{% endcomment %}
コンテナーが利用できるように、AppArmor 内に新たなプロファイルをロードします。

```bash
$ apparmor_parser -r -W /path/to/your_profile
```

{% comment %}
Then, run the custom profile with `--security-opt` like so:
{% endcomment %}
そして `--security-opt` を利用して、ユーザー独自のプロファイルを実行します。

```bash
$ docker run --rm -it --security-opt apparmor=your_profile hello-world
```

{% comment %}
To unload a profile from AppArmor:
{% endcomment %}
AppArmor からプロファイルをアンロードするには、以下のようにします。

```bash
# unload the profile
$ apparmor_parser -R /path/to/profile
```

{% comment %}
### Resources for writing profiles
{% endcomment %}
{: #resources-for-writing-profiles }
### プロファイル記述のための情報

{% comment %}
The syntax for file globbing in AppArmor is a bit different than some other
globbing implementations. It is highly suggested you take a look at some of the
below resources with regard to AppArmor profile syntax.
{% endcomment %}
AppArmor におけるワイルドカード検索（globbing）の文法は、他のワイルドカード検索とは多少異なります。
AppArmor プロファイルの文法に関しては、以下に示す情報を確認することを強くお勧めします。

{% comment %}
- [Quick Profile Language](https://gitlab.com/apparmor/apparmor/wikis/QuickProfileLanguage)
- [Globbing Syntax](https://gitlab.com/apparmor/apparmor/wikis/AppArmor_Core_Policy_Reference#AppArmor_globbing_syntax)
{% endcomment %}
- [Quick Profile Language](https://gitlab.com/apparmor/apparmor/wikis/QuickProfileLanguage)
- [Globbing Syntax](https://gitlab.com/apparmor/apparmor/wikis/AppArmor_Core_Policy_Reference#AppArmor_globbing_syntax)

{% comment %}
## Nginx example profile
{% endcomment %}
{: #nginx-example-profile }
## Nginx 用のプロファイル例

{% comment %}
In this example, you create a custom AppArmor profile for Nginx. Below is the
custom profile.
{% endcomment %}
ここに示す例では、Nginx 用に AppArmor プロファイルをカスタマイズします。
以下がそのカスタムプロファイルです。

```
#include <tunables/global>


profile docker-nginx flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/base>

  network inet tcp,
  network inet udp,
  network inet icmp,

  deny network raw,

  deny network packet,

  file,
  umount,

  deny /bin/** wl,
  deny /boot/** wl,
  deny /dev/** wl,
  deny /etc/** wl,
  deny /home/** wl,
  deny /lib/** wl,
  deny /lib64/** wl,
  deny /media/** wl,
  deny /mnt/** wl,
  deny /opt/** wl,
  deny /proc/** wl,
  deny /root/** wl,
  deny /sbin/** wl,
  deny /srv/** wl,
  deny /tmp/** wl,
  deny /sys/** wl,
  deny /usr/** wl,

  audit /** w,

  /var/run/nginx.pid w,

  /usr/sbin/nginx ix,

  deny /bin/dash mrwklx,
  deny /bin/sh mrwklx,
  deny /usr/bin/top mrwklx,


  capability chown,
  capability dac_override,
  capability setuid,
  capability setgid,
  capability net_bind_service,

  deny @{PROC}/* w,   # deny write for all files directly in /proc (not in a subdir)
  # deny write to files not in /proc/<number>/** or /proc/sys/**
  deny @{PROC}/{[^1-9],[^1-9][^0-9],[^1-9s][^0-9y][^0-9s],[^1-9][^0-9][^0-9][^0-9]*}/** w,
  deny @{PROC}/sys/[^k]** w,  # deny /proc/sys except /proc/sys/k* (effectively /proc/sys/kernel)
  deny @{PROC}/sys/kernel/{?,??,[^s][^h][^m]**} w,  # deny everything except shm* in /proc/sys/kernel/
  deny @{PROC}/sysrq-trigger rwklx,
  deny @{PROC}/mem rwklx,
  deny @{PROC}/kmem rwklx,
  deny @{PROC}/kcore rwklx,

  deny mount,

  deny /sys/[^f]*/** wklx,
  deny /sys/f[^s]*/** wklx,
  deny /sys/fs/[^c]*/** wklx,
  deny /sys/fs/c[^g]*/** wklx,
  deny /sys/fs/cg[^r]*/** wklx,
  deny /sys/firmware/** rwklx,
  deny /sys/kernel/security/** rwklx,
}
```

{% comment %}
1. Save the custom profile to disk in the
`/etc/apparmor.d/containers/docker-nginx` file.
{% endcomment %}
1. このカスタムプロファイルを `/etc/apparmor.d/containers/docker-nginx` というファイルに保存します。

   {% comment %}
   The file path in this example is not a requirement. In production, you could
   use another.
   {% endcomment %}
   この例におけるファイルパスは必須のものではありません。
   本番環境においては別のものにすることができます。

{% comment %}
2. Load the profile.
{% endcomment %}
2. プロファイルをロードします。

   ```bash
   $ sudo apparmor_parser -r -W /etc/apparmor.d/containers/docker-nginx
   ```

{% comment %}
3. Run a container with the profile.
{% endcomment %}
3. このプロファイルを使ってコンテナーを起動します。

   {% comment %}
   To run nginx in detached mode:
   {% endcomment %}
   nginx をデタッチモードで起動します。

   ```bash
   $ docker run --security-opt "apparmor=docker-nginx" \
        -p 80:80 -d --name apparmor-nginx nginx
   ```

{% comment %}
4. Exec into the running container.
{% endcomment %}
4. exec により実行中のコンテナーに入ります。

   ```bash
   $ docker container exec -it apparmor-nginx bash
   ```

{% comment %}
5. Try some operations to test the profile.
{% endcomment %}
5. 適当な操作を通じてプロファイルを確認します。

   ```bash
   root@6da5a2a930b9:~# ping 8.8.8.8
   ping: Lacking privilege for raw socket.

   root@6da5a2a930b9:/# top
   bash: /usr/bin/top: Permission denied

   root@6da5a2a930b9:~# touch ~/thing
   touch: cannot touch 'thing': Permission denied

   root@6da5a2a930b9:/# sh
   bash: /bin/sh: Permission denied

   root@6da5a2a930b9:/# dash
   bash: /bin/dash: Permission denied
   ```


{% comment %}
Congrats! You just deployed a container secured with a custom apparmor profile!
{% endcomment %}
おめでとうございます。
カスタムな AppArmor プロファイルを利用した、セキュアなコンテナーがデプロイできました。


{% comment %}
## Debug AppArmor
{% endcomment %}
{: #debug-apparmor }
## AppArmor のデバッグ

{% comment %}
You can use `dmesg` to debug problems and `aa-status` check the loaded profiles.
{% endcomment %}
`dmesg` を使ってデバッグすることができます。
また `aa-status` を使えば、ロード済みプロファイルを確認することができます。

{% comment %}
### Use dmesg
{% endcomment %}
{: #use-dmesg }
### dmesg の利用

{% comment %}
Here are some helpful tips for debugging any problems you might be facing with
regard to AppArmor.
{% endcomment %}
AppArmor に関して問題が発生したとしても、デバッグに役立つヒントをここに示します。

{% comment %}
AppArmor sends quite verbose messaging to `dmesg`. Usually an AppArmor line
looks like the following:
{% endcomment %}
AppArmor は `dmesg` に対して極めて詳細なメッセージ出力を行います。
通常 AppArmor の出力は以下のようなものです。

```
[ 5442.864673] audit: type=1400 audit(1453830992.845:37): apparmor="ALLOWED" operation="open" profile="/usr/bin/docker" name="/home/jessie/docker/man/man1/docker-attach.1" pid=10923 comm="docker" requested_mask="r" denied_mask="r" fsuid=1000 ouid=0
```

{% comment %}
In the above example, you can see `profile=/usr/bin/docker`. This means the
user has the `docker-engine` (Docker Engine Daemon) profile loaded.
{% endcomment %}
上の例では `profile=/usr/bin/docker` という記述があります。
これはユーザーが、`docker-engine`（Docker Engine のデーモン）をロードしていることを意味します。

{% comment %}
> **Note**: On version of Ubuntu > 14.04 this is all fine and well, but Trusty
> users might run into some issues when trying to `docker container exec`.
{% endcomment %}
> **メモ**: Ubuntu のバージョンが 14.04 より新しい場合は、何も問題はありませんが Trusty を利用している場合、`docker container exec` を実行することで問題が発生する場合があります。

{% comment %}
Look at another log line:
{% endcomment %}
別のログ行を見てみます。

```
[ 3256.689120] type=1400 audit(1405454041.341:73): apparmor="DENIED" operation="ptrace" profile="docker-default" pid=17651 comm="docker" requested_mask="receive" denied_mask="receive"
```

{% comment %}
This time the profile is `docker-default`, which is run on containers by
default unless in `privileged` mode. This line shows that apparmor has denied
`ptrace` in the container. This is exactly as expected.
{% endcomment %}
この場合、プロファイルは `docker-default` であり、`privileged` モードでない限り、デフォルトでコンテナー上に実行されているものです。
このログ行は、AppArmor がコンテナー内の `ptrace` を拒否していることがわかります。
これはまさに期待どおりの動作です。

{% comment %}
### Use aa-status
{% endcomment %}
{: #use-aa-status }
### aa-status の利用

{% comment %}
If you need to check which profiles are loaded,  you can use `aa-status`. The
output looks like:
{% endcomment %}
どのプロファイルがロードされているかを確認するには `aa-status` を使います。
出力は以下のようになります。

```bash
$ sudo aa-status
apparmor module is loaded.
14 profiles are loaded.
1 profiles are in enforce mode.
   docker-default
13 profiles are in complain mode.
   /usr/bin/docker
   /usr/bin/docker///bin/cat
   /usr/bin/docker///bin/ps
   /usr/bin/docker///sbin/apparmor_parser
   /usr/bin/docker///sbin/auplink
   /usr/bin/docker///sbin/blkid
   /usr/bin/docker///sbin/iptables
   /usr/bin/docker///sbin/mke2fs
   /usr/bin/docker///sbin/modprobe
   /usr/bin/docker///sbin/tune2fs
   /usr/bin/docker///sbin/xtables-multi
   /usr/bin/docker///sbin/zfs
   /usr/bin/docker///usr/bin/xz
38 processes have profiles defined.
37 processes are in enforce mode.
   docker-default (6044)
   ...
   docker-default (31899)
1 processes are in complain mode.
   /usr/bin/docker (29756)
0 processes are unconfined but have a profile defined.
```

{% comment %}
The above output shows that the `docker-default` profile running on various
container PIDs is in `enforce` mode. This means AppArmor is actively blocking
and auditing in `dmesg` anything outside the bounds of the `docker-default`
profile.
{% endcomment %}
上の出力からわかることは、`docker-default` プロファイルがさまざまなコンテナー PID 上において実行していて、`enforce` モードにより動作しているということです。
つまり `docker-default` プロファイルの範囲外のところで AppArmor は、`dmesg` においてブロックと監査を効果的に行っているということです。

{% comment %}
The output above also shows the `/usr/bin/docker` (Docker Engine daemon) profile
is running in `complain` mode. This means AppArmor _only_ logs to `dmesg`
activity outside the bounds of the profile. (Except in the case of Ubuntu
Trusty, where some interesting behaviors are enforced.)
{% endcomment %}
さらに `/usr/bin/docker`（Docker Engine デーモン）プロファイルは `complain` モードにより動作していることもわかります。
これは AppArmor がプロファイルの範囲外にて、`dmesg` に対して **のみ** ログ出力を行っているということです。
（Ubuntu Trusty の場合は例外で、`enforce` モードにより動作するものがあります。）

{% comment %}
## Contribute Docker's AppArmor code
{% endcomment %}
{: #contribute-dockers-apparmor-code }
## Docker 向け AppArmor コードの提供

{% comment %}
Advanced users and package managers can find a profile for `/usr/bin/docker`
(Docker Engine Daemon) underneath
[contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor)
in the Docker Engine source repository.
{% endcomment %}
上級ユーザーやパッケージ管理者は、`/usr/bin/docker`（Docker Engine デーモン）に対するプロファイルを、Docker Engine ソースリポジトリ内の [contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor) から検索し利用しています。

{% comment %}
The `docker-default` profile for containers lives in
[profiles/apparmor](https://github.com/moby/moby/tree/master/profiles/apparmor).
{% endcomment %}
コンテナー向けの `docker-default` プロファイルは [profiles/apparmor](https://github.com/moby/moby/tree/master/profiles/apparmor) にあります。
