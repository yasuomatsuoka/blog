+++
date = "2016-05-01T14:57:29+09:00"
title = "Hugo のセットアップ"

+++

# Hugo 設定メモ

## 参考
こちらの手順通り実行
[Hugo Part 1 - Hugo で github にブログを立ち上げる](http://blog.syati.info/post/create_hugo/)

## Hugo のインストール
```
$ brew install hugo
```

## ブログの作成
```
$ pwd
/Users/yasuo
$ hugo new site ./blog
```

## 記事の作成
```
$ pwd
/Users/yasuo/blog
$ hugo new post/setup_hugo.md
```

## テーマ一式インストール
```
$ pwd
/Users/yasuo/blog
$ git clone -\-recursive https://github.com/spf13/hugoThemes themes
```

## ブログの確認
```
$ hugo server -\-theme=gindoro -\-buildDrafts
```

## github のレポジトリの準備
ブログ用のレポジトリを作成する
```
http://yasuomatsuoka.gihub.io/blog
```

## blog の設定ファイルをいじる
```
config.toml
baseurl = "http://yasuomatsuoka.github.io/blog"
languageCode = "ja-jp"
title = "やすおのぶろぐ"
theme = "gindoro"
canonifyurls = true # 相対パスではなく baseurl を基点とした絶対パスにする

[params]
  Description = "ソフトウェア開発的なこととか自転車のこととか食べ物のこと"
  Author = "Yasuo Matsuoka"
  GithubUser = "yasuomatsuoka"

[taxonomies]
  categories = [ "Development" ]
  tags = [ "Development", "Blogging", "Food", "Bike" ]
```

## Hugo をコミットする
```
$ git init
$ git remote add origin git@github.com:yasuomatsuoka/blog.git
$ git add -A
$ git commit -m 'initial hugo commit.'
$ git push origin master
```

## gh-pages ブランチを作成する
public ディレクトリのみ管理するブランチを作成
```
$ git checkout --orphan gh-pages   # orphan ブランチ 作成
$ git rm --cached $(git ls-files)  # 要らないので、全て管理対象からすべて外す
$ git add README.md                # README.md だけいれておく
$ git commit -m "initial commit on gh-pages branch"
$ git push origin gh-pages
```
gh-pages ブランチを master の public に取り込む
```
$ git checkout master
$ git subtree add --prefix=public git@github.com:yasuomatsuoka/blog.git gh-pages --squash
$ git subtree pull --prefix=public git@github.com:yasuomatsuoka/blog.git gh-pages
```

master の変更を gh-pages が追えるようになったので public を作成
```
$ hugo
$ git add -A
$ git commit -m 'updated blog.'
$ git push origin master
$ git subtree push --prefix=public git@github.com:yasuomatsuoka/blog.git gh-pages
```

## デプロイスクリプトを作成
blog 以下に作成
```
# !/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo

# Add changes to git.
git add -A

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master
git subtree push --prefix=public git@github.com:[username]/yourblog.git gh-pages　
```

権限付与
```
chmod +x deploy.sh
```

デプロイ
```
./deploy.sh
```
