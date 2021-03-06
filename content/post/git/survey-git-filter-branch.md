---
title: 'gitの最終奥義「filter-branch」を使ったパスワードの置換ワンライナーを読み解く'
date: 2019-12-19T22:00:05+09:00
tags: [git, filter-branch]
archives: '2019'
author: Amatsuki
draft: true
---

## はじめに

この前、会社でメールアドレスとパスを`git`にあげてしまうという失態を犯してしましました。

気づいたときには、時すでにおすし 🍣  
目の前には大量のコミットが積み上げられてました＼(^o^)／

そこで、何とかならないのかと色々ぐぐっていると`filter-branch`という **git 界最大級の特大魔法コマンド** を見つけました。これを使うと過去にのすべてを書き換えることができるみたいです。

今回はこの`filter-branch`を使って、すべての履歴から特定の文字列を置換するワンライナーを読み解いていこうと思います。

## 対象のワンライナー

```bash
 git filter-branch -f --tree-filter "find . -name key.yml  -exec sed -i -e 's/PasswordPassword1234/hoge/g' {} \;"  --prune-empty -- --all
```

実際に置換できるので良いですが、あまりコマンドの意味を理解しておりません…  
ということで、ワンライナーを順番に見ていきます。

```bash
 git filter-branch -f --tree-filter
```

まず、今回の目玉である`filter-branch`について。

> 最強のオプション: filter-branch
> 歴史を書き換える方法がもうひとつあります。これは、大量のコミットの書き換えを機械的に行いたい場合 (メールアドレスを一括変更したりすべてのコミットからあるファイルを削除したりなど) に使うものです。 そのためのコマンドが filter-branch です。これは歴史を大規模にばさっと書き換えることができるものなので、プロジェクトを一般に公開した後や書き換え対象のコミットを元にしてだれかが作業を始めている場合はまず使うことはありません。 しかし、これは非常に便利なものでもあります。 一般的な使用例をいくつか説明するので、それをもとにこの機能を使いこなせる場面を考えてみましょう。
>
> 全コミットからのファイルの削除
> これは、相当よくあることでしょう。 誰かが不注意で git add . をした結果、巨大なバイナリファイルが間違えてコミットされてしまったとしましょう。これを何とか削除してしまいたいものです。 あるいは、間違ってパスワードを含むファイルをコミットしてしまったとしましょう。このプロジェクトをオープンソースにしたいと思ったときに困ります。 filter-branch は、こんな場合に歴史全体を洗うために使うツールです。 passwords.txt というファイルを歴史から完全に抹殺してしまうには、filter-branch の --tree-filter オプションを使います。
> git の公式より

## さいごに

## 参考

- [Git リポジトリ内の全履歴からパスワード を置換！ リポジトリの履歴を全て書き換えて無かったことにする](https://qiita.com/yamasaki-masahide/items/7ea28b2b682b6664ba45)
