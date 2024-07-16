---
title: Offer Decisioning を使用したパーソナライゼーション
description: Server API を使用して、Offer decisioningを介してパーソナライズされたエクスペリエンスを配信およびレンダリングする方法について説明します。
exl-id: 5348cd3e-08db-4778-b413-3339cb56b35a
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 3%

---

# Offer Decisioning を使用したパーソナライゼーション

## 概要 {#overview}

Edge Networkサーバー API は、[Offer decisioning](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/get-started-decision/starting-offer-decisioning.html?lang=ja) で管理されるパーソナライズされたエクスペリエンスを web チャネルに配信できます。

[!DNL Offer Decisioning] は、アクティビティとパーソナライゼーションエクスペリエンスを作成、アクティブ化および配信するための非ビジュアルインターフェイスをサポートしています。

## 前提条件 {#prerequisites}

[!DNL Offer Decisioning] を介したPersonalizationでは、統合を設定する前に、[Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja) へのアクセス権が必要です。

## データストリームの設定 {#configure-your-datastream}

Server API を Personalization と組み合わせて使用する前に、データストリームOffer decisioningでAdobe Experience Platformのパーソナライゼーションを有効にして、**[!UICONTROL Offer decisioning]** オプションを有効にする必要があります。

offer decisioningを有効にする方法について詳しくは、[ データストリームへのサービスの追加に関するガイド ](../datastreams/overview.md#adobe-experience-platform-settings) を参照してください。

![Offer decisioningが選択された状態のデータストリームサービス設定画面を示す UI 画像 ](assets/aep-od-datastream.png)

## オーディエンスの作成 {#audience-creation}

[!DNL Offer Decisioning] は、オーディエンスの作成にAdobe Experience Platform Segmentation Service を利用しています。 [!DNL Segmentation Service] のドキュメントについては、[ こちら ](../segmentation/home.md) を参照してください。

## 決定範囲の定義 {#creating-decision-scopes}

[!DNL Offer Decision Engine] は、Adobe Experience Platform データおよび [ リアルタイム顧客プロファイル ](../profile/home.md) を [!DNL Offer Library] と共に使用して、適切な顧客およびチャネルに適切なタイミングでオファーを提供します。

[!DNL Offer Decisioning Engine] について詳しくは、該当する [ ドキュメント ](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/get-started-decision/starting-offer-decisioning.html?lang=ja) を参照してください。

[ データストリームの設定 ](#configure-your-datastream) 後、パーソナライゼーションキャンペーンで使用する決定範囲を定義する必要があります。

[ 決定範囲 ](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/create-manage-activities/create-offer-activities.html#add-decision-scopes) は、オファーの提案時に [!DNL Offer Decisioning Service] ーザーに使用させるアクティビティ ID とプレースメント ID を含む、Base64 でエンコードされた JSON 文字列です。

**決定範囲 JSON**

```json
{
   "activityId":"xcore:offer-activity:11cfb1fa93381aca",
   "placementId":"xcore:offer-placement:1175009612b0100c"
}
```

**決定範囲 Base64 エンコードされた文字列**

```shell
"eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="
```

オファーとコレクションを作成したら、[ 決定範囲 ](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/create-manage-activities/create-offer-activities.html#add-decision-scopes) を定義する必要があります。

Base64 でエンコードされた決定範囲をコピーします。 Server API リクエストの `query` オブジェクトで使用します。

![ 決定範囲をハイライト表示したOffer decisioningUI を示す UI 画像。](assets/decision-scope.png)

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

完全な XDM オブジェクト、データオブジェクト、Offer decisioningクエリを含む完全なリクエストの概要を以下に示します。

>[!NOTE]
>
>`xdm` オブジェクトと `data` オブジェクトはオプションで、いずれかのオブジェクトのフィールドを使用する条件でセグメントを作成した場合にのみ、Offer decisioningのために必要です。

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

Edge Networkは、以下のような応答を返します。

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

[!DNL Offer Decisioning] に送信されたデータに基づいて、訪問者がパーソナライゼーションアクティビティの対象となる場合、関連するアクティビティのコンテンツは `handle` オブジェクトの下にあり、タイプは `personalization:decisions` です。

その他のコンテンツも `handle` オブジェクトの下に返されます。 その他のコンテンツタイプは、パーソナライゼーション [!DNL Offer Decisioning] は関係ありません。 訪問者が複数のアクティビティの対象となる場合、訪問者は配列に含まれます。

次の表に、応答のその部分の主な要素を説明します。

| プロパティ | 説明 | 例 |
|---|---|---|
| `scope` | 返された提案オファーに関連付けられた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `activity.id` | オファーアクティビティの一意の ID。 | `"id": "xcore:offer-activity:11cfb1fa93381aca"` |
| `placement.id` | オファープレースメントの一意の ID。 | `"id": "xcore:offer-placement:1175009612b0100c"` |
| `items.id` | 提案されたオファーの一意の ID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーの一意の ID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーのコンテンツに関連付けられた言語の配列。 | `"language": [ "en-US" ]` |
| `content` | 提案されたオファーに関連付けられたコンテンツ（文字列の形式）。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツ（URL 形式）。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | 提案されたオファーに関連付けられた特性を含む JSON オブジェクト。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |
