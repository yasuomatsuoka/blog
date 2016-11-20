+++
title = "GO 環境のセットアップ"
date = "2016-11-19T14:02:21+09:00"
categories = [ "develop" ]
tags = [ "go", "environment" ]

+++

# 参考
こちらの手順通り実行
[MacにGo言語の開発環境を構築する【準備編】](http://blog.flup.jp/2016/02/16/dev_golang_keeping_vendoring/)

# Go のインストール
```
$ brew install go
```

# GOPATH環境変数の設定
.zshrc に追加
```
export GOPATH=$HOME
export PATH=$PATH:$GOPATH/bin
```

# プロジェクト毎 に GOPATH を使い分けるために direnv を使う
- インストール
```
# direnv
$ brew install direnv
$ echo 'eval "$(direnv hook zsh)"' >> ~/.zsh/mac
```

- 設定例
```
dotfiles|master ⇒ pwd
/Users/yasuo/src/github.com/yasuomatsuoka/dotfiles
```
```direnv edit .``` をするとエディタが立ち上がるので
```
export GOPATH=$(pwd):$GOPATH
```
を入れて保存。

# vendoring 設定を有効にし、glide を使う
依存ライブラリをリポジトリ内の vender ディレクトリに取り込み自分のコードと分けることができる
### direnv の設定ファイルに環境変数を追加
```
export GO15VENDOREXPERIMENT=1
```
これで、 go が vender 以下を意識してくれるようになる

## glide
vendoring オプションを有効にしただけでは、```go get``` すると $GOPATH/src 以下に取り込まれてしまう。
プロジェクト内に glide の YAML を書くことによって、vender 以下に外部のライブラリを取り込むことができる。

- glide のインストール
```
$ brew install glide
```

- YAML の作成
```
# プロジェクトのディレクトリで
$ glide create
```
- package の追加
```
$ glide get github.com/Masterminds/cookoo
```

- package のインストール
```
$ glide up
```

- .gitignore に vender を追加
