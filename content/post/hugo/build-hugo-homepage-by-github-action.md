---
title: "Github-Actionsでhugoのデプロイを自動化してみた"
date: 2020-02-10T12:46:44+09:00
tags: [hugo, github-action, ci]
archives: "2020"
author: Amatsuki
draft: false
---
# はじめに
結構前からGitHub-Actionsを使ってみたいなと思っていましたが、題材がなく困ってました。
そこで、「なにか定期的にデプロイしてるやつってあったっけ」と考えたところ

「そういやhugoのデプロイって自動化できるんじゃね？」という考えに行き着き、実際にやってみた感じです。

# TL;DR
- この記事はhugo用リポジトリとhomepage用リポジトリで分割している場合の説明です
- GitHub-Actionsを利用してデプロイすることは可能
    - hugo用のactionがすでに存在するので、そちらを利用すると良い
- GitHub-Actions用のymlファイルを下に記載しているので、参考になるかも
- 別リポジトリにデプロイする場合はアクセスキーの準備が必要
- CD時にhugoのバージョン差異に注意しないと、デプロイ結果が微妙に異なる可能性あり（v0.60.0でMarkdownに[アップデート](https://gohugo.io/news/0.60.0-relnotes/)があった）


# 現在の環境整理
とりあえず、どう変わったのかをイメージでざっくりと載せてみました。

## 移行前
ターミナルでshellScriptを走らせて、管理用リポジトリとブログ用リポジトリにあげてました。

![実施前のイメージ図](/resources/build-hugo-homepage-by-github-action/before-deploy.png)


## 移行後
管理用リポジトリにpushするだけで、ブログがデプロイされるようになりました。
- shellscript動かす環境じゃない（今は早々ないと思いますけど）
- とりあえずサクッと作成/編集したい
- GitHub上でちょっと修正したい

といったときにgitさえ入っていれば実行可能になりました！

![実施後のイメージ図](/resources/build-hugo-homepage-by-github-action/after-deploy.png)

# 実際の作業
## 作業の洗い出し
今までデプロイするために使っていた`deploy.sh`で行っていた作業を洗い出す（洗い出すっていう程量ないけど…）

1. gitでコミット（homepage）
2. hugoでデプロイ
3. public以下をpush（AmatsukiUrato.github.io）

## ymlファイルへの落とし込む
`steps`以下の階層にいる`name`毎でjobが切れています。

最後のjobに`${{ secrets.ACTIONS_DEPLOY_KEY }}`とりますが、こちらはGitHubのリポジトリ上で秘密鍵を登録しておくと、keyが呼び出せるといったものになります。
GitHub-Actionsを走らせているリポジトリとは別のリポジトリにデプロイする際などに必要です。

```yml
name: github pages

on:
  push:
    branches:
      - master

jobs:
  build-deploy:
    runs-on: ubuntu-18.04
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Checkout theme repository
        uses: actions/checkout@v2
        with: #Themeを適応させる
          repository: Tazeg/hugo-blog-jeffprod
          path: themes/hugo-blog-jeffprod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'

      - name: Build
        run: hugo --minify --cleanDestinationDir

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }} # 秘密鍵はgithub側で登録しておく
          external_repository: AmatsukiUrato/AmatsukiUrato.github.io
          publish_dir: ./public
          publish_branch: master
```

# さいごに
GitHub-Actionsは思ってたより使いやすかったです。でも、一番触ってみたかったビジュアルワークフローはどうやらymlファイルが導入された際になくなってしまったようです😭 公式からは、検討するけど今すぐ復活はしないよという方針らしいです。

publicリポジトリは無料で利用できるしprivateも2000分/月は無料みたいなので、お金をあんまり気にせずチェックできる点はよかったです😋
GitHub専門でいうと、TravisCIがありますが、`GitHub-Actions`はprivateでも無料な点はでかいかもしれないです。後は無料でmacOSデプロイできるところですかね？

CDは上手くいくと楽しいので、ぜひやりましょう😎

# 参考
- [peaceiris/actions-hugo: GitHub Actions for Hugo ⚡️ Setup Hugo quickly and build your site fast. Hugo extended, Hugo Modules, Linux (Ubuntu), macOS, and Windows are supported.](https://github.com/peaceiris/actions-hugo)
- [peaceiris/actions-gh-pages: GitHub Actions for GitHub Pages 🚀 Deploy static files and publish your site easily. Static-Site-Generators-friendly.](https://github.com/peaceiris/actions-gh-pages)
- [新しくなったGitHub ActionsでHugoブログのデプロイをしてみた - Qiita](https://qiita.com/kaakaa_hoe/items/8fc2cfc2e16093cc7264)
- [HugoのビルドをGithub Actionで自動化する - 1ミリもわからん](https://raahii.github.io/posts/automating-hugo-builds-with-github-actions/)