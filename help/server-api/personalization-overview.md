---
title: パーソナライゼーションの概要
description: Adobe Experience Platform Edge Network Server API を使用して、パーソナライズされたコンテンツをAdobeのパーソナライゼーションソリューションから取得する方法について説明します。
exl-id: 11be9178-54fe-49d0-b578-69e6a8e6ab90
source-git-commit: 378f222b5c673632ce5792c52fc32410106def37
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 10%

---

# パーソナライゼーションの概要

を使用 [!DNL Server API]を使用すると、次のようなパーソナライズされたコンテンツをAdobeパーソナライゼーションソリューションから取得できます。 [Adobe Target](https://business.adobe.com/jp/products/target/adobe-target.html) および [offer decisioning](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/starting-offer-decisioning.html?lang=en).

また、 [!DNL Server API] では、Adobe Experience Platformのパーソナライゼーションの宛先 ( [Adobe Target](../destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーション接続](../destinations/catalog/personalization/custom-personalization.md). 同じページと次のページのパーソナライゼーション用にExperience Platformを設定する方法については、 [専用ガイド](../destinations/ui/activate-edge-personalization-destinations.md).

Server API を使用する場合は、パーソナライゼーションエンジンから提供される応答と、サイト上でのコンテンツのレンダリングに使用されるロジックを統合する必要があります。 とは異なり、 [Web SDK](../edge/home.md)、 [!DNL Server API] には、が返したコンテンツを自動的に適用するメカニズムがありません。 [!DNL Adobe Target] および [!DNL Offer Decisioning].

## 用語 {#terminology}

Adobeのパーソナライゼーションソリューションを使用する前に、次の概念を理解しておく必要があります。

* **オファー**：オファーは、オファーを表示する資格のあるユーザーを指定するルールが関連付けられているマーケティングメッセージです。
* **決定**:決定（旧称：オファーアクティビティ）は、オファーの選択を通知します。
* **スキーマ**:決定のスキーマは、返されるオファーのタイプを通知します。
* **範囲**:決定の範囲。
   * Adobe Targetでは、 [!DNL mbox]. この [!DNL global mbox] が `__view__` 範囲
   * の場合 [!DNL Offer Decisioning]これらは、offer decisioningサービスがオファーの提案に使用するアクティビティ ID と配置 ID を含む、Base64 でエンコードされた JSON の文字列です。

## この `query` object {#query-object}

パーソナライズされたコンテンツを取得するには、リクエスト例の明示的なリクエストクエリオブジェクトが必要です。 クエリオブジェクトの形式は次のとおりです。

```json
{
   "query":{
      "personalization":{
         "schemas":[
            "https://ns.adobe.com/personalization/html-content-item",
            "https://ns.adobe.com/personalization/json-content-item",
            "https://ns.adobe.com/personalization/redirect-item",
            "https://ns.adobe.com/personalization/dom-action"
         ],
         "decisionScopes":[
            "alloyStore",
            "siteWide",
            "__view__",
            "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ"
         ]
      }
   }
}
```

| 属性 | タイプ | 必須／オプション | 説明 |
| --- | --- | --- | ---|
| `schemas` | `String[]` | Target のパーソナライゼーションに必要です。 offer decisioningのオプション。 | 返されるオファーのタイプを選択するための、決定で使用されるスキーマのリスト。 |
| `scopes` | `String[]` | オプション | 決定範囲のリスト。 リクエストあたり最大 30 件。 |

## handle オブジェクト {#handle}

パーソナライゼーションソリューションから取得したパーソナライズされたコンテンツは、 `personalization:decisions` handle（ペイロードに次の形式を持つ）

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
| `payload.scope` | 文字列 | 提案されたオファーを導いた決定範囲。 |
| `payload.scopeDetails.decisionProvider` | 文字列 | に設定 `TGT` Adobe Targetを使用する場合。 |
| `payload.scopeDetails.activity.id` | 文字列 | オファーアクティビティの一意の ID。 |
| `payload.scopeDetails.experience.id` | 文字列 | オファー配置の一意の ID。 |
| `items[].id` | 文字列 | オファー配置の一意の ID。 |
| `items[].data.id` | 文字列 | 提案されたオファーの ID。 |
| `items[].data.schema` | 文字列 | 提案されたオファーに関連付けられたコンテンツのスキーマ。 |
| `items[].data.format` | 文字列 | 提案されたオファーに関連付けられたコンテンツの形式。 |
| `items[].data.language` | 文字列 | 提案されたオファーのコンテンツに関連付けられた言語の配列。 |
| `items[].data.content` | 文字列 | 提案されたオファーに関連付けられた、文字列の形式のコンテンツ。 |
| `items[].data.selector` | 文字列 | HTMLセレクターは、DOM アクションオファーのターゲット DOM 要素を識別するために使用されます。 |
| `items[].data.prehidingSelector` | 文字列 | DOMHTMLオファーの処理中に非表示にする DOM 要素を識別するために使用されるアクションセレクター。 |
| `items[].data.deliveryUrl` | 文字列 | 提案されたオファーに関連付けられた画像コンテンツ（URL 形式）。 |
| `items[].data.characteristics` | 文字列 | 提案されたオファーに JSON オブジェクトの形式で関連付けられた特性。 |

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
| `requestId` | 文字列 | × | 外部リクエストトレース ID を指定します。 何も指定されない場合、Edge ネットワークはユーザーに代わって 1 つを生成し、応答の本文/ヘッダーに返します。 |

### 応答 {#response}

を返します。 `200 OK` ステータスと 1 つ以上の `Handle` オブジェクトを作成します。

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

事前に取得されたコンテンツまたはビューがエンドユーザーに訪問またはレンダリングされた場合に、通知を発行する必要があります。 適切な範囲で通知を実行するには、必ず、対応する `id` を設定します。

右側に通知 `id` レポートを正しく反映するには、対応するスコープを実行する必要があります。

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
| `requestId` | `String` | いいえ | 外部リクエストトレース ID。 何も指定されない場合、Edge ネットワークはユーザーに代わって 1 つを生成し、応答の本文/ヘッダーに返します。 |
| `silent` | `Boolean` | いいえ | Edge ネットワークが `204 No Content` 空のペイロードを持つ応答。 重大なエラーは、対応する HTTP ステータスコードとペイロードを使用して報告されます。 |

### 応答 {#notifications-response}

正常な応答は、次のステータスのいずれかと、 `requestID` リクエストで何も指定されていない場合。

* `202 Accepted` リクエストが正常に処理されたとき。
* `204 No Content` リクエストが正常に処理され、 `silent` パラメータがに設定されました `true`;
* `400 Bad Request` リクエストの形式が正しくなかった場合（必須のプライマリ id が見つからなかった場合など）。

```json
{
  "requestId": "f567a988-4b3c-45a6-9ed8-f283188a445e"
}
```
