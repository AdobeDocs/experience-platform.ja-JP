---
title: 会話
description: Brand Concierge チャット設定を行います。
exl-id: 0f64c7f1-2c28-4c67-af05-dc9ee688fdc0
source-git-commit: 9f7464b78da9615bf6966e34eb129150a481fb5f
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 13%

---

# `conversation`

>[!AVAILABILITY]
>
>Web SDK用Brand Conciergeは現在&#x200B;**ベータ版**&#x200B;です。 機能とドキュメントは変更される場合があります。

`conversation` オブジェクトには、Brand Concierge チャットセッションの設定オプションが含まれています。 このオブジェクトは、Web SDK バージョン 2.31.0以降でサポートされています。

## プロパティ

| プロパティ | タイプ | 説明 |
| --- | --- | --- |
| **`collectSources`** | `boolean` | Web SDKが`adobe_brand_concierge_source` クエリ文字列パラメーターを読み取り、`xdm.channel.referringSource`に含めるかどうかを指定します。 デフォルト値は `false` です。 |
| **`stickyConversationSession`** | `boolean` | Web SDKがセッション Cookieを設定して、ページ読み込み全体でBrand Concierge チャットセッションを保持するかどうかを指定します。 デフォルト値は `false` です。省略または`false`に設定すると、Brand Concierge チャットはページ読み込み時に新しいセッションを開始します。 |

## 例

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  conversation: {
    collectSources: true
    stickyConversationSession: true
  }
});
```

## Web SDK タグ拡張機能を使用した会話の設定

これらの設定は、[Brand Concierge settings](/help/tags/extensions/client/web-sdk/configure/brand-concierge.md)を使用してWeb SDK タグ拡張機能で設定できます。
