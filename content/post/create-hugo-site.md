---
title: "Hugoでブログを構築する"
date: 2019-11-06T00:00:00+09:00
---

今回はHugoを用いてこのブログを構築していきます。

## Hugoとは
> Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.

https://gohugo.io/

Go言語で作られた静的サイトジェネレータのOSSです。 Markdownで書かれたものからHTML/CSS/JSを生成してくれるので、生成されたファイルを適当な場所にアップロードするだけでブログなどが公開できます。

静的サイトジェネレータは他にも以下のようなものがあります。

- jekyll(Ruby製)
- Hexo(Node製)

## Hugoの導入方法
公式のインストール方法を見ながらHugoを導入していく。 今回はReleaseページからバイナリを落としてきて利用する。

https://github.com/gohugoio/hugo/releases

```
# PATHが通っているところにバイナリを置く
$ curl -sL https://github.com/gohugoio/hugo/releases/download/v0.59.1/hugo_0.59.1_Linux-64bit.tar.gz | tar zxv -C ~/bin/ hugo
hugo
$ hugo version
Hugo Static Site Generator v0.59.1-D5DAB232 linux/amd64 BuildDate: 2019-10-31T15:21:02Z
サイト作成
Hugoで管理していく設定ファイルやディレクトリ構成を作成していきます。 以下のコマンドを実行することで、blogディレクトリにひな形が生成されます。

$ hugo new site blog
Congratulations! Your new Hugo site is created in /home/kanta/src/github.com/otomarukanta/blog.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```

## テーマ設定

次にサイトのデザインにテーマはMainroadを利用する。 https://themes.gohugo.io/mainroad/

```
# テーマの設定
$ git submodule add https://github.com/vimux/mainroad themes/mainroad
Cloning into '/home/kanta/src/github.com/otomarukanta/blog/themes/mainroad'...
remote: Enumerating objects: 17, done.
remote: Counting objects: 100% (17/17), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 1652 (delta 6), reused 7 (delta 1), pack-reused 1635
Receiving objects: 100% (1652/1652), 933.85 KiB | 1.18 MiB/s, done.
Resolving deltas: 100% (951/951), done.
# config.tomlに以下の設定を追加してテーマを反映させる。
# theme = "mainroad"
$ vim config.toml
```

## 記事作成

以下のコマンドで記事を作成します。

```
$ hugo new post/create-hugo-site.md
---
title: "Create Hugo Site"
date: 2019-11-06T00:00:00+09:00
draft: true
---
```

titleを変えるのと、draftの行を消す or falseにする。

そして、本文を書き始めます。 今回はこの記事を書いています。

## HTML/CSS/JS生成

以下のコマンドを実行するだけで、`public`ディレクトリ配下にHTML/CSS/JSが生成されます。

```
hugo
```

## Github Pagesで公開

生成されたファイルを使うことで、Blogとして公開することができます。
公式ドキュメントにはGCP/AWS/Azureといったパブリッククラウドのオブジェクトストレージを利用した
https://gohugo.io/hosting-and-deployment/hugo-deploy/
今回は[Github Pages](https://pages.github.com/)を利用します。

Githubレポジトリを作成し、SettingsにあるGithub Pages