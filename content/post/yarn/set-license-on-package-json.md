---
title: 'yarn実行時にwarning: No license fieldが出ていたのでlicenseをセットした'
description: 'package.jsonにて、licenseをセットしていないとwarningで怒られてたので、それを解決した記事です。'
date: 2020-05-31T22:10:47+09:00
tags: [yarn, license, setting]
archives: '2020'
author: matsu4ki
draft: false
toc: false
---

yarn を走らせると、license を正しくセットしていない場合、

```shell
warning package.json: No license field
```

と表示されます。
エラー名の通り、package.json 内に license がセットされていないためです。
license をセットしましょう。

```json
{
  "license": "SPDXで指定されているID"
}
```

SPDX の ID なんて言われても、わからないですよね。

[SPDX License List | Software Package Data Exchange (SPDX)](https://spdx.org/licenses/)に Identifier という項目で記載されているので、それを上記 json の license で指定すれば OK です。

## いや、俺は license 指定なんてしたくないんやが

"UNLICENSED"を入れれば良いみたい。
プライベートのパッケージなどで利用する場合は、この設定がベストだと思います。

## SPDX とはなんぞや？

ライセンス群をまとめるための、オープンな標準規格のことみたいですね。
なんかライセンスがいっぱい定義されているんだなーぐらいの認識で良いと思います。

> An open standard for communicating software bill of material information, including components, licenses, copyrights, and security references. SPDX reduces redundant work by providing a common format for companies and communities to share important data, thereby streamlining and improving compliance.
>
> https://spdx.dev/

## 参考

- [Home - Software Package Data Exchange (SPDX)](https://spdx.dev/)
- [プライベートプロダクトの package.json の license フィールドには &quot;license: UNLICENSED&quot; と指定すればいいらしい - モヒカンメモ](https://blog.pinkumohikan.com/entry/set-unlicensed-to-license-field-when-private-product)
