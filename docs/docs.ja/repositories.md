# レポジトリ

## PyPIレポジトリの使用

デフォルトでは、Poetryはパッケージのインストールと公開に [PyPI](https://pypi.org) レポジトリを使うよう設定されます。

そのため、プロジェクトに依存関係を追加すると、PoetryはそれがPyPIで利用可能だと仮定します。

これはほとんどの場合に当たり、ほとんどのユーザーにとって十分なようです。


## プライベートレポジトリの使用

しかし、あるときは、チームメイトには共有可能にしつつ、パッケージを非公開のままにする必要があるかもしれません。
その場合、プライベートレポジトリを使う必要があります。

### レポジトリの追加

レポジトリの追加は `config` コマンドを使うと簡単です。

```bash
poetry config repositories.foo https://foo.bar/simple/
```

このコマンドは、レポジトリ `foo` のURLを `https://foo.bar/simple/` に設定します。

### 認証情報の設定

もし特定のレポジトリの認証情報を保存したいなら、次のように簡単にできます:

```bash
poetry config http-basic.foo username password
```

パスワードを指定しなかった場合は、パスワードを書くように促されます。

!!!注意

    PyPIへ公開するために、 `pypi` という名前のレポジトリの認証情報を
    設定できます:

    Note that it is recommended to use [API tokens](https://pypi.org/help/#apitoken)
    when uploading packages to PyPI.
    Once you have created a new token, you can tell Poetry to use it:

    ```bash
    poetry config pypi-token.pypi my-token
    ```

    If you still want to use you username and password, you can do so with the following
    call to `config`.

    ```bash
    poetry config http-basic.pypi username password
    ```

`pyblish` コマンドを使うときに `--username` オプションと `--password`
オプションを付けて、ユーザー名とパスワードを指定することもできます。

If a system keyring is available and supported, the password is stored to
and retrieved from the keyring. In the above example, the credential will be
stored using the name `poetry-repository-pypi`. If access to keyring fails
or is unsupported, this will fall back to writing the password to the
`auth.toml` file along with the username.

Keyring support is enabled using the [keyring
library](https://pypi.org/project/keyring/). For more information on
supported backends refer to the [library
documentation](https://keyring.readthedocs.io/en/latest/?badge=latest).

Alternatively, you can use environment variables to provide the credentials:

```bash
export POETRY_PYPI_TOKEN_PYPI=my-token
export POETRY_HTTP_BASIC_PYPI_USERNAME=username
export POETRY_HTTP_BASIC_PYPI_PASSWORD=password
```

See [Using environment
variables](/docs/configuration/#using-environment-variables) for more
information on how to configure Poetry with environment variables.

#### Custom certificate authority and mutual TLS authentication
Poetry supports repositories that are secured by a custom certificate
authority as well as those that require certificate-based client
authentication.  The following will configure the "foo" repository to
validate the repository's certificate using a custom certificate authority
and use a client certificate (note that these config variables do not both
need to be set):
```bash
    poetry config certificates.foo.cert /path/to/ca.pem
    poetry config certificates.foo.client-cert /path/to/client.pem
```

### プライベートレポジトリからの依存関係インストール

プライベートレポジトリへの公開ができようになったので、そこから依存関係をインストールできるようにする必要があります。

そのためには、 `pyproject.toml` ファイルを次のように編集する必要があります

```toml
[[tool.poetry.source]]
name = "foo"
url = "https://foo.bar/simple/"
```

今後は、Poetryはプライベートレポジトリのパッケージも検索します。

!!!注意

    Any custom repository will have precedence over PyPI.

    If you still want PyPI to be your primary source for your packages
    you can declare custom repositories as secondary.

    ```toml
    [[tool.poetry.source]]
    name = "foo"
    url = "https://foo.bar/simple/"
    secondary = true
    ```

プライベートレポジトリがHTTP Basic認証を要求する場合、上の例を使って、忘れずにユーザー名とパスワードを `http-basic`
設定に追加してください (`tool.poetry.source` セクションにある名前と同じものを使うことも忘れないでください)。
レポジトリにカスタムの認証局かクライアント証明書が必要な場合も同様に上の例を参照して `certificates` セクションを設定してください。
Poetryは、パッケージのダウンロードや検索をするときに、これらの値をプライベートレポジトリに対する認証に使います。


### PyPIレポジトリの無効化

If you want your packages to be exclusively looked up from a private
repository, you can set it as the default one by using the `default` keyword

```toml
[[tool.poetry.source]]
name = "foo"
url = "https://foo.bar/simple/"
default = true
```

A default source will also be the fallback source if you add other sources.
