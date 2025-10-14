---
title: debugEnabled
description: コードを使用して、Web SDK でデバッグ機能を有効にします。
exl-id: 89392d16-9a0d-427b-86b6-70005f63f440
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# `debugEnabled`

`debugEnabled` プロパティを使用すると、Web SDK コードを使用したデバッグを有効または無効にできます。 これは、[&#x200B; デバッグ &#x200B;](../../use-cases/debugging.md) を有効にするための利用可能な方法の 1 つです。 実装内でデバッグを有効にすると、常にデバッグを有効にする必要がある場合に、web サイトの開発時に他の方法よりも便利です。 このデバッグ方法はすべての訪問者に対して有効なので、実稼動ページには推奨されません。

デバッグを有効にするその他の方法については、[&#x200B; デバッグ &#x200B;](../../use-cases/debugging.md) ユースケースページを参照してください。

## Web SDK タグ拡張機能を使用したデバッグの有効化

Web SDK タグ拡張機能を使用してネイティブに利用できるデバッグオプションはありません。 [&#x200B; 代替デバッグ方法 &#x200B;](../../use-cases/debugging.md) を使用します。

## Web SDK JavaScript ライブラリを使用したデバッグの有効化

`configure` コマンドの実行時に、`debugEnabled` ブール値を `true` に設定します。 SDK の設定時にこのプロパティを省略すると、デフォルトは `false` になります。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  debugEnabled: true
});
```
