---
description: ラベルの説明。これは Docker オブジェクトにおいてメタデータを管理するために用いられます。
keywords: Usage, user guide, labels, metadata, docker, documentation, examples, annotating
title: Docker オブジェクトラベル
redirect_from:
- /engine/userguide/labels-custom-metadata/
---

{% comment %}
Labels are a mechanism for applying metadata to Docker objects, including:
{% endcomment %}
ラベルとは Docker オブジェクトにメタデータを提供する仕組みのことです。
Docker オブジェクトには以下があります。

{% comment %}
- Images
- Containers
- Local daemons
- Volumes
- Networks
- Swarm nodes
- Swarm services
{% endcomment %}
- イメージ
- コンテナー
- ローカル実行されるデーモン
- ボリューム
- ネットワーク
- Swarm ノード
- Swarm サービス

{% comment %}
You can use labels to organize your images, record licensing information, annotate
relationships between containers, volumes, and networks, or in any way that makes
sense for your business or application.
{% endcomment %}
ラベルはさまざまな目的で利用することができます。
イメージを構成したり、ライセンス情報を記録したり、コンテナー、ボリューム、ネットワーク間の関係性を書きとめたりといったものです。
業務やアプリケーションにとって意義のあることなら、どのようなものでも含めて構いません。

{% comment %}
## Label keys and values
{% endcomment %}
{: #label-keys-and-values }
## ラベルのキーとバリュー

{% comment %}
A label is a key-value pair, stored as a string. You can specify multiple labels
for an object, but each key-value pair must be unique within an object. If the
same key is given multiple values, the most-recently-written value overwrites
all previous values.
{% endcomment %}
ラベルはキーバリューペアの形式であり、文字列として保存されます。
オブジェクトに対しては複数のラベルを指定することができますが、各キーバリューペアは 1 つのオブジェクト内で一意である必要があります。
1 つのキーに対して複数の値が設定されていた場合、古い値は最後に書き込まれた値により上書きされます。

{% comment %}
### Key format recommendations
{% endcomment %}
{: #key-format-recommendations }
### 推奨されるキーの書式

{% comment %}
A label _key_ is the left-hand side of the key-value pair. Keys are alphanumeric
strings which may contain periods (`.`) and hyphens (`-`). Most Docker users use
images created by other organizations, and the following guidelines help to
prevent inadvertent duplication of labels across objects, especially if you plan
to use labels as a mechanism for automation.
{% endcomment %}
ラベルにおけるキーは、キーバリューペアの左側を指します。
キーに含めることができる文字は、英数字、ピリオド（`.`）、ハイフン（`-`）です。
Docker ユーザーが利用するイメージは、たいていは他の組織が作り出したものであるため、ここに示すガイドラインに従っていれば、オブジェクト間でのラベル定義を不用意に重複させるようなことがなくなります。
自動化の仕組みの中でラベルを利用する場合は、特にこのことが重要になります。

{% comment %}
- Authors of third-party tools should prefix each label key with the
  reverse DNS notation of a domain they own, such as `com.example.some-label`.
{% endcomment %}
- サードパーティ製ツールの開発者は、各ラベルのプリフィックスとして、自身が所有するドメインの逆 DNS 記法を用いるようにします。
たとえば `com.example.some-label` といったものです。

{% comment %}
- Do not use a domain in your label key without the domain owner's permission.
{% endcomment %}
- ドメイン所有者の許可なく、ラベルのキーにそのドメイン名を使ってはいけません。

{% comment %}
- The `com.docker.*`, `io.docker.*`, and `org.dockerproject.*` namespaces are
  reserved by Docker for internal use.
{% endcomment %}
- 以下の名前空間 `com.docker.*`, `io.docker.*`, `org.dockerproject.*` は、Docker が内部利用のために予約しています。

{% comment %}
- Label keys should begin and end with a lower-case letter and should only
  contain lower-case alphanumeric characters, the period character (`.`), and
  the hyphen character (`-`). Consecutive periods or hyphens are not allowed.
{% endcomment %}
- ラベルのキーの始まりと終わりの 1 文字は英小文字とします。
  そして文字列全体は、英小文字と数字、ピリオド（`.`）、ハイフン（`-`）を用いるようにします。
  そしてピリオドとハイフンは連続して用いないようにします。

{% comment %}
- The period character (`.`) separates namespace "fields". Label keys without
  namespaces are reserved for CLI use, allowing users of the CLI to interactively
  label Docker objects using shorter typing-friendly strings.
{% endcomment %}
- ピリオド（`.`）は名前空間の「項目」を区切るものです。
  ラベルのキーに名前空間が指定されていないものは、CLI が用いるものとしています。
  ユーザーにとって CLI 実行の際、Docker オブジェクトに対して入力しやすい短いラベル文字列を指定できるようにするためです。

{% comment %}
These guidelines are not currently enforced and additional guidelines may apply
to specific use cases.
{% endcomment %}
上のようなガイドラインは現時点において強制されるものではありません。
特定の用途において、さらに追加のガイドラインが適用されるかもしれません。

{% comment %}
### Value guidelines
{% endcomment %}
{: #value-guidelines }
### バリューに関するガイドライン

{% comment %}
Label values can contain any data type that can be represented as a string,
including (but not limited to) JSON, XML, CSV, or YAML. The only requirement is
that the value be serialized to a string first, using a mechanism specific to
the type of structure. For instance, to serialize JSON into a string, you might
use the `JSON.stringify()` JavaScript method.
{% endcomment %}
ラベルのバリューには、文字列として表現できるものであれば、どんな型のデータでも含めることができます。
たとえば JSON, XML, CSV, YAML があり、これ以外にもまだあります。
唯一必要になることは、そのデータ型の構造に従った形で、文字列としてシリアライズされたものであることです。
たとえば JSON データを文字列にシリアライズするには、JavaScript メソッドでは `JSON.stringify()` を利用するかもしれません。

{% comment %}
Since Docker does not deserialize the value, you cannot treat a JSON or XML
document as a nested structure when querying or filtering by label value unless
you build this functionality into third-party tooling.
{% endcomment %}
Docker ではそのバリューをデシリアライズしないため、ラベルを用いた検索やフィルタリングをする際には、ネスト構造になっている JSON や XML ドキュメントを取り扱うことはできません。
これを実現するためにはサードパーティ製のツール類に、そういった機能を組み入れる必要があります。

{% comment %}
## Manage labels on objects
{% endcomment %}
{: #manage-labels-on-objects }
## オブジェクトにおけるラベルの管理

{% comment %}
Each type of object with support for labels has mechanisms for adding and
managing them and using them as they relate to that type of object. These links
provide a good place to start learning about how you can use labels in your
Docker deployments.
{% endcomment %}
ラベルがサポートされている各オブジェクトには、ラベルの追加や管理を行う機能が備わっていて、そのオブジェクトに関連づいたラベルとして取り扱うことができます。
以下に示すリンクは、Docker デプロイにおいて利用するラベルを学ぶ上で役立つものです。

{% comment %}
Labels on images, containers, local daemons, volumes, and networks are static for
the lifetime of the object. To change these labels you must recreate the object.
Labels on swarm nodes and services can be updated dynamically.
{% endcomment %}
イメージ、コンテナー、ローカルデーモン、ボリューム、ネットワークといったオブジェクトにおいては、そのオブジェクトが存在する間、ラベルは静的で不変なものです。
ラベルを変更するためにはオブジェクトを再生成する必要があります。
Swarm ノードと Swarm サービスにおけるラベルは動的に変更することができます。

{% comment %}
- Images and containers
  - [Adding labels to images](../engine/reference/builder.md#label)
  - [Overriding a container's labels at runtime](../engine/reference/commandline/run.md#set-metadata-on-container--l---label---label-file)
  - [Inspecting labels on images or containers](../engine/reference/commandline/inspect.md)
  - [Filtering images by label](../engine/reference/commandline/images.md#filtering)
  - [Filtering containers by label](../engine/reference/commandline/ps.md#filtering)
{% endcomment %}
- イメージとコンテナー
  - [イメージへのラベル追加](../engine/reference/builder.md#label)
  - [実行時のコンテナーラベルの書き換え](../engine/reference/commandline/run.md#set-metadata-on-container--l---label---label-file)
  - [イメージやコンテナー上のラベル調査](../engine/reference/commandline/inspect.md)
  - [ラベルによるイメージのフィルタリング](../engine/reference/commandline/images.md#filtering)
  - [ラベルによるコンテナーのフィルタリング](../engine/reference/commandline/ps.md#filtering)

{% comment %}
- Local Docker daemons
  - [Adding labels to a Docker daemon at runtime](../engine/reference/commandline/dockerd.md)
  - [Inspecting a Docker daemon's labels](../engine/reference/commandline/info.md)
{% endcomment %}
- ローカルの Docker デーモン
  - [Docker デーモンへの実行時のラベル追加](../engine/reference/commandline/dockerd.md)
  - [Docker デーモンのラベル調査](../engine/reference/commandline/info.md)

{% comment %}
- Volumes
  - [Adding labels to volumes](../engine/reference/commandline/volume_create.md)
  - [Inspecting a volume's labels](../engine/reference/commandline/volume_inspect.md)
  - [Filtering volumes by label](../engine/reference/commandline/volume_ls.md#filtering)
{% endcomment %}
- ボリューム
  - [ボリュームへのラベル追加](../engine/reference/commandline/volume_create.md)
  - [ボリュームのラベル調査](../engine/reference/commandline/volume_inspect.md)
  - [ラベルによるボリュームのフィルタリング](../engine/reference/commandline/volume_ls.md#filtering)

{% comment %}
- Networks
  - [Adding labels to a network](../engine/reference/commandline/network_create.md)
  - [Inspecting a network's labels](../engine/reference/commandline/network_inspect.md)
  - [Filtering networks by label](../engine/reference/commandline/network_ls.md#filtering)
{% endcomment %}
- ネットワーク
  - [ネットワークへのラベル追加](../engine/reference/commandline/network_create.md)
  - [ネットワークラベルの調査](../engine/reference/commandline/network_inspect.md)
  - [ラベルによるネットワークのフィルタリング](../engine/reference/commandline/network_ls.md#filtering)

{% comment %}
- Swarm nodes
  - [Adding or updating a swarm node's labels](../engine/reference/commandline/node_update.md#add-label-metadata-to-a-node)
  - [Inspecting a swarm node's labels](../engine/reference/commandline/node_inspect.md)
  - [Filtering swarm nodes by label](../engine/reference/commandline/node_ls.md#filtering)
{% endcomment %}
- Swarm ノード
  - [Swarm ノードのラベル追加または更新](../engine/reference/commandline/node_update.md#add-label-metadata-to-a-node)
  - [Swarm ノードのラベル調査](../engine/reference/commandline/node_inspect.md)
  - [ラベルによる Swarm ノードのフィルタリング](../engine/reference/commandline/node_ls.md#filtering)

{% comment %}
- Swarm services
  - [Adding labels when creating a swarm service](../engine/reference/commandline/service_create.md#set-metadata-on-a-service-l-label)
  - [Updating a swarm service's labels](../engine/reference/commandline/service_update.md)
  - [Inspecting a swarm service's labels](../engine/reference/commandline/service_inspect.md)
  - [Filtering swarm services by label](../engine/reference/commandline/service_ls.md#filtering)
{% endcomment %}
- Swarm サービス
  - [Swarm サービス生成時のラベル追加](../engine/reference/commandline/service_create.md#set-metadata-on-a-service-l-label)
  - [Swarm サービスのラベル更新](../engine/reference/commandline/service_update.md)
  - [Swarm サービスのラベル調査](../engine/reference/commandline/service_inspect.md)
  - [ラベルによる Swarm サービスのフィルタリング](../engine/reference/commandline/service_ls.md#filtering)
