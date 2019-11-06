---
title: "Hugo + Github Pages + Github Actionsでブログを構築する"
date: 2019-11-06T00:00:00+09:00
---

今回はHugoとGithub PagesとGithub Actionsを用いてこのブログを構築していきます。

## 構築するブログの要件

今回、以下の要件でブログを構築します。

- ブログをMarkdownで書く
- 独自ドメインで公開する
- ドメイン代以外は無料で運用する
- HTTPS(SSL)対応

とてもシンプルな要件なため、Hugo + Github Pages + Github Actionsを採用します。

### Hugoとは

> Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.

https://gohugo.io/

Go言語で作られた静的サイトジェネレータのOSSです。 Markdownで書かれたものからHTML/CSS/JSを生成してくれるので、生成されたファイルを適当な場所にアップロードするだけでブログなどが公開できます。

静的サイトジェネレータは他にも以下のようなものがあります。

- [Jekyll](https://jekyllrb.com/)
- [Hexo](https://hexo.io/)

さらに以下のサイトではもっと多くの静的サイトジェネレータがまとめられています。Next.jsって静的サイトジェネレータになるんですね・・・

https://www.staticgen.com/

今回はシンプルな要件で十分使えそうなHugoを採用します。

### Github Pagesとは

> Hosted directly from your GitHub repository. Just edit, push, and your changes are live.

https://pages.github.com/

Githubのレポジトリにあげた成果物をもとにウェブサイトが公開できるサービスです。

類似のホスティングサービスとして以下のようなものがあります。

- [netlify](https://www.netlify.com/)
- [Firebase](https://firebase.google.com/)

他にもHugoの公式ドキュメントにホスティングサービスとデプロイ方法が多数紹介されています。

https://gohugo.io/hosting-and-deployment/

ほとんどのサービスで独自ドメインとHTTPS(SSL)対応が無料で可能です。
他のサービスを使わずにすむため、今回はGitHub Pagesを採用します。

### Github Actionsとは

> GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub. Make code reviews, branch management, and issue triaging work the way you want.

https://github.com/features/actions

CI/CDなど自動化をするためのワークフローを定義できるサービスです。

CI/CDのサービスといえば、他にも以下のようなものがあります。

- [CircleCI](https://circleci.com/)
- [Travis CI](https://travis-ci.org/)

今回はGitHub Pagesを使っていて、GitHubのみで完結するということでGitHub Actionsを採用します。

## ブログの構築方法

### Hugoの導入

公式のインストール方法を見ながらHugoを導入していきます。 今回はReleaseページからバイナリを落としてきて利用します。

https://github.com/gohugoio/hugo/releases

```
# PATHが通っているところにバイナリを置く
$ curl -sL https://github.com/gohugoio/hugo/releases/download/v0.59.1/hugo_0.59.1_Linux-64bit.tar.gz | tar zxv -C ~/bin/ hugo
hugo
$ hugo version
Hugo Static Site Generator v0.59.1-D5DAB232 linux/amd64 BuildDate: 2019-10-31T15:21:02Z
```

### サイト作成

Hugoで管理していく設定ファイルやディレクトリ構成を作成していきます。 以下のコマンドを実行することで、blogディレクトリにひな形が生成されます。

```
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

### テーマ設定

次にサイトのデザインにテーマは[Mainroad](https://themes.gohugo.io/mainroad/)を利用します。
submodule追加と設定ファイル編集だけで簡単に反映ができます。

```
$ git submodule add https://github.com/vimux/mainroad themes/mainroad

# config.tomlに以下の設定を追加してテーマを反映させる。
# theme = "mainroad"
$ vim config.toml
```

### 記事作成

以下のコマンドで記事を作成します。

```
$ hugo new post/create-hugo-site.md
---
title: "Create Hugo Site"
date: 2019-11-06T00:00:00+09:00
draft: true
---
```

draftの行を消すかfalseにすることで生成対象のMarkdownになります。
`archetypes/default.md`を編集することで、記事新規作成時のテンプレートを編集することが可能です。

```
$ cat archetypes/default.md
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
---
```

そして、本文を書き始めます。
今回はこの記事を書いています。

### HTML/CSS/JS生成

以下のコマンドを実行するだけで、`public`ディレクトリ配下にHTML/CSS/JSが生成されます。

```
$ hugo
```

### GitHub Pagesで公開

生成されたファイルを使うことで、ブログを公開することができます。
今回は`public`ディレクトリ配下に生成された成果物を`gh-pages`ブランチにコミットすることでGitHub Pagesで公開できます。

`https://github.com/<username>/<repository>`のレポジトリにプッシュした場合、`https://<username>.github.io/<repository>/`のURLでブログが公開できます。
今回はhttps://github.com/otomarukanta/blog のレポジトリにあげているため、https://otomarukanta.github.io/blog/ に書いた記事が公開されるようになります。

### 独自ドメインの設定

今回は独自ドメインを設定したいのでレポジトリのSettingsから「Github Pages」の欄から「Custom domain」を設定します。
今回は`blog.otomarukanta.com`設定します。

この設定を加えることで、[CNAMEを作成するコミット](https://github.com/otomarukanta/blog/commit/20ed20beb2dba7899112a2cfb0209aca4b351f5a)が自動で実行されます。

https://blog.otomarukanta.com/ にアクセス
次に、お名前.comやムームードメインなどのドメイン管理サイト側で設定をします。

今回はムームードメインを利用しているので、以下のような設定を追加します。

|サブドメイン|種別|内容|
|---|---|---|
|blog|CNAME|otomarukanta.github.io|

この段階で、https://blog.otomarukanta.com/ でブログが公開できています。

### GitHub Actionsで自動デプロイ

GitHub Actionsで以下の作業を自動化していきます。

- トリガーはmasterブランチへのコミット
- `hugo`コマンドを利用してHTML/CSS/JSなどを生成
- 生成したものをgh-pagesブランチへコミット

このようなGitHub Actionsをすでに作られている方がいるので、流用します。
[GitHub Pages action](https://github.com/marketplace/actions/github-pages-action)

以下のコマンドでSSH鍵を生成します。

```
$ ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
```

GitHubのレポジトリのSettingsからDeploy Keysに公開鍵を追加します。
このときにAllow write accessにチェックを入れるのを忘れずに。

同じくSettingsからSecretsにNameが`ACTIONS_DEPLOY_KEY`の秘密鍵を追加します。

以下のworkflowの定義を利用します。基本的にはドキュメントにかかれているサンプルをそのまま利用していますが、テーマにsubmoduleを利用しているので、`actions/checkout`のステップの`submodules: true`を忘れずに。

```
name: github pages

on:
  push:
    branches:
    - master

jobs:
  build-deploy:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@master
      with:
        submodules: true

    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2.2.2
      with:
        hugo-version: '0.59.1'

    - name: Build
      run: hugo --minify

    - name: Deploy
      uses: peaceiris/actions-gh-pages@v2.5.0
      env:
        ACTIONS_DEPLOY_KEY: ${{ secrets.ACTIONS_DEPLOY_KEY }}
        PUBLISH_BRANCH: gh-pages
        PUBLISH_DIR: ./public
```

この設定まで終わると、`master`ブランチにコミットすると自動でファイルを生成し`gh-pages`にあげてくれます。