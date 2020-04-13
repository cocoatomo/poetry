# 設定

Poetryは、 `config` コマンド ([使い方のより詳しいことはこちら](/poetry-ja/cli/#config))
による設定や、初めてそのコマンドを実行するときに自動的に作成される `config.toml` で直接設定が行えます。
このファイルはたいてい次のディレクトリのうち1つで見付かります。

- macOS:   `~/Library/Application Support/pypoetry`
- Windows: `C:\Users\<username>\AppData\Roaming\pypoetry`

Unixでは、XDG仕様に従い、 `$XDG_CONFIG_HOME` をサポートしています。
つまり、デフォルトでは `~/.config/pypoetry` となります。

## ローカル設定

Poetryは、`config` コマンドの `--local` オプションを渡すことで、プロジェクトに特定の設定を持たせる機能も提供しています。

```bash
poetry config virtualenvs.create false --local
```

## 現在の設定の一覧

現在の設定の一覧を出すには、`config` コマンドの `--list` オプションが使えます:

```bash
poetry config --list
```

これで次のような出力が見れます:

```toml
cache-dir = "/path/to/cache/directory"
virtualenvs.create = true
virtualenvs.in-project = false
virtualenvs.path = "{cache-dir}/virtualenvs"  # /path/to/cache/directory/virtualenvs
```

## 設定を1つだけ表示

ある1つの設定の値を見たい場合は、`config` コマンドにその設定の名前を渡せばよいです。

```bash
poetry config virtualenvs.path
```

サポートされている設定の全一覧は [利用可能な設定](#available-settings) を参照してください。

## 設定の追加あるいは更新

新しい設定の変更それか追加するには、設定名の後ろに値を渡せば良いです:

```bash
poetry config virtualenvs.path /path/to/cache/directory/virtualenvs
```

サポートされている設定の全一覧は [利用可能な設定](#available-settings) を参照してください。

## ある設定を消す

前に設定したものを消したい場合は、`--unset` オプションが使えます:

```bash
poetry config virtualenvs.path --unset
```

その設定はデフォルト値に戻ります。

## 環境変数の使用

あるとき、特にPoetryをCIツールと一緒に使っているとき、環境変数を使うのは簡単で、設定コマンドを実行する必要がありません。

Poetryはそれをサポートしていて、どんな設定でも環境変数で設定できます。

環境変数は `POETRY_` で始まらなければならず、大文字にした設定の名前で構成され、ドットとダッシュをアンダースコアで置き換えたものとなります。
これが例です:

```bash
export POETRY_VIRTUALENVS_PATH=/path/to/virtualenvs/directory
```

この機能は証明書のような秘密情報に対しても使えます:

```bash
export POETRY_HTTP_BASIC_MY_REPOSITORY_PASSWORD=secret
```


## 利用可能な設定

### `cache-dir`: string

Poetryが使うキャッシュディレクトリのpathです。

次のディレクトリのうち1つがデフォルト値になります:

- macOS:   `~/Library/Caches/pypoetry`
- Windows: `C:\Users\<username>\AppData\Local\pypoetry\Cache`
- Unix:    `~/.cache/pypoetry`

### `virtualenvs.create`: boolean

まだ存在していない場合、新しい仮想環境を作成します。
デフォルトは `true` です。

### `virtualenvs.in-project`: boolean

プロジェクトのルートディレクトリに仮想環境を作成します。
デフォルトは `false` です。

### `virtualenvs.path`: string

仮想環境が作成されるディレクトリ。
デフォルトは `{cache-dir}/virtualenvs` (Windowsでは `{cache-dir}\virtualenvs`) です。

### `repositories.<name>`: string

別のレポジトリを新しく設定します。
より詳しいことは [レポジトリ](/poetry-ja/repositories/) を参照してください。
