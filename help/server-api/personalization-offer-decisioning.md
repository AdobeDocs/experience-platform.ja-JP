---
title: offer decisioning
description: Server API を使用して、パーソナライズされたエクスペリエンスをOffer decisioning経由で配信およびレンダリングする方法を説明します
keywords: パーソナライゼーション；server api;Adobe Experience Platform Edge Network;パーソナライゼーションの取得；target;offer decisioning;
source-git-commit: 59cb43007c4a7ff125738c21064381cf833063b2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 2%

---


# offer decisioning

## 概要 {#overview}

Edge Network Server API は、 [offer decisioning](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/get-started-decision/starting-offer-decisioning.html?lang=en) を web チャネルに追加します。

[!DNL Offer Decisioning] は、アクティビティとパーソナライゼーションエクスペリエンスを作成、アクティブ化、配信するための、非視覚的なインターフェイスをサポートしています。

## データストリームの設定 {#configure-your-datastream}

Server API をOffer decisioningと組み合わせて使用する前に、データストリーム設定でAdobe Experience Platformのパーソナライゼーションを有効にし、 **[!UICONTROL offer decisioning]** オプション。

詳しくは、 [データストリームへのサービスの追加に関するガイド](../edge/datastreams/overview.md#adobe-experience-platform-settings)offer decisioning

![データストリームサービス設定画面を示す UI 画像 (Offer decisioningを選択 )](assets/aep-od-datastream.png)

## オーディエンスの作成 {#audience-creation}

[!DNL Offer Decisioning] は、Adobe Experience Platform Segmentation Service を使用してオーディエンスを作成します。 次のドキュメントを参照してください： [!DNL Segmentation Service] [ここ](../segmentation/home.md).

## 決定範囲の定義 {#creating-decision-scopes}

この [!DNL Offer Decision Engine] はAdobe Experience Platformデータを使用し、 [リアルタイム顧客プロファイル](../profile/home.md)、 [!DNL Offer Library]：適切な顧客およびチャネルに適切なタイミングでオファーを配信する。

詳しくは、 [!DNL Offer Decisioning Engine]、該当する [ドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/get-started-decision/starting-offer-decisioning.html).

後 [データストリームの設定](#configure-your-datastream)に値を入力する場合は、パーソナライゼーションキャンペーンで使用する決定範囲を定義する必要があります。

[決定範囲](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/create-manage-activities/create-offer-activities.html?lang=en#add-decision-scopes) は、 [!DNL Offer Decisioning Service] オファーを提案する際に使用します。

**決定範囲 JSON**

```json
{
   "activityId":"xcore:offer-activity:11cfb1fa93381aca",
   "placementId":"xcore:offer-placement:1175009612b0100c"
}
```

**決定範囲 Base64 でエンコードされた文字列**

```shell
"eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="
```

オファーとコレクションを作成したら、 [決定範囲](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/create-manage-activities/create-offer-activities.html?lang=en#add-decision-scopes).

Base64 でエンコードされた決定範囲をコピーします。 これは、 `query` オブジェクトに含める必要があります。

![決定範囲をハイライト表示し、Offer decisioningUI を示す UI 画像。](assets/decision-scope.png)

```json
"query":{
   "personalization":{
      "decisionScopes":[
         "eyJ4ZG06YWN0aXZpdHlJZCI6Inhjb3JlOm9mZmVyLWFjdGl2aXR5OjE0ZWZjYTg5NDE4OTUxODEiLCJ4ZG06cGxhY2VtZW50SWQiOiJ4Y29yZTpvZmZlci1wbGFjZW1lbnQ6MTJkNTQ0YWU1NGU3ZTdkYiJ9"
      ]
   }
}
```

## API 呼び出しの例 {#api-example}

**API 形式**

```http
POST /ee/v2/interact
```

### リクエスト {#request}

完全な XDM オブジェクト、データオブジェクトおよびOffer decisioningクエリを含む完全なリクエストを以下に示します。

>[!NOTE]
>
>この `xdm` および `data` オブジェクトはオプションで、Offer decisioningに必要なのは、これらのオブジェクトのいずれかにフィールドを使用する条件を持つセグメントを作成した場合のみです。

```shell
curl -X POST 'https://server.adobedc.net/ee/v2/interact?dataStreamId={DATASTREAM_ID}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org: {ORG_ID}' \
--header 'Authorization: Bearer {TOKEN}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "event": {
        "xdm": {
            "eventType": "web.webpagedetails.pageViews",
            "identityMap": {
                "ECID": [
                    {
                        "id": "05907638112924484241029082405297151763",
                        "authenticatedState": "ambiguous",
                        "primary": true
                    }
                ]
            },
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
                "version": "1.0",
                "environment": "serverapi"
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
            },
            "__adobe.target": {
                "profile.eyeColor": "brown",
                "profile.hairColor": "brown"
            }
        }
    },
    "query": {
        "personalization": {
            "decisionScopes": [
                "eyJ4ZG06YWN0aXZpdHlJZCI6Inhjb3JlOm9mZmVyLWFjdGl2aXR5OjE0ZWZjYTg5NDE4OTUxODEiLCJ4ZG06cGxhY2VtZW50SWQiOiJ4Y29yZTpvZmZlci1wbGFjZW1lbnQ6MTJkNTQ0YWU1NGU3ZTdkYiJ9"
            ]
        }
    }
}'
```

### 応答 {#response}

Edge ネットワークは、次のような応答を返します。

```json
{
   "requestId":"b375077d-7e1d-4c18-b7d3-e4da0fb4fbc5",
   "handle":[
      {
         "payload":[
            
         ],
         "type":"personalization:decisions",
         "eventIndex":0
      },
      {
         "payload":[
            {
               "id":"120d5db7-181c-42c5-8653-88b3cd3e1e69",
               "scope":"eyJ4ZG06YWN0aXZpdHlJZCI6Inhjb3JlOm9mZmVyLWFjdGl2aXR5OjE0ZWZjYTg5NDE4OTUxODEiLCJ4ZG06cGxhY2VtZW50SWQiOiJ4Y29yZTpvZmZlci1wbGFjZW1lbnQ6MTJkNTQ0YWU1NGU3ZTdkYiJ9",
               "activity":{
                  "id":"xcore:offer-activity:14efca8941895181",
                  "etag":"1"
               },
               "placement":{
                  "id":"xcore:offer-placement:12d544ae54e7e7db",
                  "etag":"1"
               },
               "items":[
                  {
                     "id":"xcore:personalized-offer:14efc848a3577d92",
                     "etag":"2",
                     "schema":"https://ns.adobe.com/experience/offer-management/content-component-json",
                     "data":{
                        "id":"xcore:personalized-offer:14efc848a3577d92",
                        "format":"application/json",
                        "language":[
                           "en-us"
                        ],
                        "content":"{\n\t\"ODEFirstTest\" : \"Personalizaton Content\"\n}",
                        "characteristics":{
                           "reporting":"testRequest"
                        }
                     }
                  }
               ]
            }
         ],
         "type":"personalization:decisions",
         "eventIndex":0
      },
      {
         "payload":[
            {
               "key":"kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_identity",
               "value":"CiYwNTkwNzYzODExMjkyNDQ4NDI0MTAyOTA4MjQwNTI5NzE1MTc2M1IOCLr6xb39LxgBKgNPUjLwAbr6xb39Lw==",
               "maxAge":34128000
            }
         ],
         "type":"state:store"
      }
   ]
}
```

訪問者が、に送信されたデータに基づいてパーソナライゼーションアクティビティの対象となる場合 [!DNL Offer Decisioning]関連するアクティビティのコンテンツは、 `handle` オブジェクト ( タイプは `personalization:decisions`.

その他のコンテンツは、 `handle` オブジェクトも同様です。 その他のコンテンツタイプは、 [!DNL Offer Decisioning] パーソナライゼーション。 訪問者が複数のアクティビティに該当する場合、それらは配列に含まれます。

次の表に、応答のその部分の主な要素を示します。

| プロパティ | 説明 | 例 |
|---|---|---|
| `scope` | 返された提案されたオファーに関連付けられた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `activity.id` | オファーアクティビティの一意の ID。 | `"id": "xcore:offer-activity:11cfb1fa93381aca"` |
| `placement.id` | オファー配置の一意の ID。 | `"id": "xcore:offer-placement:1175009612b0100c"` |
| `items.id` | 提案されたオファーの一意の ID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーの一意の ID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーのコンテンツに関連付けられた言語の配列。 | `"language": [ "en-US" ]` |
| `content` | 提案されたオファーに関連付けられた、文字列の形式のコンテンツ。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツ（URL 形式）。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | 提案されたオファーに関連付けられた特性を含む JSON オブジェクト。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

