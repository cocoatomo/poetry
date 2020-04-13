# コマンド

これまでに、何かをするためのコマンドラインインターフェースの使い方を学びました。
この章では、全ての利用可能なコマンドについて記述します。

コマントラインでヘルプを出すには、 `poetry` と呼び出すとコマンドの完全なリストが見られて、コマンドのどれかに `--help`
を付けて実行するとより詳しい情報が得られます。

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
    └── test_my_package.py
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
    └── test_my_package.py
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

デフォルトでは、`poetry` は `install` を実行するたびにプロジェクトのパッケージをインストールします:

```bash
$ poetry install
Installing dependencies from lock file

No dependencies to install or update

  - Installing <your-package-name> (x.x.x)

```

このインストールをスキップしたい場合は、`--no-root` オプションを使ってください。

```bash
poetry install --no-root
```

### Options

* `--no-dev`: 開発用の依存関係をインストールしない。
* `--no-root`: ルートパッケージ (あなたのプロジェクト) をインストールしない。
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

`add` コマンドは `pyproject.toml` に要求しているパッケージを追加し、インストールします。

```bash
poetry add requests pendulum
```

パッケージを追加するときに、次のように制約を指定できます:

```bash
poetry add pendulum@^2.0.5
poetry add "pendulum>=2.0.5"
```

既に追加してあるパッケージを追加しようとすると、エラーが出ます。
ただし、上にあるような制約を指定している場合は、その指定された制約を使って依存関係が更新されます。既に追加してある依存関係の最新バージョンが欲しい場合は、
`latest` という特殊な制約が使えます:

```bash
poetry add pendulum@latest
```

`git` 依存関係の追加もできます:

```bash
poetry add git+https://github.com/sdispater/pendulum.git
```

特定のブランチ、タグ、リビジョンをチェックアウトする必要がある場合に、 `add` を使っているときはその指定ができます:

```bash
poetry add git+https://github.com/sdispater/pendulum.git#develop
poetry add git+https://github.com/sdispater/pendulum.git#2.0.5
```

のようにするか、ローカルのディレクトリやファイルを指し示すようにします:

```bash
poetry add ./my-package/
poetry add ../my-package/dist/my-package-0.1.0.tar.gz
poetry add ../my-package/dist/my_package-0.1.0.whl
```

ローカルのディレクトリを指すパス依存関係は、編集可能モード (つまり、setuptoolsの"develop mode") でインストールされます。
これは、ローカルのディレクトリの変更が、動作環境に直接影響するということです。

依存関係を編集可能モードでインストールしたくない場合は、 `pyproject.toml` ファイルでその指定ができます。

```toml
[tool.poetry.dependencies]
my-package = {path = "../my/path", develop = false}
```

インストールしたいパッケージがextraを提供している場合、パッケージを追加するときにそのextrasを指定できます:

```bash
poetry add requests[security,socks]
poetry add "requests[security,socks]~=2.22.0"
poetry add "git+https://github.com/pallets/flask.git@1.1.1[dotenv,dev]"
```

### Options

* `--dev (-D)`: 開発用の依存関係としてパッケージを追加します。
* `--path`: 依存関係へのパス。
* `--optional` : オプションの依存関係を追加します。
* `--dry-run` : 作業を表示しますが、何も実行しません (暗黙的に --verbose が有効になります)。


## remove

`remove` コマンドは、現在のインストールされたパッケージの一覧からパッケージを削除します。

```bash
poetry remove pendulum
```

### Options

* `--dev (-D)`: 開発用の依存関係からパッケージを削除します。
* `--dry-run` : 作業を表示しますが、何も実行しません (暗黙的に --verbose が有効になります)。


## show

全ての利用可能なパッケージを列挙するには、 `show` コマンドが使えます。

```bash
poetry show
```

ある特定のパッケージの詳細を見たい場合は、パッケージ名を渡せます。

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

* `--no-dev`: 開発用の依存関係を列挙しません。
* `--tree`: 依存関係を木構造で列挙します。
* `--latest (-l)`: 最新バージョンを表示します。
* `--outdated (-o)`: 古くなったパッケージの最新バージョンだけを表示します。


## build

`build` コマンドは、ソースアーカイブとwheelアーカイブへビルドします。

```bash
poetry build
```

現時点では、ピュアPythonのwheelのみサポートされていることに注意してください。

### Options

* `--format (-f)`: 形式をwheelもしくはsdistに限定します。

## publish

このコマンドは、前に [`build`](#build) コマンドでビルドしたパッケージを、リモートレポジトリに公開します。

初めて提出する場合には、このコマンドは、アップロードの前に自動的にパッケージの登録を行います。

```bash
poetry publish
```

`--build` オプションを渡すと、パッケージのビルドもしてくれます。

### Options

* `--repository (-r)`: パッケージを登録するレポジトリ (デフォルト: `pypi`) です。
[`config`](#config) コマンドで設定したレポジトリ名と一致しなければなりません。
* `--username (-u)`: レポジトリにアクセスするためのユーザー名。
* `--password (-p)`: レポジトリにアクセスするためのパスワード。

## config

`config` コマンドを使うと、Poetryのsettingsやrepositoriesの設定を編集できます。

```bash
poetry config --list
```

### 使い方

````bash
poetry config [options] [setting-key] [setting-value1] ... [setting-valueN]
````

`setting-key` は設定のオプション名で `setting-value1` は設定値です。
利用可能な全ての設定は [設定](/poetry-ja/configuration/) を参照してください。

### Options

* `--unset`: `setting-key` で名付けられた設定要素を削除します。
* `--list`: 現在の設定変数の一覧を表示します。

## run

`run` コマンドは、与えられたコマンドをプロジェクトの仮想環境の中で実行します。

```bash
poetry run python -V
```

このコマンドは `pyproject.toml` に定義されたスクリプトうち1つを実行することもできます。

それなので、次のように定義されたスクリプトがある場合:

```toml
[tool.poetry.scripts]
my-script = "my_module:main"
```

このようにして実行できます:

```bash
poetry run my-script
```

このコマンドにはオプションが無いことに注意してください。

## shell

`shell` コマンドは、 `$SHELL` 環境変数に従って、仮想環境の中でシェルを起動します。
仮想環境がまだ無い場合は、作成されます。

```bash
poetry shell
```

## check

`check` コマンドは、 `pyproject.toml` ファイルの構造を検証し、エラーがあった場合は、詳細なレポートを返します。

```bash
poetry check
```

## search

このコマンドは、リモートの目録にあるパッケージを検索します。

```bash
poetry search requests pendulum
```

### Options

* `--only-name (-N)`: 名前でのみ検索します。

## lock

このコマンドは、(インストールはせずに) `pyproject.toml` に指定されている依存関係をロックします。

```bash
poetry lock
```

## version

このコマンドは現在のプロジェクトのバージョンを表示したり、正しいバージョン更新規則を指定された場合、プロジェクトのバージョンを進めて
`pyproject.toml` へ新しいバージョンを書き戻します。

新しいバージョンは理想的には、有効なセマンティックバージョン文字列か、有効な更新規則であるべきです。有効な更新規則: `patch`, `minor`,
`major`, `prepatch`, `preminor`, `premajor`, `prerelease` 。


## export

このコマンドはlockファイルを他のフォーマットへ出力します。

```bash
poetry export -f requirements.txt > requirements.txt
```

!!!注意

    現時点では `requirements.txt` 形式のみをサポートしています。

### Options

* `--format (-f)`: 出力する形式。現時点では `requirements.txt` のみサポートされています。
* `--output (-o)`: 出力ファイル名。省略した場合は、標準出力に表示されます。
* `--dev`: 開発用の依存関係を含めます。
* `--extras (-E)`: 含めたい依存関係のextra一式です。
* `--without-hashes`: 出力したファイルのハッシュを除外します。
* `--with-credentials`: Include credentials for extra indices.

## env

The `env` command regroups sub commands to interact with the virtualenvs
associated with a specific project.

See [Managing environments](./managing-environments.md) for more information
about these commands.
