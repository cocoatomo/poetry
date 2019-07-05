# `pyproject.toml` ファイル

`pyproject.toml` ファイルの `tool.poetry` 節は複数の節で構成されています。

## name

パッケージ名。
**必須**

## version

パッケージのバージョン。 **必須**

これは [semantic versioning](http://semver.org/) に従うべきです。
ただし、強制はしませんし、他の仕様に従う自由も残されています。

## description

パッケージの短い説明。 **必須**

## license

パッケージのライセンス。

最もよく使われるライセンスの推奨される記法は (アルファベット順で) 次の通りです:

* Apache-2.0
* BSD-2-Clause
* BSD-3-Clause
* BSD-4-Clause
* GPL-2.0
* GPL-2.0+
* GPL-3.0
* GPL-3.0+
* LGPL-2.1
* LGPL-2.1+
* LGPL-3.0
* LGPL-3.0+
* MIT

任意ですが、この節を載せておくことを強く推奨します。
この他の識別子は [SPDX Open Source License Registry](https://www.spdx.org/licenses/)
に一覧があります。

## authors

パッケージの作者。
**必須**

これは作者のリストで、少なくとも1人は載せるべきです。
作者は `name <email>` という形式で載せなければなりません。

## readme

パッケージのreadmeファイル。
**任意**

ファイルは `README.rst` もしくは `README.md` のどちらでも構いません。

## homepage

プロジェクトのウェブサイトへのURL。
**任意**

## repository

プロジェクトのウェブサイトへのURL。
**任意**

## ドキュメント

プロジェクトのドキュメントへのURL。
**任意**

## keywords

パッケージに関連する (最大5つの) キーワードの一覧。
**任意**

## classifiers

プロジェクトの説明となるPyPIの [trove classifiers](https://pypi.org/classifiers/) の一覧。
**任意**

```toml
[tool.poetry]
# ...
classifiers = [
    "Topic :: Software Development :: Build Tools",
    "Topic :: Software Development :: Libraries :: Python Modules"
]
```

!!!note

    何にせよ、Python classifierは自動的に追加され、
    それは `python` 要件で決まります。

    `license` 属性から自動的にLicense classifierが設定されます。

## packages

最終配布物に含まれるパッケージやモジュールの一覧。

プロジェクトの構成が `poetry` が標準でサポートしているものと異なる場合は、最終配布物に含めたいパッケージを指定できます。

```toml
[tool.poetry]
# ...
packages = [
    { include = "my_package" },
    { include = "extra_package/**/*.py" },
]
```

パッケージが"source"ディレクトリ内にある場合は、それを指定しなければなりません:

```toml
[tool.poetry]
# ...
packages = [
    { include = "my_package", from = "lib" },
]
```

!!!note

    `packages` を使うとパッケージの自動検出機能が無効になり、
    「デフォルトの」パッケージを **明示的に** 指定しないといけないことになります。

    例えば、 `my_package` という名前のパッケージがあり、 `extra_package` という名前の別のパッケージも一緒に含めたい場合、
    明示的に `my_package` も指定する必要があります:

    ```toml
    packages = [
        { include = "my_package" },
        { include = "extra_package" },
    ]
    ```

!!!note

    Poetryは賢いのでPythonのサブパッケージを検出できます。

    それなので、ルートパッケージがあるディレクトリを指定するだけで済みます。

## includeとexclude

最終的なパッケージに含まれるパターンの一覧。

パッケージ化のために無視するべき、あるいは含めるべきglobの集合をPoetryに明示的に指定できます。
excludeフィールドに指定されたglobは、パッケージのビルド時に含めないファイルの集合を特定します。

パッケージの管理にVCSを使っている場合は、excludeフィールドはVCSの除外設定 (例えばgitの `.gitignore`) を元に作られます。

```toml
[tool.poetry]
# ...
include = ["CHANGELOG.md"]
```

```toml
exclude = ["my_package/excluded.py"]
```

## `dependencies` と `dev-dependencies`

Poetryは、デフォルトでは [PyPi](https://pypi.org) の依存関係を探すよう設定されています。
その場合では、パッケージ名とバージョン文字列だけが要求されます。

```toml
[tool.poetry.dependencies]
requests = "^2.13.0"
```

プライベートレポジトリを使いたい場合は、次のように `pyproject.toml` ファイルに追加できます:

```toml
[[tool.poetry.source]]
name = 'private'
url = 'http://example.com/simple'
```

!!!note

    パッケージと互換性のあるPythonバージョンの宣言は
    義務であることに気を付けてください:

    ```toml
    [tool.poetry.dependencies]
    python = "^3.6"
    ```

## `scripts`

この節は、パッケージをインストールするときにインストールされるスクリプトや実行可能ファイルを記述します

```toml
[tool.poetry.scripts]
poetry = 'poetry:console.run'
```

ほら、これで `poetry` バッケージにある `console.run` を実行する `poetry` スクリプトがインストールされました。

## `extras`

Poetryは次の表現を可能にするために追加の依存関係をサポートしています:

* パッケージを拡張するものだが必須ではない、オプションの依存関係
* オプションの依存関係のパッケージ群

```toml
[tool.poetry]
name = "awesome"

[tool.poetry.dependencies]
# These packages are mandatory and form the core of this package’s distribution.
mandatory = "^1.0"

# A list of all of the optional dependencies, some of which are included in the
# below `extras`. They can be opted into by apps.
psycopg2 = { version = "^2.7", optional = true }
mysqlclient = { version = "^1.3", optional = true }

[tool.poetry.extras]
mysql = ["mysqlclient"]
pgsql = ["psycopg2"]
```

パッケージをインストールするときに、 `-E|--extras` オプションを使って追加分だと指定できます:

```bash
poetry install --extras "mysql pgsql"
poetry install -E mysql -E pgsql
```

## `plugins`

Poetryは
[setuptoolsのエントリーポイント](http://setuptools.readthedocs.io/en/latest/setuptools.html)
と同じように動作する任意のプラグインをサポートしています。
setuptoolsのドキュメントにある例に合わせると、次のように使います:

```toml
[tool.poetry.plugins] # Optional super table

[tool.poetry.plugins."blogtool.parsers"]
".rst" = "some_module:SomeClass"
```

## PoetryとPEP-517

[PEP-517](https://www.python.org/dev/peps/pep-0517/)
は、Pythonプロジェクトをビルドする別のビルドシステムを定義する標準的な方法を導入します。

PoetryはPEP-517に準拠しているので、Poetryを使ってPythonプロジェクトを管理していたなら、次のように
`pyproject.toml` ファイルの `build-system` 節で目にするはずです:

```toml
[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"
```

!!!note

    `new` コマンドや `init` コマンドを使うときには、この節は自動的に追加されます。
