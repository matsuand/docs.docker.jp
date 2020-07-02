---
description: How to set up and use certificates with a registry to verify access
keywords: Usage, registry, repository, client, root, certificate, docker, apache, ssl, tls, documentation, examples, articles, tutorials
redirect_from:
- /engine/articles/certificates/
title: 証明書を使ったリポジトリクライアントの確認
---

{% comment %}
In [Running Docker with HTTPS](https.md), you learned that, by default,
Docker runs via a non-networked Unix socket and TLS must be enabled in order
to have the Docker client and the daemon communicate securely over HTTPS.  TLS ensures authenticity of the registry endpoint and that traffic to/from registry is encrypted.
{% endcomment %}
[HTTPS による Docker 起動](https.md) において学んだことは、デフォルトにおいて Docker はインターネット通信ではない Unix ソケットを通じて動作しているということでした。
そして Docker クライアントとデーモンとの間で HTTPS を介して安全なやり取りとするためには TLS を有効にしなければならないということでした。
TLS はレジストリエンドポイントが信頼できるものであることを確実にし、レジストリとの間のトラフィックは暗号化してくれます。

{% comment %}
This article demonstrates how to ensure the traffic between the Docker registry
server and the Docker daemon (a client of the registry server) is encrypted and
properly authenticated using *certificate-based client-server authentication*.
{% endcomment %}
本文では
This article demonstrates how to ensure the traffic between the Docker registry
server and the Docker daemon (a client of the registry server) is encrypted and
properly authenticated using *certificate-based client-server authentication*.

{% comment %}
We show you how to install a Certificate Authority (CA) root certificate
for the registry and how to set the client TLS certificate for verification.
{% endcomment %}
We show you how to install a Certificate Authority (CA) root certificate
for the registry and how to set the client TLS certificate for verification.

{% comment %}
## Understand the configuration
{% endcomment %}
{: #understand-the-configuration }
## 設定内容の理解

{% comment %}
A custom certificate is configured by creating a directory under
`/etc/docker/certs.d` using the same name as the registry's hostname, such as
`localhost`. All `*.crt` files are added to this directory as CA roots.
{% endcomment %}
カスタム証明書は `/etc/docker/certs.d` ディレクトリ配下に新たなディレクトリを生成して、そこに設定ファイルを置きます。
ディレクトリ名はレジストリのホスト名と同一に、たとえば `localhost` のようにします。
`*.crt` ファイルはすべて、CA ルートとしてこのディレクトリに追加していきます。

{% comment %}
> **Note**:
> As of Docker 1.13, on Linux any root certificates authorities are merged
> with the system defaults, including as the host's root CA set. On prior
versions of Docker, and on Docker Enterprise Edition for Windows Server,
> the system default certificates are only used when no custom root certificates
> are configured.
{% endcomment %}
> **メモ**:
> As of Docker 1.13, on Linux any root certificates authorities are merged
> with the system defaults, including as the host's root CA set. On prior
versions of Docker, and on Docker Enterprise Edition for Windows Server,
> the system default certificates are only used when no custom root certificates
> are configured.

{% comment %}
The presence of one or more `<filename>.key/cert` pairs indicates to Docker
that there are custom certificates required for access to the desired
repository.
{% endcomment %}
1 つでも `<filename>.key/cert` のペアがあるということは、そのリポジトリに対するアクセスにはカスタム証明書が必要であることを Docker に伝えるものです。

{% comment %}
> **Note**:
> If multiple certificates exist, each is tried in alphabetical
> order. If there is a 4xx-level or 5xx-level authentication error, Docker
> continues to try with the next certificate.
{% endcomment %}
> **メモ**:
> 複数の証明書が存在していた場合、その処理はアルファベット順に行われます。
> 4xx や 5xx のレベルの認証エラーがあると、Docker はその次の証明書を使った処理を試します。

{% comment %}
The following illustrates a configuration with custom certificates:
{% endcomment %}
以下は、複数のカスタム証明書がある場合の設定例です。

{% comment %}
```
    /etc/docker/certs.d/        <-- Certificate directory
    └── localhost:5000          <-- Hostname:port
       ├── client.cert          <-- Client certificate
       ├── client.key           <-- Client key
       └── ca.crt               <-- Certificate authority that signed
                                    the registry certificate
```
{% endcomment %}
```
    /etc/docker/certs.d/        <-- 証明書のディレクトリ
    └── localhost:5000          <-- ホスト名：ポート
       ├── client.cert          <-- クライアント証明書
       ├── client.key           <-- クライアント鍵
       └── ca.crt               <-- Certificate authority that signed
                                    the registry certificate
```

{% comment %}
The preceding example is operating-system specific and is for illustrative
purposes only. You should consult your operating system documentation for
creating an os-provided bundled certificate chain.
{% endcomment %}
The preceding example is operating-system specific and is for illustrative
purposes only. You should consult your operating system documentation for
creating an os-provided bundled certificate chain.


{% comment %}
## Create the client certificates
{% endcomment %}
{: #create-the-client-certificates }
## クライアント証明書の生成

{% comment %}
Use OpenSSL's `genrsa` and `req` commands to first generate an RSA
key and then use the key to create the certificate.
{% endcomment %}
OpenSSL の `genrsa` コマンドと `req` コマンドを使って、まずは RSA 鍵を生成します。
そしてこの鍵を使って証明書を生成します。

    $ openssl genrsa -out client.key 4096
    $ openssl req -new -x509 -text -key client.key -out client.cert

{% comment %}
> **Note**:
> These TLS commands only generate a working set of certificates on Linux.
> The version of OpenSSL in macOS is incompatible with the type of
> certificate Docker requires.
{% endcomment %}
> **メモ**:
> この TLS コマンドが生成するのは Linux 上において動作する証明書です。
> macOS における OpenSSL バージョンは、Docker が必要とする種類の証明書のタイプとは互換性がありません。

{% comment %}
## Troubleshooting tips
{% endcomment %}
## Troubleshooting tips

{% comment %}
The Docker daemon interprets `.crt` files as CA certificates and `.cert` files
as client certificates. If a CA certificate is accidentally given the extension
`.cert` instead of the correct `.crt` extension, the Docker daemon logs the
following error message:
{% endcomment %}
The Docker daemon interprets `.crt` files as CA certificates and `.cert` files
as client certificates. If a CA certificate is accidentally given the extension
`.cert` instead of the correct `.crt` extension, the Docker daemon logs the
following error message:

```
Missing key KEY_NAME for client certificate CERT_NAME. CA certificates should use the extension .crt.
```

{% comment %}
If the Docker registry is accessed without a port number, do not add the port to the directory name.  The following shows the configuration for a registry on default port 443 which is accessed with `docker login my-https.registry.example.com`:
{% endcomment %}
If the Docker registry is accessed without a port number, do not add the port to the directory name.  The following shows the configuration for a registry on default port 443 which is accessed with `docker login my-https.registry.example.com`:

```
    /etc/docker/certs.d/
    └── my-https.registry.example.com          <-- Hostname without port
       ├── client.cert
       ├── client.key
       └── ca.crt
```

{% comment %}
## Related information
{% endcomment %}
{: #related-information }
## 関連情報

{% comment %}
* [Use trusted images](index.md)
* [Protect the Docker daemon socket](https.md)
{% endcomment %}
* [Use trusted images](index.md)
* [Protect the Docker daemon socket](https.md)
