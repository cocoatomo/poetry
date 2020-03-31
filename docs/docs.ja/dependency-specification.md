# 依存関係指定

プロジェクトの依存関係は、依存関係の種類とインストールするときに必要になる任意の制約に基づいて様々な形式で指定できます。

## バージョン制約

### キャレット要件

**キャレット要件** を使うと、指定したバージョンへのsemantic versioningに適合した更新が行えます。
メジャー、マイナー、パッチという番号群のうち0でない最も左の数字を、新しいバージョン番号が更新しない場合に、更新が許可されます。
この場合に、 `poetry update requests` を実行したとすると、Poetryはもし利用可能ならバージョン `2.14.0` ヘ更新しますが、 `3.0.0` には更新しません。
その変わりにバージョン文字列に `^0.1.13` を指定していた場合は、Poetryは `0.2.0` ではなく `0.1.14` へ更新します。
`0.0.x` は他のどんなバージョンとも適合しないと見なされます。

これらがキャレット要件とそれに許可されたバージョンの例です:

| Requirement | Versions allowed |
| ----------- | ---------------- |
| ^1.2.3      | >=1.2.3 <2.0.0   |
| ^1.2        | >=1.2.0 <2.0.0   |
| ^1          | >=1.0.0 <2.0.0   |
| ^0.2.3      | >=0.2.3 <0.3.0   |
| ^0.0.3      | >=0.0.3 <0.0.4   |
| ^0.0        | >=0.0.0 <0.1.0   |
| ^0          | >=0.0.0 <1.0.0   |

### チルダ要件

**チルダ要件** は更新できる最小バージョンを指定します。
メジャー、マイナー、パッチバージョンを指定するか、メジャー、マイナーバージョンだけを指定した場合は、パッチレベルのみ変更が許可されます。
メジャーバージョンだけを指定した場合は、マイナーレベルとパッチレベルの変更が許可されます。

`~1.2.3` はチルダ要件の例です。

| Requirement | Versions allowed |
| ----------- | ---------------- |
| ~1.2.3      | >=1.2.3 <1.3.0   |
| ~1.2        | >=1.2.0 <1.3.0   |
| ~1          | >=1.0.0 <2.0.0   |

### ワイルドカード要件

**ワイルドカード要件** を使うと、ワイルドカードの位置に任意のバージョンが許可されます。

`*`, `1.*`, `1.2.*` はワイルドカード要件の例です。

| Requirement | Versions allowed |
| ----------- | ---------------- |
| *           | >=0.0.0          |
| 1.*         | >=1.0.0 <2.0.0   |
| 1.2.*       | >=1.2.0 <1.3.0   |

### 不等要件

**不等要件** を使うと、バージョンの範囲や依存している厳密なバージョンを自分の手で指定できます。

これらが不等要件の例です:

```text
>= 1.2.0

> 1

< 2

!= 1.2.3

```

### 厳密要件

パッケージの厳密なバージョンを指定できます。
この指定は、このバージョンかつこのバージョンのみをインストールするようPoetryに指示します。
他の依存関係が別のバージョンを要求した場合は、ソルバーは最終的には失敗し、インストールや更新の手続きを中止します。

#### 複合要件

複数のバージョン要件は、コンマ区切りで並べられます。例えば、 `>= 1.2, < 1.5` などです。

## `git` 依存関係

`git` レポジトリにあるライブラリに依存するための、指定する必要のある最小限の情報はgitをキーとするレポジトリの場所です:

```toml
[tool.poetry.dependencies]
requests = { git = "https://github.com/requests/requests.git" }
```

これ以外の情報を指定していないので、Poetryは `master` ブランチの最新のコミットを使いプロジェクトをするつもりなのだと仮定します。

`git` キーと `rev` キー, `tag` キー, `branch` キーを組み合わせて、それ以外のものも指定できます。
これが、 `next` という名前のブランチの最新のコミットを使いたいことを指定する例です:

```toml
[tool.poetry.dependencies]
# Get the latest revision on the branch named "next"
requests = { git = "https://github.com/kennethreitz/requests.git", branch = "next" }
# Get a revision by its commit hash
flask = { git = "https://github.com/pallets/flask.git", rev = "38eb5d3b" }
# Get a revision by its tag
numpy = { git = "https://github.com/numpy/numpy.git", tag = "v0.13.2" }
```

## `path` 依存関係

ローカルディレクトリやローカルファイルにあるライブラリに依存するために、 `path` 属性が使えます:

```toml
[tool.poetry.dependencies]
# directory
my-package = { path = "../my-package/" }

# file
my-package = { path = "../my-package/dist/my-package-0.1.0.tar.gz" }
```


## `url` 依存関係

リモートアーカイブにあるライブラリに依存するために、 `url` 属性が使えます:

```toml
[tool.poetry.dependencies]
# directory
my-package = { url = "https://example.com/my-package-0.1.0.tar.gz" }
```

with the corresponding `add` call:

```bash
poetry add https://example.com/my-package-0.1.0.tar.gz
```


## Python制限依存関係

ある依存関係を特定のバージョンのPythonでだけインストールする、という指定もできます:

```toml
[tool.poetry.dependencies]
pathlib2 = { version = "^2.2", python = "~2.7" }
```

```toml
[tool.poetry.dependencies]
pathlib2 = { version = "^2.2", python = "~2.7 || ^3.2" }
```

## Using environment markers

If you need more complex install conditions for your dependencies, Poetry
supports [environment
markers](https://www.python.org/dev/peps/pep-0508/#environment-markers)  via
the `markers` property:

```toml
[tool.poetry.dependencies]
pathlib2 = { version = "^2.2", markers = "python_version ~= '2.7' or sys_platform == 'win32'" }
```


## 複数制約依存関係

あるときは、対象のPythonバージョンに依存して、依存関係に異なるバージョン範囲が設定されることがあります。

バージョン1.9まではPython 3.0未満、2.0以降はPython 3.4以上というPythonとの互換性を持つパッケージ `foo` への依存関係があるとしましょう:
このとき次のように宣言します:

```toml
[tool.poetry.dependencies]
foo = [
    {version = "<=1.9", python = "^2.7"},
    {version = "^2.0", python = "^3.4"}
]
```

!!!注意

    制約は (`python` のように) 異なる要件で **なければならず** 、
    そうでない場合は、依存関係の解決でエラーが発生します。
