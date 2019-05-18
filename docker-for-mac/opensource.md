---
description: Docker's use of Open Source
keywords: docker, opensource
title: オープンソースコンポーネントとライセンス
notoc: true
---

{% comment %}
Docker Desktop Editions are built using open source software.
For details on the licensing, choose
![whale menu](images/whale-x.png){: .inline} →
**About Docker** from within the application, then click **Acknowledgements**.
{% endcomment %}
Docker Desktop エディションはオープンソースソフトウェアを利用して構築されています。
ライセンスの詳細は、アプリケーション上の![クジラメニュー](images/whale-x.png){: .inline} → **About Docker** を選び **Acknowledgements** をクリックしてください。

{% comment %}
Docker Desktop Editions distribute some components that are licensed under the
GNU General Public License. You can download the source for these components
[here](https://download.docker.com/opensource/License.tar.gz).
{% endcomment %}
Docker Desktop エディションのコンポーネントの中には GNU General Public License によってライセンスされたものがあります。
そのコンポーネントのソースは[ここ](https://download.docker.com/opensource/License.tar.gz)からダウンロードすることができます。

{% comment %}
The sources for `qemu-img` can be obtained
[here](http://wiki.qemu-project.org/download/qemu-2.4.1.tar.bz2). The sources
for the `gettext` and `glib` libraries that `qemu-img` requires were obtained
from [Homebrew](https://brew.sh) and may be retrieved using `brew install
--build-from-source gettext glib`.
{% endcomment %}
`qemu-img`のソースは[ここから](http://wiki.qemu-project.org/download/qemu-2.4.1.tar.bz2)入手できます。
`qemu-img`が必要としているライブラリ`gettext`と`glib`のソースは、かつては[Homebrew](https://brew.sh)から入手できました。
今は `brew install --build-from-source gettext glib` によって入手できるかもしれません。
