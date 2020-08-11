---
description: HTTPS により Docker を起動します。
keywords: docker, docs, article, example, https, daemon, tls, ca,  certificate
redirect_from:
- /engine/articles/https/
- /articles/https/
title: Docker デーモンソケットの保護
---

{% comment %}
By default, Docker runs through a non-networked UNIX socket. It can also
optionally communicate using an HTTP socket.
{% endcomment %}
デフォルトで Docker は、インターネットを介さない UNIX ソケットを通じて実行されます。
HTTPS ソケットを用いた通信を行うこともできます。

{% comment %}
If you need Docker to be reachable through the network in a safe manner, you can
enable TLS by specifying the `tlsverify` flag and pointing Docker's
`tlscacert` flag to a trusted CA certificate.
{% endcomment %}
Docker がネットワークから接続される際に安全性を確保するには、`tlsverify` フラグを指定して TLS を有効にし、`tlscacert` フラグを使って信頼された CA 証明書を指定します。

{% comment %}
In the daemon mode, it only allows connections from clients
authenticated by a certificate signed by that CA. In the client mode,
it only connects to servers with a certificate signed by that CA.
{% endcomment %}
デーモンモードにおいては、CA によって署名された証明書を用いて認証されたクライアントからのみ、接続を許可します。
クライアントモードでは、その CA によって署名された証明書を利用するサーバーに対してのみ、接続を可能にします。

{% comment %}
> Advanced topic
>
> Using TLS and managing a CA is an advanced topic. Please familiarize yourself
> with OpenSSL, x509, and TLS before using it in production.
{:.important}
{% endcomment %}
> 高度なトピック
>
> TLS 利用と CA 管理は高度なトピックです。
> これを本番環境に利用する場合は、OpenSSL、x509、TLS についてよく理解してから行ってください。
{:.important}

