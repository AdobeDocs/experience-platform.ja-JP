---
title: パーソナライズされたコンテンツのレンダリング
seo-title: Adobe Experience Platform Web SDK：パーソナライズされたコンテンツのレンダリング
description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
seo-description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
keywords: personalization;renderDecisions;sendEvent;decisionScopes;result.decisions;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 23%

---


# パーソナライゼーションオプションの概要

Adobe Experience Platform[!DNL Web SDK]は、Adobe Targetを含むAdobeでのパーソナライゼーションソリューションのクエリをサポートしています。 パーソナライゼーションには2つのモードがあります。自動的にレンダリングできるコンテンツと開発者がレンダリングする必要のあるコンテンツを取得します。 また、SDKは、[ちらつき](../personalization/manage-flicker.md)を管理する機能も提供します。

## コンテンツの自動レンダリング

イベントをサーバーに送信し、イベントのオプションとして `renderDecisions` を `true` に設定している場合、SDK はパーソナライズされたコンテンツを自動的にレンダリングします。

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

パーソナライズされたコンテンツのレンダリングは非同期的なので、特定のコンテンツがページの一部である場合を想定する必要はありません。

## コンテンツの手動レンダリング

`decisionScopes`オプションを指定すると、`sendEvent`コマンドに対して約束として返す決定のリストを要求できます。 スコープとは、パーソナライゼーションソリューションに対して、どの決定を行うかを知らせる文字列です。

```javascript
alloy("sendEvent",{
    xdm:{...},
    decisionScopes:['demo-1', 'demo-2']
  }).then(function(result){
    if (result.decisions){
      // Do something with the decisions.
    }
  });
```

これにより、決定のリストが各決定に対してJSONオブジェクトとして返されます。

```json
{
  "decisions": [
    {
      "id": "TNT:123456:0",
      "scope": "demo-1",
      "items": [
        {
          "schema": "https://ns.adobe.com/personalization/html-content-item",
          "data": {
            "id": "11223344",
            "content": "<h2 style=\"color: yellow\">Scoped Decision for location \"alloy-location-1\"</h2>"
          }
        }
      ]
    },
    {
      "id": "TNT:654321:1",
      "scope": "demo-2",
      "items": [
        {
          "schema": "https://ns.adobe.com/personalization/json-content-item",
          "data": {
            "id": "4433221",
            "content": {
              "name":"Scoped decision in JSON"
            }
          }
        }
      ]
    }
  ]
}
```

>[!TIP]
>
> [!DNL Target]を使用すると、スコープはサーバー上のmBoxになり、個別にリクエストされるのではなく、一度にリクエストされるのみです。 グローバルmboxは常に送信されます。

### 自動コンテンツの取得

`result.decisions`に自動レンダリング可能な決定を含め、Aloy自動レンダリングを行わない場合は、`renderDecisions`を`false`に設定し、特別なスコープ`__view__`を含めます。
