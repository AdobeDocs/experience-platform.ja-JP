---
title: Adobe Analytics の操作
description: Edge Networkサーバー API を使用してAdobe Analyticsとやり取りする方法を説明します。
exl-id: b5e7a4d0-9aea-4e70-a7d6-b9aad09aaddf
source-git-commit: 5de1ec17b78c97be21c0d2afd6f0b119a6074b6f
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 8%

---

# Adobe Analytics の操作

## 概要 {#overview}

Adobe Analyticsのデータ収集は、XDM データをAdobe Analyticsが理解できる形式に変換することで機能します。 複数の XDM フィールドが Analytics 変数に [ 自動的にマッピング ](https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/variable-mapping.html) されます。 また、XDM 値を従来の Analytics 変数に手動でマッピングすることもできます。

Adobe Analyticsが Server API からデータを受信できるようにするには、データストリーム設定ページでレポートスイート ID を入力してイベントをAdobe Analyticsに転送する [ データストリームを設定する ](../datastreams/overview.md#adobe-analytics-settings) 必要があります。

![Adobe Analytics データストリーム設定 ](assets/analytics-datastream.png)

## Adobe Analytics の操作 {#interacting-analytics}

### API 形式 {#format}

```http
POST /ee/v2/interact?dataStreamId={DATASTREAM_ID}
```

### リクエスト {#request}

以下のサンプルには、`_experience.analytics` フィールドグループから自動的にマッピングされた複数の値が含まれています。 また、JSON ベースのデータレイヤーも含まれます。 これらのデータレイヤーは自動的にマッピングできませんが、[ データ収集のデータ準備 ](../datastreams/data-prep.md) を使用して、上記で参照されたフィールドグループを含むスキーマにこれらの値をマッピングできます。

ユーザーがこれらのフィールドにマッピングするすべての値は、API リクエストに含まれているかのように、適切な Analytics 値に自動的にマッピングされます。

```shell
curl -X POST "https://server.adobedc.net/ee/v2/interact?dataStreamId={DATASTREAM_ID}" \
-H "Authorization: Bearer {TOKEN}" 
-H "x-gw-ims-org-id: {ORG_ID}" 
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

### 回答 {#response}

```json
{
   "requestId": "d2ad6364-5675-4a86-ba41-50e7a4c4a299",
   "handle": []
}
```
