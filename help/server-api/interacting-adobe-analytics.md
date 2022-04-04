---
title: Adobe Analyticsの操作
description: Edge Network Server API を使用してAdobe Analyticsとやり取りする方法を説明します
seo-description: Learn how to use the Edge Network Server API to interact with Adobe Analytics
keywords: データ収集；出口；分析；Adobe Experience Platform Edge Network api;analytics
exl-id: b5e7a4d0-9aea-4e70-a7d6-b9aad09aaddf
source-git-commit: 422f859bef8faf292fd7e5fd8b6a8d31967421c1
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 2%

---

# Adobe Analyticsの操作

## 概要 {#overview}

Adobe Analyticsのデータ収集は、XDM データをAdobe Analyticsが理解できる形式に変換することで機能します。 複数の XDM フィールドは次のとおりです [自動的にマッピング](../edge/data-collection/adobe-analytics/automatically-mapped-vars.md) を Analytics 変数に追加します。

また、 [XDM 値の手動マッピング](../edge/data-collection/adobe-analytics/manually-mapping-variables.md) を従来の Analytics 変数に追加します。

Adobe Analyticsが Server API からデータを受け取れるようにするには、次の手順を実行する必要があります。 [データストリームの設定](../edge/fundamentals/datastreams.md#adobe-analytics-settings) イベントをAdobe Analyticsに転送するには、データストリーム設定ページでレポートスイート ID を入力します。

![Adobe Analytics Datastream 設定](assets/analytics-datastream.png)

## Adobe Analyticsの操作 {#interacting-analytics}

### API 形式 {#format}

```http
POST https://server.adobedc.net/v2/interact?dataStreamId={DATASTREAM_ID}
```

### リクエスト {#request}

以下のサンプルには、 `_experience.analytics` フィールドグループを使用します。 また、JSON ベースのデータレイヤーも含まれます。 これらのデータレイヤーは自動的にマッピングできませんが、 [データ収集用のデータ準備](../edge/fundamentals/datastreams.md#data-prep) を使用して、上記で参照されているフィールドグループを含むスキーマにこれらの値をマッピングできます。

ユーザーがこれらのフィールドにマッピングしたすべての値は、API リクエストに含まれる場合と同じように、適切な Analytics 値に自動的にマッピングされます。

```shell
curl -X POST "https://server.adobedc.net/v2/interact?dataStreamId={DATASTREAM_ID}" \
-H "Authorization: Bearer {TOKEN}" 
-H "x-gw-ims-org-id: {IMS_ORG_ID}" 
-H "x-api-key: {API_KEY}" 
-H "Content-Type: application/json" \
-d '{
   "event": {
      "xdm": {
         "eventType": "web.webpagedetails.pageViews",
         "web": {
            "webPageDetails": {
               "URL": "https://alloystore.dev",
               "name": "Home Page"
            },
            "webReferrer": {
               "URL": ""
            }
         },
         "device": {
            "screenHeight": 1440,
            "screenWidth": 3440,
            "screenOrientation": "landscape"
         },
         "environment": {
            "type": "browser",
            "browserDetails": {
               "viewportWidth": 3440,
               "viewportHeight": 1440
            }
         },
         "placeContext": {
            "localTime": "2022-03-22T22:45:21.193-06:00",
            "localTimezoneOffset": 360
         },
         "timestamp": "2022-03-23T04:45:21.193Z",
         "implementationDetails": {
            "name": "https://ns.adobe.com/experience/alloy/reactor",
            "version": "2.8.0+2.9.0",
            "environment": "browser"
         },
         "_experience": {
            "analytics": {
               "customDimensions": {
                  "eVars": {
                     "eVar14": "eVar14"
                  }
               },
               "event1to100": {
                  "event13": {
                     "value": 1
                  },
                  "event14": {
                     "value": 2
                  }
               }
            }
         }
      }
   },
   "data": {
      "page": {
         "pageInfo": {
            "pageName": "Promotions",
            "siteSection": "Home"
         },
         "promos": {
            "heroPromos": "purse,shoes,sunglasses"
         },
         "customVariables": {
            "testGroup": "orange/black theme"
         },
         "events": {
            "homePage": true
         },
         "products": [
            {
               "productSKU": "abc123",
               "productName": "shirt"
            }
         ]
      }
   }
}'
```

### レスポンス {#response}

```json
{
   "requestId": "d2ad6364-5675-4a86-ba41-50e7a4c4a299",
   "handle": []
}
```
