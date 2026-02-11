---
title: 会話
description: Brand Concierge チャットを設定します。
source-git-commit: 0a45b688243b17766143b950994f0837dc0d0b48
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 13%

---

# `conversation`

>[!AVAILABILITY]
>
>Web SDK用のBrand Conciergeは現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

`conversation` オブジェクトには、Brand Concierge チャットセッションの設定オプションが含まれています。 このオブジェクトは、Web SDK バージョン 2.31.0 以降でサポートされています。

## プロパティ

| プロパティ | タイプ | 説明 |
| --- | --- | --- |
| **`stickyConversationSession`** | `boolean` | ページの読み込み中にBrand Concierge チャットセッションを保持するために、Web SDKがセッション Cookie を設定するかどうかを決定します。 デフォルト値は `false` です。この引数を省略するか `false` に設定すると、Brand Concierge チャットは、ページが読み込まれるたびに新しいセッションを開始します。 |

## 例

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  conversation: {
    stickyConversationSession: true
  }
});
```

## Web SDK タグ拡張機能を使用して会話設定を指定します

これらの設定は、[Brand Concierge settings](/help/tags/extensions/client/web-sdk/configure/brand-concierge.md) を使用して web SDKのタグ拡張機能で指定することができます。
