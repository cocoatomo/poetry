# コマンド

これまでに、何かをするためのコマンドラインインターフェースの使い方を学びました。
この章では、全ての利用可能なコマンドについて記述します。

コマントラインでヘルプを出すには、 `poetry` もしくは `poetry list`
と呼び出すとコマンドの完全なリストが見られて、コマンドのどれかに `--help` を付けて実行するとより詳しい情報が得られます。

`Poetry` は [cleo](https://github.com/sdispater/cleo)
を使っているので、曖昧性が無い場合は、短い名前でもコマンドが呼び出せます。

```bash
poetry up
```


これは `poetry update` コマンドを呼び出します。


## グローバルオプション

* `--verbose (-v|vv|vvv)`: メッセージの冗長さを増やします:
  1つでは標準の出力、2つではより冗長な出力、3つではデバッグ出力です。
* `--help (-h)` : ヘルプ情報を表示します。
* `--quiet (-q)` : メッセージを出力しません。
* `--ansi`: ANSI出力を強制します。
* `--no-ansi`: ANSI出力を無効にします。
* `--version (-V)`: このアプリケーションのバージョンを表示します。


## new

このコマンドは、ほとんどのプロジェクトに適するディレクトリ構造を作成して、新しいPythonプロジェクトに勢いをつける助けをしてくれます。

```bash
poetry new my-package
```


というコマンドは次のフォルダを作成します:

```text
my-package
├── pyproject.toml
├── README.rst
├── my_package
│   └── __init__.py
└── tests
    ├── __init__.py
    └── test_my_package
```


フォルダとは別のプロジェクト名を付けたい場合は、 `--name` オプションで渡せます:

```bash
poetry new my-folder --name my-package
```


srcフォルダを使いたい場合は、 `--src` オプションが使えます:

```bash
poetry new --src my-package
```


このコマンドは次のようなフォルダ構成を作成します:

```text
my-package
├── pyproject.toml
├── README.rst
├── src
│   └── my_package
│       └── __init__.py
└── tests
    ├── __init__.py
    └── test_my_package
```


## init

このコマンドは、パッケージについての基本的な情報を提供するよう促して、 `pyproject.toml` ファイルを対話的に作成する助けになります。

このコマンドは、賢いデフォルト設定を使いつつ、フィールドを埋めるよう対話的に求めます。

```bash
poetry init
```


### Options

* `--name`: パッケージ名。
* `--description`: パッケージの説明。
* `--author`: パッケージの作者。
* `--dependency`: バージョン制約付きの要求されるパッケージ。 `foo:1.0.0` 形式でなければいけません。
* `--dev-dependency`: 開発時に要求されるパッケージ、 `--require` を参照してください。


## install

`install` コマンドは、現在のプロジェクトにある `pyproject.toml` ファイルを読み込み、依存関係を解決し、インストールします。

```bash
poetry install
```


`poetry.lock` ファイルが今のディレクトリにある場合は、依存関係を解決する代わりに、ロックファイルにある厳密なバージョンを使います。
これにより、そのライブラリを使う全員が同じバージョンの依存関係を手に入れることを保証します。

`poetry.lock` ファイルが無い場合は、Poetryは依存関係解決の結果に従ってロックファイルを作成します。

`--no-dev` オプションを渡すことで、開発用の依存関係をインストールさせないで欲しいことをコマンドに指定できます。

```bash
poetry install --no-dev
```


`--E|--extras` オプションを渡すことで、インストールして欲しい追加の依存関係の指定もできます (より詳しいことは
[Extras](#extras) を参照してください)

```bash
poetry install --extras "mysql pgsql"
poetry install -E mysql -E pgsql
```


### Options

* `--no-dev`: 開発用の依存関係をインストールしない。
* `--extras (-E)`: インストールする機能 (複数値も可能です)。

## update

最新バージョンの依存関係を取得し、 `poetry.lock` を更新するには、 `update` コマンドを使いましょう。

```bash
poetry update
```


このコマンドは、プロジェクトの全ての依存関係を解決し、その厳密なバージョンを `poetry.lock` に書き込みます。

全てではない一部のパッケージだけを更新したい場合は、このように並べて指定できます:

```bash
poetry update requests toml
```


### Options

* `--dry-run` : 作業を表示しますが、何も実行しません (暗黙的に --verbose が有効になります)。
* `--no-dev` : 開発用の依存関係をインストールしません。
* `--lock` : インストールを行いません (ロックファイルを更新するだけです)。

## add

`add` コマンドは `pyproject.toml` に要求しているパッケージを追加し、インストールします。

バージョン制約を指定しなかった場合は、Poetryは利用可能なパッケージバージョンに基づいて適切なものを選びます。

```bash
poetry add requests pendulum
```


`git` 依存関係の追加もできます:

```bash
poetry add pendulum --git https://github.com/sdispater/pendulum.git
```


のようにするか、ローカルのディレクトリやファイルを指し示すようにします:

```bash
poetry add my-package --path ../my-package/
poetry add my-package --path ../my-package/dist/my-package-0.1.0.tar.gz
poetry add my-package --path ../my-package/dist/my_package-0.1.0.whl
```


Path dependencies pointing to a local directory will be installed in
editable mode (i.e. setuptools "develop mode").  It means that changes in
the local directory will be reflected directly in environment.

If you don't want the dependency to be installed in editable mode you can
specify it in the `pyproject.toml` file:

```
[tool.poetry.dependencies]
my-package = {path = "../my/path", develop = false}
```


### Options

* `--dev (-D)`: Add package as development dependency.
* `--git`: The url of the Git repository.
* `--path`: The path to a dependency.
* `--extras (-E)`: Extras to activate for the dependency.
* `--optional` : Add as an optional dependency.
* `--dry-run` : 作業を表示しますが、何も実行しません (暗黙的に --verbose が有効になります)。


## remove

The `remove` command removes a package from the current list of installed
packages.

```bash
poetry remove pendulum
```


### Options

* `--dev (-D)`: Removes a package from the development dependencies.
* `--dry-run` : 作業を表示しますが、何も実行しません (暗黙的に --verbose が有効になります)。


## show

To list all of the available packages, you can use the `show` command.

```bash
poetry show
```


If you want to see the details of a certain package, you can pass the
package name.

```bash
poetry show pendulum

name        : pendulum
version     : 1.4.2
description : Python datetimes made easy

dependencies:
 - python-dateutil >=2.6.1
 - tzlocal >=1.4
 - pytzdata >=2017.2.2
```


### Options

* `--no-dev`: Do not list the dev dependencies.
* `--tree`: List the dependencies as a tree.
* `--latest (-l)`: Show the latest version.
* `--outdated (-o)`: Show the latest version but only for packages that are
  outdated.


## build

The `build` command builds the source and wheels archives.

```bash
poetry build

```


Note that, at the moment, only pure python wheels are supported.

### Options

* `--format (-F)`: Limit the format to either wheel or sdist.

## publish

This command publishes the package, previously built with the
[`build`](#build) command, to the remote repository.

It will automatically register the package before uploading if this is the
first time it is submitted.

```bash
poetry publish
```


It can also build the package if you pass it the `--build` option.

### Options

* `--repository (-r)`: The repository to register the package to (default: `pypi`).
Should match a repository name set by the [`config`](#config) command.
* `--username (-u)`: The username to access the repository.
* `--password (-p)`: The password to access the repository.

## config

The `config` command allows you to edit poetry config settings and
repositories.

```bash
poetry config --list
```


### Usage

````bash
poetry config [options] [setting-key] [setting-value1] ... [setting-valueN]
````


`setting-key` is a configuration option name and `setting-value1` is a
configuration value.  See [Configuration](/docs/configuration/) for all
available settings.

### Options

* `--unset`: Remove the configuration element named by `setting-key`.
* `--list`: Show the list of current config variables.

## run

The `run` command executes the given command inside the project's
virtualenv.

```bash
poetry run python -V
```


It can also execute one of the scripts defined in `pyproject.toml`.

So, if you have a script defined like this:

```toml
[tool.poetry.scripts]
my-script = "my_module:main"
```


You can execute it like so:

```bash
poetry run my-script
```


Note that this command has no option.

## shell

The `shell` command spawns a shell, according to the `$SHELL` environment
variable, within the virtual environment.  If one doesn't exist yet, it will
be created.

```bash
poetry shell
```


## check

The `check` command validates the structure of the `pyproject.toml` file and
returns a detailed report if there are any errors.

```bash
poetry check
```


## search

This command searches for packages on a remote index.

```bash
poetry search requests pendulum
```


### Options

* `--only-name (-N)`: Search only in name.

## lock

This command locks (without installing) the dependencies specified in
`pyproject.toml`.

```bash
poetry lock
```


## version

This command bumps the version of the project and writes the new version
back to `pyproject.toml`

The new version should ideally be a valid semver string or a valid bump
rule: `patch`, `minor`, `major`, `prepatch`, `preminor`, `premajor`,
`prerelease`.
