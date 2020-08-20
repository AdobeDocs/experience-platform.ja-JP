---
title: パーソナライズされたコンテンツのレンダリング
seo-title: Adobe Experience Platform Web SDK：パーソナライズされたコンテンツのレンダリング
description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
seo-description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
keywords: personalization;renderDecisions;sendEvent;decisionScopes;result.decisions;
translation-type: tm+mt
source-git-commit: 8c256b010d5540ea0872fa7e660f71f2903bfb04
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 23%

---


# パーソナライゼーションオプションの概要

Adobe Experience Platformは、Adobe Targetを含むAdobeでのパーソナライゼーションソリューションのクエリを [!DNL Web SDK] サポートしています。 パーソナライゼーションには2つのモードがあります。自動的にレンダリングできるコンテンツと開発者がレンダリングする必要のあるコンテンツを取得します。 SDKは、ちらつきを [管理する機能も提供します](../../edge/solution-specific/target/flicker-management.md)。

## コンテンツを自動的にレンダリング

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

オプションを指定すると、 `sendEvent` コマンドに対する約束として返される決定のリストを要求でき `decisionScopes` ます。 スコープとは、パーソナライゼーションソリューションに対して、どの決定を行うかを知らせる文字列です。

```javascript
alloy("sendEvent",{
    xdm:{...},
    decisionScopes:['demo-1', 'demo-2']
  }).then(function(result){
    if (result.decisions){
      // Do something with the decisions.
    }
  })
```

これにより、決定のリストが各決定に対してJSONオブジェクトとして返されます。

```javascript
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
> 使用する場合 [!DNL Target]、スコープはサーバー上のmBoxになり、個別ではなくすべてのスコープが一度に要求されるだけになります。 グローバルmboxは常に送信されます。

### 自動コンテンツの取得

を自動レンダリング可能な決定を含め、Alloy自動レンダリング `result.decisions` を行わない場合は、をに設定し、特別なスコープ `renderDecisions` を含め `false`ることができ `__view__`ます。
