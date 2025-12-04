---
title: edgeBasePath
description: Adobe サービスの操作に使用するエンドポイントのベースパス。
exl-id: 3542575d-ad02-415c-8e47-97c877dfdd9d
source-git-commit: 1fa7b48f99948a5252635dc1a8c5142fea5df57b
workflow-type: tm+mt
source-wordcount: '110'
ht-degree: 0%

---

# `edgeBasePath`

Adobe サービスを操作する際、`edgeBasePath` プロパティは対象のパスを変更します。 データ収集用のアルファまたはベータ版プログラムに参加している場合は、Adobeから、この変数の変更をリクエストされることがあります。 ほとんどの組織では、このプロパティを設定または変更する必要はありません。

`edgeBasePath` コマンドの実行時に「`configure`」テキストフィールドを設定します。 このプロパティを省略すると、既定値の `ee` が使用されます。 Adobeでは、ほとんどの設定でこのプロパティを省略することをお勧めします。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  edgeBasePath: "ee"
});
```

## Web SDK タグ拡張機能を使用したEdgeのベースパス

このフィールドと同等の Web SDK タグ拡張機能は、タグ拡張機能を設定する際に [&#x200B; 詳細設定 &#x200B;](/help/tags/extensions/client/web-sdk/configure/advanced-settings.md) の下にあります。
