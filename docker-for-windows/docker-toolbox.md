---
description: Docker Desktop for Windows と Docker Toolbox
keywords: windows, alpha, beta, toolbox, docker-machine, tutorial
title: Docker Toolbox の移行
---

{% comment %}
This page explains how to migrate your Docker Toolbox disk image, or images if
you have them, to Docker Desktop for Windows.
{% endcomment %}
このページでは Docker Toolbox ディスクイメージの Docker Desktop for Windows への移行方法について説明します。

{% comment %}
## How to migrate Docker Toolbox disk images to Docker Desktop
{% endcomment %}
{: #how-to-migrate-docker-toolbox-disk-images-to-docker-desktop }
## Docker Toolbox ディスクイメージの Docker Desktop への移行方法

{% comment %}
Docker Desktop does not propose Toolbox image migration as part of its
installer since version 18.01.0. You can migrate existing Docker
Toolbox images with the steps described below.
{% endcomment %}
Docker Desktop ではバージョン 18.01.0 以降、インストール処理において Toolbox イメージの移行操作を行わないようになりました。
既存の Docker Toolbox イメージは、以下に示すスクリプトを使って移行できます。

{% comment %}
In a terminal, while running Toolbox, use `docker commit` to create an image snapshot
from a container, for each container you wish to preserve:
{% endcomment %}
Toolbox の実行中、ターミナル画面において `docker commit` を実行して、コンテナーからイメージスナップショットを生成します。
これは保存しておきたいコンテナーすべてに対して行います。

```
> docker commit nginx
sha256:1bc0ee792d144f0f9a1b926b862dc88b0206364b0931be700a313111025df022
```

{% comment %}
Next, export each of these images (and any other images you wish to keep):
{% endcomment %}
上のイメージを（そして他にも保存しておきたいイメージがあればそれも含めて）エクスポートします。

```
> docker save -o nginx.tar sha256:1bc0ee792d144f0f9a1b926b862dc88b0206364b0931be700a313111025df022
```

{% comment %}
Next, when running Docker Desktop on Windows, reload all these images:
{% endcomment %}
Docker Desktop on Windows が実行中に、上のイメージをすべてリロードします。

```
> docker load -i nginx.tar
Loaded image ID: sha256:1bc0ee792d144f0f9a1b926b862dc88b0206364b0931be700a313111025df022
```

{% comment %}
Note these steps will not migrate any `docker volume` contents: these must
be copied across manually.
{% endcomment %}
なお上の手順では `docker volume` によるデータは移行されません。
そういったデータは手動でコピーを行う必要があります。

{% comment %}
## How to uninstall Docker Toolbox
{% endcomment %}
{: #how-to-uninstall-docker-toolbox }
## Docker Toolbox のアンインストール方法

{% comment %}
Whether or not you migrate your Docker Toolbox images, you may decide to
uninstall it. For details on how to perform a clean uninstall of Toolbox,
see [How to uninstall Toolbox](../toolbox/toolbox_install_windows.md#how-to-uninstall-toolbox){: target="_blank" class="_"}.
{% endcomment %}
Docker Toolbox イメージを移行するしないには関係なく、Docker Toolbox のアンインストールを行うことがあるでしょう。
Toolbox の適切なアンインストール方法については [Toolbox のアンインストール方法](../toolbox/toolbox_install_windows.md#how-to-uninstall-toolbox){: target="_blank" class="_"} を参照してください。
