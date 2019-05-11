# 設定

Poetryは、 `config` コマンド ([使い方のより詳しいことはこちら](/docs/cli/#config))
による設定や、初めてそのコマンドを実行するときに自動的に作成される `config.toml` で直接設定が行えます。
このファイルはたいてい次のディレクトリのうち1つで見付かります。

- macOS:   `~/Library/Application Support/pypoetry`
- Windows: `C:\Users\<username>\AppData\Roaming\pypoetry`

Unixでは、XDG仕様に従い、 `$XDG_CONFIG_HOME` をサポートしています。
つまり、デフォルトでは `~/.config/pypoetry` となります。

## 利用可能な設定

### `settings.virtualenvs.create`: boolean

まだ存在していない場合、新しい仮想環境を作成します。
デフォルトは `true` です。

### `settings.virtualenvs.in-project`: boolean

プロジェクトのルートディレクトリに仮想環境を作成します。
デフォルトは `false` です。

### `settings.virtualenvs.path`: string

仮想環境が作成されるディレクトリ。
デフォルトは次のディレクトリのうちの1つです。

- macOS:   `~/Library/Caches/pypoetry/virtualenvs`
- Windows: `C:\Users\<username>\AppData\Local\pypoetry\Cache/virtualenvs`
- Unix:    `~/.cache/pypoetry/virtualenvs`

### `repositories.<name>`: string

別のレポジトリを新しく設定します。
より詳しいことは [レポジトリ](/docs/repositories/) を参照してください。
