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

### osx / linux / bashonwindows インストール手順
```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python
```
### windows powershell インストール手順
```powershell
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python
```

!!! 注意

    Poetryは一度だけインストールすれば良いです。Poetryは現在使われているPythonバージョンを自動的に読み取り、
    それを踏まえて適切に [仮想環境を作成](/poetry-ja/basic-usage/#poetry-and-virtualenvs) します。

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

デフォルトでは、Poetryはユーザーのプラットフォーム特有のホームディイレクトリにインストールされます。インストールされる場所を変えたい場合は、`POETRY_HOME`
環境変数を設定します:

```bash
POETRY_HOME=/etc/poetry python get-poetry.py
```

プレリリースバージョンをインストールしたい場合は、 `get-poetry.py` に `--preview` を渡すか
`POETRY_PREVIEW` 環境変数を使えばできます:

```bash
python get-poetry.py --preview
POETRY_PREVIEW=1 python get-poetry.py
```

同様に、特定のバージョンをインストールしたい場合は、 `--version` や `POETRY_VERSION` 環境変数が使えます:

```bash
python get-poetry.py --version 0.12.0
POETRY_VERSION=0.12.0 python get-poetry.py
```

!!!注意

    インストーラーはリリースバージョンが0.12.0より前のPoetryはサポートしていないことに注意してください。

### 他のインストール方法 (非推奨)

!!!注意

    他のインストール方法を使うと、Poetryは常に、
    仮想環境を作成するためにインストールされたバージョンのPythonを使います。

そのため、使いたいPythonのバージョンごとにPoetryをインストールし、切り替える必要があります。

#### `pip` でインストール

`pip` を使ってPoetryをインストールできます。

```bash
pip install --user poetry
```

!!!警告

    他のパッケージと衝突を起こす可能性のあるPoetryの依存パッケージを
    インストールすることにもなるのを認識しておいてください。

#### `pipx` でインストール

[`pipx`](https://github.com/cs01/pipx)を使ってPoetryをインストールすることもできます。
[pipx]は、PythonのCLIアプリケーションを仮想環境に隔離したまま、システム全体で使えるようにインストールするのに使われます。
これにより更新やアンインストールが環境を汚さずに行えます。
pipxはPython 3.6以降のバージョンをサポートしています。
もっと前のバージョンのPythonを使っている場合は、
[pipsi](https://github.com/mitsuhiko/pipsi)を検討してください。

```bash
pipx install poetry
```

```bash
pipx upgrade poetry
```

```bash
pipx uninstall poetry
```

[Github repository](https://github.com/cs01/pipx)。


## `poetry` のアップデート

Poetryを最新バージョンまで更新するのは簡単で `self:update` コマンドを呼び出すだけです。

```bash
poetry self update
```

プレリリースバージョンをインストールしたい場合は、 `--preview` オプションが使えます。

```bash
poetry self update --preview
```

最後に、特定のバージョンをインストールしたい場合は、引数としてバージョンを `self update` に渡せます。

```bash
poetry self update 0.8.0
```

!!!注意

    `self update` コマンドが使えるのは、推奨インストーラーを使って
    Poetryをインストールしたときだけです。

!!!注意

まだ1.0より前のバージョンのpoetryを使っている場合は、`poetry self:update`を使ってください。


## Bash, Fish, Zshでの補完の有効化

`poetry` はBash, Fish, Zsh用の補完スクリプトの生成をサポートしています。
完全な詳細については `poetry help completions` を参照してください。要点は簡単で、次のコマンドのどれか1つを使う、です:


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

    変更が効くようにするのにシェルを再起動する必要があるかもしれません。

`zsh` では、 `~/.zshrc` の `compinit` より前に次の行を追加しなければなりません:

```bash
fpath+=~/.zfunc
```

`oh-my-zsh` では、 `~/.zshrc` プラグインでpoetryを有効化しなければなりません

```
plugins(
	poetry
	...
	)
```
