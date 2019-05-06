# 基本的な使い方

基本的な使い方の入門として、 `pendulum` という日付ライブラリをインストールします。
Poetryをまだインストールしていない場合は、 [Introduction](/docs/) の章を参照してください。

## プロジェクトのセットアップ

最初に、新しいプロジェクトを作成し、 `poetry-demo` と名付けましょう:

```bash
poetry new poetry-demo
```


このコマンドは、次の内容を持つ `poetry-demo` ディレクトリを作成します:

```text
poetry-demo
├── pyproject.toml
├── README.rst
├── poetry_demo
│   └── __init__.py
└── tests
    ├── __init__.py
    └── test_poetry_demo.py
```


`pyproject.toml` ファイルがここでは最も重要なものです。
このファイルが、プロジェクトの依存関係を統括します。
今のところの中身は、このようになっています:

```toml
[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = ""
authors = ["Sébastien Eustace <sebastien@eustace.io>"]

[tool.poetry.dependencies]
python = "*"

[tool.poetry.dev-dependencies]
pytest = "^3.4"
```


### 依存関係の指定

プロジェクトに依存関係を追加したい場合は、 `tool.poetry.dependencies` セクションに指定できます。

```toml
[tool.poetry.dependencies]
pendulum = "^1.4"
```


見て分かるように、このセクションには **パッケージ名** と **バージョン制約** の対応付けを書きます。

Poetryはこの情報を使い、 `tool.poetry.repositories` セクションに登録されたパッケージ "レポジトリ"
もしくはデフォルトの [PyPI](https://pypi.org) から、正しいファイル群を検索します。

`pyproject.toml` ファイルを手で編集する変わりに、 `add` コマンドも使えます。

```bash
$ poetry add pendulum
```


このコマンドは自動的に適切なバージョン制約を見付け、パッケージとその依存関係を **インストールします** 。


### バージョン制約

今の例では、 `^1.4` というバージョン制約の付いた  `pendulum` パッケージを要求しています。
この制約は、1.4.0以上かつ2.0.0未満のバージョンという意味です (`>=1.4.0 <2.0.0`)。

バージョンやバージョンどうしの関係、バージョン制約について、より深いことについては [versions](/docs/versions/)
を読んでください。


!!!注意

**Poetryはどうやって正しいファイルをダウンロードするのか?**

    `pyproject.toml` に依存関係を指定しているとき、Poetryはまず最初に要求されたパッケージ名を取り出し、
    `repositories` キーに登録されたレポジトリを検索します。
    追加のレポジトリを登録していないか、指定したレポジトリから要求された名前の
    パッケージが見付からない場合は、PyPIに戻って検索します。

    Poetryが正しいパッケージを複数見付けたときは、指定されたバージョン制約に
    最も適合するものを見付けようとします。


## 依存関係のインストール

プロジェクトに定義された依存関係をインストールするには、 `install` コマンドを実行するだけです。

```bash
poetry install
```


このコマンドを実行すると、2つのうちどちらかが起こります:

### `poetry.lock` 無しのインストール

以前にこのコマンドを実行したことが無く、 `poetry.lock` ファイルも存在しない場合は、Poetryは `pyproject.toml`
に並べられた全ての依存関係を解決し、それらのファイルの最新バージョンをダウンロードするだけです。

Poetryがインストールを完了すると、ダウンロードしたパッケージとその正確なバージョンを `poetry.lock`
へ書き込み、その特定のバージョンでプロジェクトを固定します。
プロジェクトレポジトリに `poetry.lock`
ファイルをコミットし、このプロジェクトに携わる全ての人にとって、同じバージョンの依存関係を強制されるべきです (下でさらに解説します)。


### `poetry.lock` 有りのインストール

This brings us to the second scenario. If there is already a `poetry.lock`
file as well as a `pyproject.toml` file when you run `poetry install`, it
means either you ran the `install` command before, or someone else on the
project ran the `install` command and committed the `poetry.lock` file to
the project (which is good).

Either way, running `install` when a `poetry.lock` file is present resolves
and installs all dependencies that you listed in `pyproject.toml`, but
Poetry uses the exact versions listed in `poetry.lock` to ensure that the
package versions are consistent for everyone working on your project.  As a
result you will have all dependencies requested by your `pyproject.toml`
file, but they may not all be at the very latest available versions (some of
the dependencies listed in the `poetry.lock` file may have released newer
versions since the file was created).  This is by design, it ensures that
your project does not break because of unexpected changes in dependencies.

### Commit your `poetry.lock` file to version control

Committing this file to VC is important because it will cause anyone who
sets up the project to use the exact same versions of the dependencies that
you are using.  Your CI server, production machines, other developers in
your team, everything and everyone runs on the same dependencies, which
mitigates the potential for bugs affecting only some parts of the
deployments.  Even if you develop alone, in six months when reinstalling the
project you can feel confident the dependencies installed are still working
even if your dependencies released many new versions since then.  (See note
below about using the update command.)

!!!注意

    For libraries it is not necessary to commit the lock file.


## Updating dependencies to their latest versions

As mentioned above, the `poetry.lock` file prevents you from automatically
getting the latest versions of your dependencies.  To update to the latest
versions, use the `update` command.  This will fetch the latest matching
versions (according to your `pyproject.toml` file)  and update the lock file
with the new versions.  (This is equivalent to deleting the `poetry.lock`
file and running `install` again.)

!!!注意

    Poetry will display a **Warning** when executing an install command if `poetry.lock` and `pyproject.toml`
    are not synchronized.


## Poetry and virtualenvs

When you execute the `install` command (or any other "install" commands like
`add` or `remove`), Poetry will check if it's currently inside a virtualenv
and, if not, will use an existing one or create a brand new one for you to
always work isolated from your global Python installation.

!!!注意

    To create the virtualenv for the current project, Poetry will use
    the currently activated Python version.

    To easily switch between Python versions, it is recommended to
    use [pyenv](https://github.com/pyenv/pyenv) or similar tools.

    For instance, if your project is Python 2.7 only, a standard workflow
    would be:

    ```bash
    pyenv install 2.7.15
    pyenv local 2.7.15  # Activate Python 2.7 for the current project
    poetry install
    ```
