# 設定

Poetryは、 `config` コマンド ([使い方のより詳しいことはこちら](/poetry-ja/cli/#config))
による設定や、初めてそのコマンドを実行するときに自動的に作成される `config.toml` で直接設定が行えます。
このファイルはたいてい次のディレクトリのうち1つで見付かります。

- macOS:   `~/Library/Application Support/pypoetry`
- Windows: `C:\Users\<username>\AppData\Roaming\pypoetry`

Unixでは、XDG仕様に従い、 `$XDG_CONFIG_HOME` をサポートしています。
つまり、デフォルトでは `~/.config/pypoetry` となります。

## Local configuration

Poetry also provides the ability to have settings that are specific to a
project by passing the `--local` option to the `config` command.

```bash
poetry config virtualenvs.create false --local
```

## Listing the current configuration

To list the current configuration you can use the `--list` option of the
`config` command:

```bash
poetry config --list
```

which will give you something similar to this:

```toml
cache-dir = "/path/to/cache/directory"
virtualenvs.create = true
virtualenvs.in-project = false
virtualenvs.path = "{cache-dir}/virtualenvs"  # /path/to/cache/directory/virtualenvs
```

## Displaying a single configuration setting

If you want to see the value of a specific setting, you can give its name to
the `config` command

```bash
poetry config virtualenvs.path
```

For a full list of the supported settings see [Available
settings](#available-settings).

## Adding or updating a configuration setting

To change or otherwise add a new configuration setting, you can pass a value
after the setting's name:

```bash
poetry config virtualenvs.path /path/to/cache/directory/virtualenvs
```

For a full list of the supported settings see [Available
settings](#available-settings).

## Removing a specific setting

If you want to remove a previously set setting, you can use the `--unset`
option:

```bash
poetry config virtualenvs.path --unset
```

The setting will then retrieve its default value.

## Using environment variables

Sometimes, in particular when using Poetry with CI tools, it's easier to use
environment variables and not have to execute configuration commands.

Poetry supports this and any setting can be set by using environment
variables.

The environment variables must be prefixed by `POETRY_` and are comprised of
the uppercase name of the setting and with dots and dashes replaced by
underscore, here is an example:

```bash
export POETRY_VIRTUALENVS_PATH=/path/to/virtualenvs/directory
```

This also works for secret settings, like credentials:

```bash
export POETRY_HTTP_BASIC_MY_REPOSITORY_PASSWORD=secret
```


## 利用可能な設定

### `cache-dir`: string

The path to the cache directory used by Poetry.

Defaults to one of the following directories:

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
