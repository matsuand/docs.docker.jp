{% comment %}
This table shows which Compose file versions support specific Docker releases.
{% endcomment %}
以下の一覧表は、Compose ファイルのどのバージョンが Docker リリースをサポートしているかを示すものです。

{% comment %}
| **Compose file format** | **Docker Engine release** |
|  -------------------    |    ------------------     |
{% endcomment %}
| **Compose ファイルフォーマット** | **Docker Engine リリース** |
|  -------------------    |    ------------------     |
|      3.8                |       19.03.0 以上        |
|      3.7                |       18.06.0 以上        |
|      3.6                |       18.02.0 以上        |
|      3.5                |       17.12.0 以上        |
|      3.4                |       17.09.0 以上        |
|      3.3                |       17.06.0 以上        |
|      3.2                |       17.04.0 以上        |
|      3.1                |       1.13.1 以上         |
|      3.0                |       1.13.0 以上         |
|      2.4                |       17.12.0 以上        |
|      2.3                |       17.06.0 以上        |
|      2.2                |       1.13.0 以上         |
|      2.1                |       1.12.0 以上         |
|      2.0                |       1.10.0 以上         |
|      1.0                |       1.9.1. 以上         |

{% comment %}
In addition to Compose file format versions shown in the table, the Compose
itself is on a release schedule, as shown in [Compose
releases](https://github.com/docker/compose/releases/), but file format versions
do not necessarily increment with each release. For example, Compose file format
3.0 was first introduced in [Compose release
1.10.0](https://github.com/docker/compose/releases/tag/1.10.0), and versioned
gradually in subsequent releases.
{% endcomment %}
この表に示している Compose のファイルフォーマットバージョンに加えて、Compose そのもののリリーススケジュールがあります。
詳しくは [Compose リリース](https://github.com/docker/compose/releases/)に示されています。
しかしファイルフォーマットのバージョンは、個々のリリースに合わせてバージョンアップする必要がありません。
たとえば Compose ファイルフォーマット 3.0 が初めて導入されたのは  [Compose リリース 1.10.0](https://github.com/docker/compose/releases/tag/1.10.0) ですが、その後のリリースの中で、徐々にバージョンアップを行ってきています。