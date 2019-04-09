# 入門

PoetryはPythonでの依存関係管理とパッケージングのためのツールです。
Poetryを使うとプロジェクトが依存しているライブラリを宣言でき、それらを管理 (インストールおよびアップデート) してくれます。


## システム要件

PoetryはPython 2.7あるいはPython 3.4以降を必要とします。
Poetryはマルチプラットフォームに対応していて、Windows, Linux, OSXで同じように動作することを目標としています。


## インストール

Poetryは独自のインストーラーを提供していて、依存パッケージをベンダー化 (vendorizing) によって `poetry`
を、システムの他のものから隔離された状態でインストールします。
これは推奨された `poetry` のインストール方法です。

```bash
curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python
```


!!! 注意

    Poetryは一度だけインストールすれば良いです。Poetryは現在使われているPythonバージョンを自動的に読み取り、
    それを踏まえて適切に [仮想環境を作成](/docs/basic-usage/#poetry-and-virtualenvs) します。

このインストーラーは、 `poetry` ツールをPoetryの `bin` ディレクトリにインストールします。
そのディレクトリの位置は、Unixでは `$HOME/.poetry/bin` で、Windowsでは
`%USERPROFILE%\.poetry\bin` です。

このディレクトリは `$PATH` 環境変数に含まれ、何かしらの設定をしなくてもシェルから実行できるということになります。
シェルを新たに開き、次のコマンドを打ち込んでください:

```bash
poetry --version
```


`Poetry 0.12.0` のような出力が見えたら、Poetryを使う準備が整いました。
もしPoetryが自分には向かないと判断したら、 `--uninstall` オプションを付けるか、インストーラーを実行する前に
`POETRY_UNINSTALL` 環境変数を設定するかして、先程のインストーラーを再度実行すると、システムからPoetryを完全に消去できます。

```bash
python get-poetry.py --uninstall
POETRY_UNINSTALL=1 python get-poetry.py
```


If you want to install prerelease versions, you can do so by passing
`--preview` to `get-poetry.py` or by using the `POETRY_PREVIEW` environment
variable:

```bash
python get-poetry.py --preview
POETRY_PREVIEW=1 python get-poetry.py
```


Similarly, if you want to install a specific version, you can use
`--version` or the `POETRY_VERSION` environment variable:

```bash
python get-poetry.py --version 0.12.0
POETRY_VERSION=0.12.0 python get-poetry.py
```


!!!note

    インストーラーはリリースバージョンが0.12.0より前のPoetryはサポートしていないことに注意してください。

### 他のインストール方法 (非推奨)

!!!note

    他のインストール方法を使うと、Poetryは常に、
    仮想環境を作成するためにインストールされたバージョンのPythonを使います。

そのため、使いたいPythonのバージョンごとにPoetryをインストールし、切り替える必要があります。

#### `pip` でインストール

Using `pip` to install Poetry is possible.

```bash
pip install --user poetry
```


!!!warning

    他のパッケージと衝突を起こすかもしれないPoetryの依存パッケージを
    インストールすることにもなることを認識しておいてください。

#### `pipsi` でインストール

Using [`pipsi`](https://github.com/mitsuhiko/pipsi) to install Poetry is
also possible.

```bash
pipsi install poetry
```


Make sure your installed version of `pipsi` is at least version `0.10`,
otherwise Poetry will not function properly. You can get it from its [Github
repository](https://github.com/mitsuhiko/pipsi).


## `poetry` のアップデート

Updating poetry to the latest stable version is as simple as calling the
`self:update` command.

```bash
poetry self:update
```


If you want to install prerelease versions, you can use the `--preview`
option.

```bash
poetry self:update --preview
```


And finally, if you want to install a specific version you can pass it as an
argument to `self:update`.

```bash
poetry self:update 0.8.0
```


!!!note

    `self:update` コマンドが使えるのは、推奨インストーラーを使って
    Poetryをインストールしたときだけです。


## Bash, Fish, Zshでの補完の有効化

`poetry` supports generating completion scripts for Bash, Fish, and Zsh.
See `poetry help completions` for full details, but the gist is as simple as
using one of the following:


```bash
# Bash

poetry completions bash > /etc/bash_completion.d/poetry.bash-completion



# Bash (macOS/Homebrew)

poetry completions bash > $(brew --prefix)/etc/bash_completion.d/poetry.bash-completion



# Fish

poetry completions fish > ~/.config/fish/completions/poetry.fish



# Zsh

poetry completions zsh > ~/.zfunc/_poetry



# Oh-My-Zsh

mkdir $ZSH/plugins/poetry

poetry completions zsh > $ZSH/plugins/poetry/_poetry



```


!!! 注意

    You may need to restart your shell in order for the changes to take effect.

For `zsh`, you must then add the following line in your `~/.zshrc` before
`compinit`:

```bash
fpath+=~/.zfunc

```


For `oh-my-zsh`, you must then enable poetry in your `~/.zshrc` plugins

```
plugins(

	poetry

	...

	)

```

