---
title: 'hugoの.GitInfoが適応された話(理由は不明です…)'
date: 2019-10-20T12:47:18+09:00
tags: [hugo, git]
archives: '2019'
author: Amatsuki
draft: false
---

ずっと前から、記事の更新は git のコミット履歴を使いたいと思っていたのですが、  
`.GitInfo`の情報が記事にうまく反映されず`nil`になっていたため、手動で作業してました 😇

`.GitInfo`が使えるようになった原因は恐らく、記事のパスを

`/content/post/[記事].md`→`/content/post/[分類用フォルダ]/[記事].md`  
※[]内はそれぞれの用途で変更する

としたことです。  
何故これで`.GitInfo`が適応されたのかは不明ですが、とにかくずっとやりたかったことなので、大満足 😭
