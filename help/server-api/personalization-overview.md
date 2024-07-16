---
title: パーソナライゼーションの概要
description: Adobe Experience Platform Edge Networkサーバー API を使用して、Adobeのパーソナライゼーションソリューションからパーソナライズされたコンテンツを取得する方法を説明します。
exl-id: 11be9178-54fe-49d0-b578-69e6a8e6ab90
source-git-commit: ae6c6d21b1eea900d01be3287827296071429d30
workflow-type: tm+mt
source-wordcount: '735'
ht-degree: 10%

---

# パーソナライゼーションの概要

[!DNL Server API] を使用すると、[Adobe Target、[Adobe Journey Optimizer](https://business.adobe.com/jp/products/target/adobe-target.html)、[Offer decisioning](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/starting-offer-decisioning.html?lang=ja) など、Adobeのパーソナライゼーションソリューションからパーソナライズされたコンテンツを取得 ](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home) きます。

さらに、[!DNL Server API] は、[Adobe Target](../destinations/catalog/personalization/adobe-target-connection.md) や [ カスタムパーソナライゼーション接続 ](../destinations/catalog/personalization/custom-personalization.md) などのAdobe Experience Platform パーソナライゼーションの宛先を通じて、同じページおよび次のページのパーソナライゼーション機能を強化します。 同じページと次のページのパーソナライゼーション用にExperience Platformを設定する方法については、[ 専用ガイド ](../destinations/ui/activate-edge-personalization-destinations.md) を参照してください。

Server API を使用する場合、パーソナライゼーションエンジンから提供される応答を、サイト上でコンテンツをレンダリングするために使用されるロジックと統合する必要があります。 [Web SDK](../web-sdk/home.md) とは異なり、[!DNL Server API] には、Adobeパーソナライゼーションソリューションから返されたコンテンツを自動的に適用するメカニズムはありません。

## 用語 {#terminology}

Adobeのパーソナライゼーションソリューションを使用する前に、以下の概念を理解しておく必要があります。

* **オファー**：オファーは、オファーを表示する資格のあるユーザーを指定するルールが関連付けられているマーケティングメッセージです。
* **決定**：決定（旧称：オファーアクティビティ）は、オファーの選択を通知します。
* **スキーマ**：決定のスキーマは、返されるオファーのタイプを通知します。
* **範囲**：決定の範囲。
   * Adobe Targetでは、これが [!DNL mbox] です。 [!DNL global mbox] は `__view__` の範囲です
   * [!DNL Offer Decisioning]：これらは、offer decisioningサービスでオファーを提案するために使用するアクティビティ ID とプレースメント ID を含む、Base64 でエンコードされた JSON の文字列です。

## `query` オブジェクト {#query-object}

パーソナライズされたコンテンツを取得するには、リクエスト例に明示的なリクエストクエリオブジェクトが必要です。 クエリオブジェクトの形式は次のとおりです。

```json
{
  "query": {
    "personalization": {
      "schemas": [
        "https://ns.adobe.com/personalization/html-content-item",
        "https://ns.adobe.com/personalization/json-content-item",
        "https://ns.adobe.com/personalization/redirect-item",
        "https://ns.adobe.com/personalization/dom-action"
      ],
      "decisionScopes": [
        "alloyStore",
        "siteWide",
        "__view__",
        "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ"
      ],
      "surfaces": [
        "web://mywebpage.html/",
        "web://mywebpage.html/#sample-json-content"
      ]
    }
  }
}
```



| 属性 | タイプ | 必須／オプション | 説明 |
| --- | --- | --- | ---|
| `schemas` | `String[]` | Target のパーソナライゼーションに必須。 offer decisioningの場合はオプション。 | 決定で使用されるスキーマのリストで、返されるオファーのタイプを選択します。 |
| `scopes` | `String[]` | オプション | 決定範囲のリスト。 リクエストあたり最大 30。 |

## handle オブジェクト {#handle}

パーソナライゼーションソリューションから取得されたパーソナライズされたコンテンツは、`personalization:decisions` ハンドルで表示されます。このハンドルは、ペイロードとして次の形式を持ちます。

```json
{
   "type":"personalization:decisions",
   "payload":[
      {
         "id":"AT:eyJhY3Rpdml0eUlkIjoiMTMxMDEwIiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
         "scope":"__view__",
         "scopeDetails":{
            "decisionProvider":"TGT",
            "activity":{
               "id":"131010"
            },
            "experience":{
               "id":"0"
            },
            "strategies":[
               {
                  "algorithmID":"0",
                  "trafficType":"0"
               }
            ]
         },
         "items":[
            {
               "id":"0",
               "schema":"https://ns.adobe.com/personalization/dom-action",
               "meta":{
                  "offer.name":"Default Content",
                  "experience.id":"0",
                  "activity.name":"Luma target reporting",
                  "activity.id":"131010",
                  "experience.name":"Experience A",
                  "option.id":"2",
                  "offer.id":"0"
               },
               "data":{
                  "type":"setHtml",
                  "format":"application/vnd.adobe.target.dom-action",
                  "content":"Customer Service not chrome",
                  "selector":"HTML > BODY > DIV.page-wrapper:eq(0) > FOOTER.page-footer:eq(0) > DIV.footer:eq(0) > DIV.links:eq(0) > DIV.widget:eq(0) > UL.footer:eq(0) > LI.nav:eq(1) > A:nth-of-type(1)",
                  "prehidingSelector":"HTML > BODY > DIV:nth-of-type(1) > FOOTER:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > UL:nth-of-type(1) > LI:nth-of-type(2) > A:nth-of-type(1)"
               }
            }
         ]
      }
   ]
}
```

| 属性 | タイプ | 説明 |
| --- | --- | --- |
| `payload.id` | 文字列 | 決定 ID。 |
| `payload.scope` | 文字列 | 提案されたオファーにつながった決定範囲。 |
| `payload.scopeDetails.decisionProvider` | 文字列 | Adobe Targetを使用する場合は、`TGT` に設定します。 |
| `payload.scopeDetails.activity.id` | 文字列 | オファーアクティビティの一意の ID。 |
| `payload.scopeDetails.experience.id` | 文字列 | オファープレースメントの一意の ID。 |
| `items[].id` | 文字列 | オファープレースメントの一意の ID。 |
| `items[].data.id` | 文字列 | 提案されたオファーの ID。 |
| `items[].data.schema` | 文字列 | 提案されたオファーに関連付けられたコンテンツのスキーマ。 |
| `items[].data.format` | 文字列 | 提案されたオファーに関連付けられたコンテンツの形式。 |
| `items[].data.language` | 文字列 | 提案されたオファーのコンテンツに関連付けられた言語の配列。 |
| `items[].data.content` | 文字列 | 提案されたオファーに関連付けられたコンテンツ（文字列の形式）。 |
| `items[].data.selector` | 文字列 | DOM アクションオファーのターゲット DOMHTMLを識別するために使用される要素セレクター。 |
| `items[].data.prehidingSelector` | 文字列 | DOM アクションオファーの処理中に非表示にする DOMHTMLを識別するために使用される要素セレクター。 |
| `items[].data.deliveryUrl` | 文字列 | 提案されたオファーに関連付けられた画像コンテンツ（URL 形式）。 |
| `items[].data.characteristics` | 文字列 | 提案されたオファーに関連付けられた特性（JSON オブジェクト形式）。 |

## サンプル API 呼び出し {#sample-call}

**API 形式**

```http
POST /ee/v2/interact
```

### リクエスト {#request}

```shell
curl -X POST "https://server.adobedc.net/ee/v2/interact?dataStreamId={DATASTREAM_ID}"
-H "Authorization: Bearer {TOKEN}"
-H "x-gw-ims-org-id: {ORG_ID}"
-H "x-api-key: {API_KEY}"
-H "Content-Type: application/json"
-d '{
   "event":{
      "xdm":{
         "identityMap":{
            "Email_LC_SHA256":[
               {
                  "id":"0c7e6a405862e402eb76a70f8a26fc732d07c32931e9fae9ab1582911d2e8a3b",
                  "primary":true
               }
            ]
         },
         "eventType":"web.webpagedetails.pageViews",
         "web":{
            "webPageDetails":{
               "URL":"https://alloystore.dev/",
               "name":"home-demo-Home Page"
            }
         },
         "timestamp":"2021-08-09T14:09:20.859Z"
      }
   },
   "query":{
      "personalization":{
         "schemas":[
            "https://ns.adobe.com/personalization/html-content-item",
            "https://ns.adobe.com/personalization/json-content-item",
            "https://ns.adobe.com/personalization/redirect-item",
            "https://ns.adobe.com/personalization/dom-action"
         ],
         "decisionScopes":[
            "__view__",
            "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ"
         ]
      }
   }
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `configId` | 文字列 | ○ | データストリーム ID。 |
| `requestId` | 文字列 | × | 外部リクエストトレース ID を指定します。 何も指定されない場合、Edge Networkによって生成され、応答本文/ ヘッダーに返されます。 |

### 応答 {#response}

データストリーム設定で有効になっているエッジサービスに応じて、`200 OK` ステータスおよび 1 つ以上の `Handle` オブジェクトを返します。

```json
{
   "requestId":"da20d11d-adac-458c-91ac-15bf4e420a15",
   "handle":[
      {
         "payload":[
            {
               "id":"AT:eyJhY3Rpdml0eUlkIjoiMTMxMDEwIiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
               "scope":"__view__",
               "scopeDetails":{
                  "decisionProvider":"TGT",
                  "activity":{
                     "id":"131010"
                  },
                  "experience":{
                     "id":"0"
                  },
                  "strategies":[
                     {
                        "algorithmID":"0",
                        "trafficType":"0"
                     }
                  ]
               },
               "items":[
                  {
                     "id":"0",
                     "schema":"https://ns.adobe.com/personalization/dom-action",
                     "meta":{
                        "offer.name":"Default Content",
                        "experience.id":"0",
                        "activity.name":"Luma target reporting",
                        "activity.id":"131010",
                        "experience.name":"Experience A",
                        "option.id":"2",
                        "offer.id":"0"
                     },
                     "data":{
                        "type":"setHtml",
                        "format":"application/vnd.adobe.target.dom-action",
                        "content":"Customer Service not chrome",
                        "selector":"HTML > BODY > DIV.page-wrapper:eq(0) > FOOTER.page-footer:eq(0) > DIV.footer:eq(0) > DIV.links:eq(0) > DIV.widget:eq(0) > UL.footer:eq(0) > LI.nav:eq(1) > A:nth-of-type(1)",
                        "prehidingSelector":"HTML > BODY > DIV:nth-of-type(1) > FOOTER:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > UL:nth-of-type(1) > LI:nth-of-type(2) > A:nth-of-type(1)"
                     }
                  }
               ]
            }
         ],
         "type":"personalization:decisions"
      }
   ]
}
```

## 通知 {#notifications}

通知は、プリフェッチされたコンテンツまたはビューがエンドユーザーに訪問された、またはエンドユーザーにレンダリングされた場合に実行する必要があります。 適切な範囲に対して通知を送信するには、各範囲に対応する `id` を必ず追跡します。

レポートが正しく反映されるためには、対応する範囲の適切な `id` を持つ通知を実行する必要があります。

**API 形式**

```http
POST /ee/v2/collect
```

### リクエスト {#notifications-request}

```shell
curl -X POST "https://server.adobedc.net/ee/v2/collect?dataStreamId={DATASTREAM_ID}" 
-H "Authorization: Bearer {TOKEN}" 
-H "x-gw-ims-org-id: {ORG_ID}" 
-H "x-api-key: {API_KEY}"
-H "Content-Type: application/json"
-d '{
   "events":[
      {
         "xdm":{
            "identityMap":{
               "Email_LC_SHA256":[
                  {
                     "id":"0c7e6a405862e402eb76a70f8a26fc732d07c32931e9fae9ab1582911d2e8a3b",
                     "primary":true
                  }
               ]
            },
            "eventType":"web.webpagedetails.pageViews",
            "web":{
               "webPageDetails":{
                  "URL":"https://alloystore.dev/",
                  "name":"home-demo-Home Page"
               }
            },
            "timestamp":"2021-08-09T14:09:20.859Z",
            "_experience":{
               "decisioning":{
                  "propositions":[
                     {
                        "id":"AT:eyJhY3Rpdml0eUlkIjoiMTMxMDEwIiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
                        "scope":"__view__",
                        "items":[
                           {
                              "id":"0"
                           }
                        ]
                     }
                  ]
               }
            }
         }
      }
   ]
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `dataStreamId` | `String` | ○ | データ収集エンドポイントで使用されるデータストリームの ID。 |
| `requestId` | `String` | × | 外部要求トレース ID です。 何も指定されない場合、Edge Networkによって生成され、応答本文/ ヘッダーに返されます。 |
| `silent` | `Boolean` | × | Edge Networkが空のペイロードを持つ `204 No Content` 応答を返す必要があるかどうかを示すオプションのブール値パラメーター。 重大なエラーは、対応する HTTP ステータスコードとペイロードを使用して報告されます。 |

### 応答 {#notifications-response}

応答が成功すると、次のいずれかのステータスと、リクエストで何も指定されなかった場合の `requestID` が返されます。

* リク `202 Accepted` ストが正常に処理された日時。
* リクエストが正常に処理され、`silent` パラメーターが `true` に設定された `204 No Content`。
* リクエストの形式が正しくなかった場合（必須のプライマリ ID が見つからなかったなど）の `400 Bad Request`。

```json
{
  "requestId": "f567a988-4b3c-45a6-9ed8-f283188a445e"
}
```
