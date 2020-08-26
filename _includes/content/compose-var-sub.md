{% comment %}
Your configuration options can contain environment variables. Compose uses the
variable values from the shell environment in which `docker-compose` is run. For
example, suppose the shell contains `POSTGRES_VERSION=9.3` and you supply this
configuration:
{% endcomment %}
設定オプションには環境変数を含めることができます。
Compose は `docker-compose` が実行されたシェル環境から変数値を取得します。
たとえばシェル環境に `POSTGRES_VERSION=9.3` が定義されているとして、以下のような設定を行ったとします。

```yaml
db:
  image: "postgres:${POSTGRES_VERSION}"
```

{% comment %}
When you run `docker-compose up` with this configuration, Compose looks for the
`POSTGRES_VERSION` environment variable in the shell and substitutes its value
in. For this example, Compose resolves the `image` to `postgres:9.3` before
running the configuration.
{% endcomment %}
上の設定を使って `docker-compose up` を実行すると、Compose はシェル環境内の環境変数 `POSTGRES_VERSION` を見にいき、その値を使って設定を置換します。
この例では `image` が `postgres:9.3` と置換された上で、設定に関する処理へ進みます。

{% comment %}
If an environment variable is not set, Compose substitutes with an empty
string. In the example above, if `POSTGRES_VERSION` is not set, the value for
the `image` option is `postgres:`.
{% endcomment %}
その環境変数が何も設定されていなかった場合、Compose は空文字に置換します。
上の例でいうと `POSTGRES_VERSION` が設定されていなかったとき、`image` オプションの値は `postgres:` になります。

{% comment %}
You can set default values for environment variables using a
[`.env` file](/compose/env-file/), which Compose automatically looks for. Values
set in the shell environment override those set in the `.env` file.
{% endcomment %}
環境変数のデフォルト値を [`.env` ファイル](/compose/env-file/) に設定しておくことができます。
Compose はこのファイルを自動的に探しにいきます。
`.env` ファイルで定義されたものよりも、シェル環境で設定された値が優先されます。

{% comment %}
> Note when using docker stack deploy
>
> The `.env file` feature only works when you use the `docker-compose up` command
> and does not work with `docker stack deploy`.
{: .important }
{% endcomment %}
> `.env` ファイルによる機能は `docker-compose up` コマンドを使うときだけ動作します。
> `docker stack deploy` コマンドでは動作しません。
{: .important }

{% comment %}
Both `$VARIABLE` and `${VARIABLE}` syntax are supported. Additionally when using
the [2.1 file format](/compose/compose-file/compose-versioning/#version-21), it is possible to
provide inline default values using typical shell syntax:
{% endcomment %}
`$VARIABLE` と `${VARIABLE}` という 2 つの文法がともにサポートされます。
さらに [2.1 ファイルフォーマット](/compose/compose-file/compose-versioning/#version-21) を利用している場合は、シェル上でも利用されているインラインのデフォルト指定方法を行うことができます。

{% comment %}
- `${VARIABLE:-default}` evaluates to `default` if `VARIABLE` is unset or
  empty in the environment.
- `${VARIABLE-default}` evaluates to `default` only if `VARIABLE` is unset
  in the environment.
{% endcomment %}
- `${VARIABLE:-default}` は `VARIABLE` がセットされていないか空文字であるときに `default` として評価されます。
- `${VARIABLE-default}` は `VARIABLE` がセットされていないときのみ `default` として評価されます。

{% comment %}
Similarly, the following syntax allows you to specify mandatory variables:
{% endcomment %}
同様に以下の文法により、変数を必須とする扱いができます。

{% comment %}
- `${VARIABLE:?err}` exits with an error message containing `err` if
  `VARIABLE` is unset or empty in the environment.
- `${VARIABLE?err}` exits with an error message containing `err` if
  `VARIABLE` is unset in the environment.
{% endcomment %}
- `${VARIABLE:?err}` は `VARIABLE` がセットされていないか空文字であるときに、`err` を含むエラーメッセージを表示して終了します。
- `${VARIABLE?err}` は `VARIABLE` がセットされていないときに、`err` を含むエラーメッセージを表示して終了します。

{% comment %}
Other extended shell-style features, such as `${VARIABLE/foo/bar}`, are not
supported.
{% endcomment %}
シェル風のこの他の機能、たとえば `${VARIABLE/foo/bar}` などはサポートされていません。

{% comment %}
You can use a `$$` (double-dollar sign) when your configuration needs a literal
dollar sign. This also prevents Compose from interpolating a value, so a `$$`
allows you to refer to environment variables that you don't want processed by
Compose.
{% endcomment %}
設定ファイル内にドル記号そのものが必要な場合は `$$`（ドル記号 2 つ）を書きます。
この記述は Compose が値を取り違えないようにします。
つまり `$$` と書くことで、環境変数を参照しつつ Compose には処理させないようにすることができます。

```yaml
web:
  build: .
  command: "$$VAR_NOT_INTERPOLATED_BY_COMPOSE"
```

{% comment %}
If you forget and use a single dollar sign (`$`), Compose interprets the value
as an environment variable and warns you:
{% endcomment %}
このことを忘れてドル記号 1 つ（`$`）を用いてしまうと、Compose は環境変数として値を解釈し、以下の警告を出します。

{% comment %}
`The VAR_NOT_INTERPOLATED_BY_COMPOSE is not set. Substituting an empty string.`
{% endcomment %}
`The VAR_NOT_INTERPOLATED_BY_COMPOSE is not set. Substituting an empty string.`
（VAR_NOT_INTERPOLATED_BY_COMPOSE は設定されていません。空文字として置換します。）
