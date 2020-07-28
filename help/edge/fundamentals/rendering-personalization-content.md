---
title: パーソナライズされたコンテンツのレンダリング
seo-title: Adobe Experience Platform Web SDK：パーソナライズされたコンテンツのレンダリング
description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
seo-description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
translation-type: tm+mt
source-git-commit: 7b07a974e29334cde2dee7027b9780a296db7b20
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 24%

---


# パーソナライゼーションオプションの概要

Adobe Experience Platformでは、Adobe時(Adobe Targetを含む)のパーソナライゼーションソリューションのクエリをサポートしています。 [!DNL Web SDK] パーソナライゼーションには2つのモードがあります。 自動的にレンダリングできるコンテンツと開発者がレンダリングする必要のあるコンテンツを取得します。 SDKは、ちらつきを [管理する機能も提供します](../../edge/solution-specific/target/flicker-management.md)。

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

を使用して、 `event` コマンドに対する約束として返す決定のリストを要求でき `scopes`ます。 スコープとは、パーソナライゼーションソリューションに対して、どの決定を行うかを知らせる文字列です。

```javascript
alloy("sendEvent",{
    xdm:{...},
    scopes:['demo-1', 'demo-2']
  }).then(function(result){
    if (result.decisions){
      //do something with the decisions
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
> 使用するスコープがサーバー上のmBoxになると、個々の [!DNL Target] スコープではなく、すべてのリクエストが一度に作成されます。 グローバルmboxは常に送信されます。

### 自動コンテンツの取得

レンダリング可能な自動決定 `result.decisions` を含める場合は、をfalseに設定 `renderDecisions` し、特別なスコープを含めることができ `__view__`ます。
