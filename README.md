## BLOG
http://yasuomatsuoka.github.io/blog/

## 利用しているテーマ
https://github.com/jbub/ghostwriter

## git clone 直後
themes/以下 がないのでテーマを git clone する

```
themes|master ⇒ pwd
/Users/yasuo/src/github.com/yasuomatsuoka/blog/themes
themes|master ⇒ git clone https://github.com/jbub/ghostwriter
```

## 記事の作成手順
- 記事の作成
```
blog|master ⇒ pwd
/Users/yasuo/src/github.com/yasuomatsuoka/blog
~|⇒ hugo new post/setup_hugo.md
```

- ブログの確認
```
blog|master ⇒ pwd
/Users/yasuo/src/github.com/yasuomatsuoka/blog
blog|master ⇒ hugo server -\-theme=ghostwriter -\-buildDrafts
```
http://localhost:1313/blog/

## 記事のデプロイ
記事.md 内の ```draft=true``` を削除してから
```
blog|master ⇒ ./deploy.sh
```

## 環境構築メモ
http://yasuomatsuoka.github.io/blog/2016/05/hugo-%E3%81%AE%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97/
