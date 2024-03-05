---
title: debugEnabled
description: Web SDK でデバッグ機能を有効にするには、コードを使用します。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# `debugEnabled`

The `debugEnabled` プロパティを使用すると、Web SDK コードを使用したデバッグを有効または無効にできます。 これは、 [デバッグ](../../use-cases/debugging.md). 常にデバッグを有効にする場合、Web サイト開発時に他の方法よりも実装内でデバッグを有効にする方が便利です。 このデバッグ方法では、すべての訪問者に対してこの機能が有効になるので、実稼動ページにはお勧めしません。

詳しくは、 [デバッグ](../../use-cases/debugging.md) デバッグを有効にするその他の方法については、使用例のページを参照してください。

## Web SDK タグ拡張機能を使用したデバッグの有効化

Web SDK タグ拡張をネイティブで使用できるデバッグオプションはありません。 を使用します。 [代替デバッグ方法](../../use-cases/debugging.md).

## Web SDK JavaScript ライブラリを使用したデバッグの有効化

を設定します。 `debugEnabled` ブール値 `true` （実行時） `configure` コマンドを使用します。 SDK を設定する際にこのプロパティを省略した場合、デフォルトはになります。 `false`.

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```
