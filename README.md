# git-largefile

hg largefile みたいにおっきいファイルを git でも扱いたい

## 動作

コミットするときにハッシュ値だけをコミットし、ファイルの実態は `~/.gitasset/data`
に格納します。

別のマシンでチェックアウトする場合は、 `~/.gitasset/data` を rsync しておきます.

## 設定方法

### インストール

``store-largefile.py`` と ``load-largefile.py`` にパスを通してください。
`pip install path.py` もしておいてください。

### gitconfig

`~/.gitconfig` か `.git/config` に、次のように設定してください

```
[filter "largefile"]
    clean = store-largefile.py
    smudge = load-largefile.py
```

これで largefile フィルターが動きます.

### gitattribute

git リポジトリの中に `.gitattributes` っていうファイルを作って、次のように設定してください。

```
*.png  filter=largefile
*.jpeg filter=largefile
*.jpg  filter=largefile
*.gif  filter=largefile
```

これで設定したファイルは largefile フィルターを通るようになります.

# gits3

largefile の S3 版

`.gitasset/data` を rsync しなくても S3 からダウンロードできる.

## インストール

largefile とだいたい同じですが、 boto がいります.

```
$ pip install boto path.py
```

もしくは、go版もあります。自分で `go build` するか、 `dist/` ディレクトリ内の
ビルド済みバイナリを利用してください。
インストールは PATH が通ったディレクトリにバイナリを放り込むだけです。

## 設定

予め S3 にアクセスできるキーとバケットを作っておいてください。

`~/.gitasset/gits3.ini` に次のように書いてください:

```
[DEFAULT]
awskey = "Access Key Id:Secret Access Key"
bucket = バケット名
```

## gitconfig

```
[filter "s3"]
    clean = gits3.py store
    smudge = gits3.py load
```

Go 版を使う時は `gits3.py` の代わりに `gits3-linux-amd64` など、用意したバイナリ名を指定してください。

gitattributes も `filter=s3` に設定しておいてください.