{% comment %}
## Create a CA, server and client keys with OpenSSL
{% endcomment %}
{: #create-a-ca-server-and-client-keys-with-openssl }
## OpenSSL を用いた CA、サーバー鍵、クライアント鍵の生成

{% comment %}
> **Note**: Replace all instances of `$HOST` in the following example with the
> DNS name of your Docker daemon's host.
{% endcomment %}
> **メモ**:
> 以下に示す例において `$HOST` と示されている箇所はすべて、利用している Docker デーモンホストの DNS 名に置き換えてください。

{% comment %}
First, on the **Docker daemon's host machine**, generate CA private and public keys:
{% endcomment %}
まず **Docker デーモンが起動するホストマシン** において、CA 秘密鍵と公開鍵を生成します。

    $ openssl genrsa -aes256 -out ca-key.pem 4096
    Generating RSA private key, 4096 bit long modulus
    ............................................................................................................................................................................................++
    ........++
    e is 65537 (0x10001)
    Enter pass phrase for ca-key.pem:
    Verifying - Enter pass phrase for ca-key.pem:

    $ openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
    Enter pass phrase for ca-key.pem:
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    State or Province Name (full name) [Some-State]:Queensland
    Locality Name (eg, city) []:Brisbane
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:Docker Inc
    Organizational Unit Name (eg, section) []:Sales
    Common Name (e.g. server FQDN or YOUR name) []:$HOST
    Email Address []:Sven@home.org.au

{% comment %}
Now that you have a CA, you can create a server key and certificate
signing request (CSR). Make sure that "Common Name" matches the hostname you use
to connect to Docker:
{% endcomment %}
CA を生成したので、次にサーバー鍵と証明書署名要求（certificate signing request; CSR）を生成します。
「Common Name」欄には、Docker に接続するホストの名前となっていることを確認してください。

{% comment %}
> **Note**: Replace all instances of `$HOST` in the following example with the
> DNS name of your Docker daemon's host.
{% endcomment %}
> **メモ**:
> 以下に示す例において `$HOST` と示されている箇所はすべて、利用している Docker デーモンホストの DNS 名に置き換えてください。

    $ openssl genrsa -out server-key.pem 4096
    Generating RSA private key, 4096 bit long modulus
    .....................................................................++
    .................................................................................................++
    e is 65537 (0x10001)

    $ openssl req -subj "/CN=$HOST" -sha256 -new -key server-key.pem -out server.csr

{% comment %}
Next, we're going to sign the public key with our CA:
{% endcomment %}
次に公開鍵を CA を使って署名します。

{% comment %}
Since TLS connections can be made through IP address as well as DNS name, the IP addresses
need to be specified when creating the certificate. For example, to allow connections
using `10.10.10.20` and `127.0.0.1`:
{% endcomment %}
TLS 接続は DNS 名だけでなく IP アドレスを使っても行われるため、証明書の生成時には IP アドレスが必要になります。
たとえば `10.10.10.20` と `127.0.0.1` を使って接続を許可するには、以下のようにします。

    $ echo subjectAltName = DNS:$HOST,IP:10.10.10.20,IP:127.0.0.1 >> extfile.cnf

{% comment %}
Set the Docker daemon key's extended usage attributes to be used only for
server authentication:
{% endcomment %}
Docker デーモンの拡張属性は、サーバー認証に対してのみ利用するものとして設定します。

    $ echo extendedKeyUsage = serverAuth >> extfile.cnf

{% comment %}
Now, generate the signed certificate:
{% endcomment %}
そこで署名された証明書を生成します。

    $ openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem \
      -CAcreateserial -out server-cert.pem -extfile extfile.cnf
    Signature ok
    subject=/CN=your.host.com
    Getting CA Private Key
    Enter pass phrase for ca-key.pem:

{% comment %}
[Authorization plugins](/engine/extend/plugins_authorization/) offer more
fine-grained control to supplement authentication from mutual TLS. In addition
to other information described in the above document, authorization plugins
running on a Docker daemon receive the certificate information for connecting
Docker clients.
{% endcomment %}
[認証プラグイン](/engine/extend/plugins_authorization/) は、相互 TLS からの認証を補完する、きめ細かな制御を可能にします。
上記のドキュメント内の説明内容に加えて、Docker デーモン上で動作する認証プラグインは、Docker クライアントに接続するための認証情報を受け取ります。

{% comment %}
For client authentication, create a client key and certificate signing
request:
{% endcomment %}
クライアント認証に対しては、クライアント鍵と証明書署名要求を生成します。

{% comment %}
> **Note**: For simplicity of the next couple of steps, you may perform this
> step on the Docker daemon's host machine as well.
{% endcomment %}
> **メモ**: ここから続く手順を簡単にするために、以下の手順は Docker デーモンが稼動するホストマシン上で行ってもかまいません。

    $ openssl genrsa -out key.pem 4096
    Generating RSA private key, 4096 bit long modulus
    .........................................................++
    ................++
    e is 65537 (0x10001)

    $ openssl req -subj '/CN=client' -new -key key.pem -out client.csr

{% comment %}
To make the key suitable for client authentication, create a new extensions
config file:
{% endcomment %}
生成した鍵をクライアント認証用とするために、新たな拡張設定ファイルを生成します。

    $ echo extendedKeyUsage = clientAuth > extfile-client.cnf

{% comment %}
Now, generate the signed certificate:
{% endcomment %}
そこで署名された証明書を生成します。

    $ openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem \
      -CAcreateserial -out cert.pem -extfile extfile-client.cnf
    Signature ok
    subject=/CN=client
    Getting CA Private Key
    Enter pass phrase for ca-key.pem:

{% comment %}
After generating `cert.pem` and `server-cert.pem` you can safely remove the
two certificate signing requests and extensions config files:
{% endcomment %}
`cert.pem` と `server-cert.pem` を生成したら、証明書署名要求と拡張設定ファイルの 2 つは、安全に削除することができます。

    $ rm -v client.csr server.csr extfile.cnf extfile-client.cnf

{% comment %}
With a default `umask` of 022, your secret keys are *world-readable* and
writable for you and your group.
{% endcomment %}
`umask` をデフォルトの 022 のまま使ってしまうと、秘密鍵は **誰もが読み込み可能** となり、また所有者とグループが書き込み可能となってしまいます。

{% comment %}
To protect your keys from accidental damage, remove their
write permissions. To make them only readable by you, change file modes as follows:
{% endcomment %}
秘密鍵を保護し、予期しない被害を受けないために、書き込み権限は削除してください。
読み込み権限は所有者のみとするように、以下のようにしてファイルモードの変更を行います。

    $ chmod -v 0400 ca-key.pem key.pem server-key.pem

{% comment %}
Certificates can be world-readable, but you might want to remove write access to
prevent accidental damage:
{% endcomment %}
証明書は誰でも読み込めるようにするのでもかまいません。
ただし書き込み権限は、被害を避ける意味で削除するようにしてください。

    $ chmod -v 0444 ca.pem server-cert.pem cert.pem

{% comment %}
Now you can make the Docker daemon only accept connections from clients
providing a certificate trusted by your CA:
{% endcomment %}
このようにして Docker デーモンが接続を受け入れるクライアントは、CA に信頼された証明書を利用するクライアントのみとすることができました。

    $ dockerd --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem \
      -H=0.0.0.0:2376

{% comment %}
To connect to Docker and validate its certificate, provide your client keys,
certificates and trusted CA:
{% endcomment %}
Docker に接続して証明書を確認します。
クライアント鍵、証明書、信頼された CA を指定してください。

{% comment %}
> Run it on the client machine
>
> This step should be run on your Docker client machine. As such, you
> need to copy your CA certificate, your server certificate, and your client
> certificate to that machine.
{% endcomment %}
> クライアントマシン上での実行
>
> ここでの手順は Docker クライアントマシン上で行います。
> したがって CA 証明書、サーバー証明書、クライアント証明書は、そのマシン上にコピーしておく必要があります。

{% comment %}
> **Note**: Replace all instances of `$HOST` in the following example with the
> DNS name of your Docker daemon's host.
{% endcomment %}
> **メモ**:
> 以下に示す例において `$HOST` と示されている箇所はすべて、利用している Docker デーモンホストの DNS 名に置き換えてください。

    $ docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem \
      -H=$HOST:2376 version

{% comment %}
> **Note**:
> Docker over TLS should run on TCP port 2376.
{% endcomment %}
> **メモ**:
> Docker over TLS は TCP ポート 2376 上を使って動作させる必要があります。

{% comment %}
> **Warning**:
> As shown in the example above, you don't need to run the `docker` client
> with `sudo` or the `docker` group when you use certificate authentication.
> That means anyone with the keys can give any instructions to your Docker
> daemon, giving them root access to the machine hosting the daemon. Guard
> these keys as you would a root password!
{:.warning}
{% endcomment %}
> **警告**
> 上の例に示したように、証明書認証操作を行う際の `docker` クライアント実行において `sudo` を使ったり `docker` グループに属していたりする必要はありません。
> つまり鍵を使うのであれば、Docker デーモンに対して指示を出すのは、デーモンホストのマシンに root 権限を持っていれば誰でもよいということです。
> したがってこの鍵データは root パスワードと同じように、しっかりと管理してください。
{:.warning}

{% comment %}
## Secure by default
{% endcomment %}
{: #secure-by-default }
## セキュアな接続のデフォルト設定

{% comment %}
If you want to secure your Docker client connections by default, you can move
the files to the `.docker` directory in your home directory --- and set the
`DOCKER_HOST` and `DOCKER_TLS_VERIFY` variables as well (instead of passing
`-H=tcp://$HOST:2376` and `--tlsverify` on every call).
{% endcomment %}
Docker クライアント接続を、デフォルトで安全なものとしたい場合は、ホームディレクトリ内の `.docker` ディレクトリに、各ファイルを移動させます。
これに合わせて `DOCKER_HOST` と `DOCKER_TLS_VERIFY` の変数も設定します
（これはコマンド実行時に `-H=tcp://$HOST:2376` と `--tlsverify` を指定しない代わりとして行うものです）。

    $ mkdir -pv ~/.docker
    $ cp -v {ca,cert,key}.pem ~/.docker

    $ export DOCKER_HOST=tcp://$HOST:2376 DOCKER_TLS_VERIFY=1

{% comment %}
Docker now connects securely by default:
{% endcomment %}
Docker はデフォルトで安全な接続を行うようになります。

    $ docker ps

{% comment %}
## Other modes
{% endcomment %}
{: #other-modes }
## その他のモード

{% comment %}
If you don't want to have complete two-way authentication, you can run
Docker in various other modes by mixing the flags.
{% endcomment %}
完全な双方向認証は行う必要がない場合は、他にもあるさまざまなモードや各種フラグを組み合わせて Docker を実行することができます。

{% comment %}
### Daemon modes
{% endcomment %}
{: #daemon-modes }
### デーモンモード

 {% comment %}
 - `tlsverify`, `tlscacert`, `tlscert`, `tlskey` set: Authenticate clients
 - `tls`, `tlscert`, `tlskey`: Do not authenticate clients
 {% endcomment %}
 - `tlsverify`、`tlscacert`、`tlscert`、`tlskey` の各設定は、クライアント認証を行います。
 - `tls`、`tlscert`、`tlskey`はクライアント認証を行いません。

{% comment %}
### Client modes
{% endcomment %}
{: #client-modes }
### クライアントモード

 {% comment %}
 - `tls`: Authenticate server based on public/default CA pool
 - `tlsverify`, `tlscacert`: Authenticate server based on given CA
 - `tls`, `tlscert`, `tlskey`: Authenticate with client certificate, do not
   authenticate server based on given CA
 - `tlsverify`, `tlscacert`, `tlscert`, `tlskey`: Authenticate with client
   certificate and authenticate server based on given CA
 {% endcomment %}
 - `tls` は、公開またはデフォルトの CA プールに基づくサーバーを認証します。
 - `tlsverify`、`tlscacert` は、指定された CA に基づくサーバーを認証します。
 - `tls`、`tlscert``、`tlskey` は、クライアント証明書を使って認証します。指定の CA に基づたサーバー認証は行いません。
 - `tlsverify`、`tlscacert`、`tlscert`、`tlskey` は、クライアント証明書を使って認証します。
   そして指定の CA に基づいたサーバー認証を行います。

{% comment %}
If found, the client sends its client certificate, so you just need
to drop your keys into `~/.docker/{ca,cert,key}.pem`. Alternatively,
if you want to store your keys in another location, you can specify that
location using the environment variable `DOCKER_CERT_PATH`.
{% endcomment %}
クライアントに証明書があればクライアントはそれを送信するので、鍵データは `~/.docker/{ca,cert,key}.pem` に配置しておくことが必要です。
あるいは鍵データを別のディレクトリに保持しておきたい場合は、環境変数 `DOCKER_CERT_PATH` にそのディレクトリを指定します。

    $ export DOCKER_CERT_PATH=~/.docker/zone1/
    $ docker --tlsverify ps

{% comment %}
### Connecting to the secure Docker port using `curl`
{% endcomment %}
{: #connecting-to-the-secure-docker-port-using-curl }
### `curl` を用いたセキュアな Docker ポートへの接続

{% comment %}
To use `curl` to make test API requests, you need to use three extra command line
flags:
{% endcomment %}
`curl` を使って API リクエストを行ってみるなら、指定を 3 つ追加したコマンドライン実行を行います。

    $ curl https://$HOST:2376/images/json \
      --cert ~/.docker/cert.pem \
      --key ~/.docker/key.pem \
      --cacert ~/.docker/ca.pem

{% comment %}
## Related information
{% endcomment %}
{: #related-information }
## 関連情報

{% comment %}
* [Using certificates for repository client verification](certificates.md)
* [Use trusted images](trust/index.md)
{% endcomment %}
* [証明書を使ったリポジトリクライアントの確認](certificates.md)
* [信頼できるイメージの利用](trust/index.md)
