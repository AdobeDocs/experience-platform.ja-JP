---
title: パーソナライズされたコンテンツのレンダリング
seo-title: Adobe Experience Platform Web SDK：パーソナライズされたコンテンツのレンダリング
description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
seo-description: Experience Platform Web SDK でパーソナライズされたコンテンツをレンダリングする方法について説明します
translation-type: tm+mt
source-git-commit: 938e0e56111c96f6b0b378c9e14fb92af70c5e10
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 41%

---


# （ベータ版）パーソナライゼーションオプションの概要

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Web SDKは、アドビのターゲットを含むアドビのパーソナライゼーションソリューションのクエリをサポートしています。 パーソナライゼーションには2つのモードがあります。 自動的にレンダリングできるコンテンツと開発者がレンダリングする必要のあるコンテンツを取得します。 SDKは、ちらつきを [管理する機能も提供します](../../edge/solution-specific/target/flicker-management.md)。

## コンテンツを自動的にレンダリング

イベントをサーバーに送信し、イベントのオプションとして `renderDecisions` を `true` に設定している場合、SDK はパーソナライズされたコンテンツを自動的にレンダリングします。

```javascript
alloy("event", {
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

パーソナライズされたコンテンツのレンダリングは非同期でおこなわれます。そのため、コンテンツの特定の部分がページの一部である場合を想定しないでください。

## コンテンツの手動レンダリング

を使用して、 `event` コマンドに対する約束として返す決定のリストを要求でき `scopes`ます。 スコープとは、パーソナライゼーションソリューションに対して、どの決定を行うかを知らせる文字列です。

```javascript
alloy("event",{
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

{info}ターゲットスコープを使用すると、サーバー上でmBoxになる場合は、個別のリクエストではなく、すべてのリクエストが一度に作成されます。 グローバルmboxは常に送信されます。
{info}

### 自動コンテンツの取得

レンダリング可能な自動決定 `result.decisions` をに含める場合は、falseに設定し `renderDecisions` て、特別なスコープを含めます `__view__`
