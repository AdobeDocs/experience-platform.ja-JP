---
title: Adobe Targetを使用したパーソナライゼーション
description: Server API を使用して、Adobe Targetで作成したパーソナライズされたエクスペリエンスを提供し、レンダリングする方法を説明します。
exl-id: c9e2f7ef-5022-4dc4-82b4-ecc210f27270
source-git-commit: 091d5440d7346861b7c882fa0a17bd03d528e438
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 3%

---

# Adobe Targetを使用したパーソナライゼーション

## 概要 {#overview}

Edge Network Server API を使用すると、Adobe Targetで作成されたパーソナライズされたエクスペリエンスを、 [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en).

>[!IMPORTANT]
>
>を通じて作成されたパーソナライゼーションエクスペリエンス [Target Visual Experience Composer(VEC)](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja) は、Server API で完全にはサポートされていません。 Server API では、 **取得** アクティビティが VEC で作成されたが、Server API では作成されない **レンダー** VEC で作成されたアクティビティ。 VEC で作成されたアクティビティをレンダリングする場合は、 [ハイブリッドパーソナライズ](../edge/personalization/hybrid-personalization.md) Web SDK と Edge Network Server API を使用して、

## データストリームの設定 {#configure-your-datastream}

Server API をAdobe Targetと組み合わせて使用する前に、データストリーム設定でAdobe Targetのパーソナライゼーションを有効にする必要があります。

詳しくは、 [データストリームへのサービスの追加に関するガイド](../edge/datastreams/overview.md#adobe-target-settings)(Adobe Targetを有効にする方法について詳しくは、を参照 )。

データストリームを設定する際に、 [!DNL Property Token], [!DNL Target Environment ID]、および [!DNL Target Third Party ID Namespace].

![Adobe Targetを選択した状態で、データストリームサービス設定画面を示す UI 画像](assets/target-datastream.png)


## カスタムパラメーター {#custom-parameters}

のほとんどのフィールド [!DNL XDM] 各リクエストの一部は、ドット表記にシリアル化され、カスタムまたはとして Target に送信されます。 [!DNL mbox] パラメーター。


### 例 {#custom-parameters-example}

次の XDM サンプルを考えてみましょう。

```json
"xdm":{
   "marketing":{
      "campaignGroup":"winter22",
      "campaignName":"homeOwnerPromo22",
      "trackingCode":"hop22"
   }
}
```

Target でオーディエンスを作成する場合、次の値がカスタムパラメーターとして使用できます。

* `xdm.marketing.campaignGroup`
* `xdm.marketing.campaignName`
* `xdm.marketing.trackingCode`

## Target プロファイルの更新 {#profile-update}

この [!DNL Server API] では、Target プロファイルの更新を許可します。 Target プロファイルを更新するには、必ず `data` 次の形式のリクエストの一部：

```json
"data":  {
    "__adobe.target": {
        "profile.eyeColor": "brown",
        "profile.hairColor": "brown"
    }
}
```

## Target アクティビティに対するクエリ {#querying-target-activities}

### スキーマ {#schemas}

リクエストのクエリ部分は、Target から返されるコンテンツを決定します。 以下 `personalization` オブジェクト `schemas` は、Target から返されるコンテンツのタイプを決定します。

どのようなオファーを取得するかが不明な場合は、パーソナライゼーションクエリに次の 4 つのスキーマをすべて Edge ネットワークに含める必要があります。

* **HTMLベースのオファー：**
https://ns.adobe.com/personalization/html-content-item
* **JSON ベースのオファー：**
https://ns.adobe.com/personalization/json-content-item
* **Target のリダイレクトオファー**
https://ns.adobe.com/personalization/redirect-item
* **Target DOM 操作オファー**
https://ns.adobe.com/personalization/dom-action

### 決定範囲 {#decision-scopes}

Adobe Target [!DNL mbox] 名前は `decisionScopes` 配列を使用して、適切なコンテンツを返します。

#### 例 {#decision-scopes-example}

以下の例では、4 つのオファータイプすべてが、「 」という Target アクティビティと共にリクエストされます。 `serverapimbox`.

```json
"query":{
   "personalization":{
      "schemas":[
         "https://ns.adobe.com/personalization/html-content-item",
         "https://ns.adobe.com/personalization/json-content-item",
         "https://ns.adobe.com/personalization/redirect-item",
         "https://ns.adobe.com/personalization/dom-action"
      ],
      "decisionScopes":[
         "serverapimbox"
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

完全な XDM オブジェクト、プロファイルパラメーター、および適切な Target クエリを含む完全なリクエストの概要を以下に示します。

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
            },
            "data": {
                "__adobe": {
                    "target": {
                        "profile.eyeColor": "brown",
                        "profile.hairColor": "brown",
                        "profile.shoeColor": "black"
                    }
                }
            }
        }
    },
    "query": {
        "personalization": {
            "schemas": [
                "https://ns.adobe.com/personalization/html-content-item",
                "https://ns.adobe.com/personalization/json-content-item",
                "https://ns.adobe.com/personalization/redirect-item",
                "https://ns.adobe.com/personalization/dom-action"
            ],
            "decisionScopes": [
                "serverapimbox"
            ]
        }
    }
}'
```

### 応答 {#response}

Edge ネットワークは、次のような応答を返します。

```json
{
   "requestId":"10959bbf-f83d-40e1-9521-d9145f19cdc5",
   "handle":[
      {
         "payload":[
            {
               "id":"AT:eyJhY3Rpdml0eUlkIjoiMTQwMjgxIiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
               "scope":"serverapimbox",
               "scopeDetails":{
                  "decisionProvider":"TGT",
                  "activity":{
                     "id":"140281"
                  },
                  "experience":{
                     "id":"0"
                  },
                  "strategies":[
                     {
                        "algorithmID":"0",
                        "trafficType":"0"
                     }
                  ],
                  "characteristics":{
                     "eventToken":"xycjBJlZhwVV5MN0kMkmoGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw=="
                  }
               },
               "items":[
                  {
                     "id":"282484",
                     "schema":"https://ns.adobe.com/personalization/json-content-item",
                     "meta":{
                        "offer.name":"/server_apiform/experiences/0/pages/0/zones/0/1648103551041",
                        "experience.id":"0",
                        "activity.name":"Server API Form",
                        "activity.id":"140281",
                        "experience.name":"Experience A",
                        "option.id":"2",
                        "offer.id":"282484"
                     },
                     "data":{
                        "id":"282484",
                        "format":"application/json",
                        "content":{
                           "value":"a/b json experience a",
                           "platform":"server"
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
               "value":"CiYwNTkwNzYzODExMjkyNDQ4NDI0MTAyOTA4MjQwNTI5NzE1MTc2M1IOCL-pwpv9LxgBKgNPUjLwAb-pwpv9Lw==",
               "maxAge":34128000
            }
         ],
         "type":"state:store"
      }
   ]
}
```

訪問者がAdobe Targetに送信されたデータに基づいてパーソナライゼーションアクティビティの対象となる場合、関連するアクティビティのコンテンツは `handle` オブジェクト ( タイプは `personalization:decisions`.

その他のコンテンツは、 `handle` 同様に。 その他のコンテンツタイプは、Target のパーソナライゼーションとは関係ありません。 訪問者が複数のアクティビティに該当する場合、各アクティビティは個別の `personalization` オブジェクトを指定します。

次の表に、応答のその部分の主な要素を示します。

| プロパティ | 説明 | 例 |
|---|---|---|
| `scope` | 提案されたオファーに結び付いた Target mbox 名。 | `"scope": "serverapimbox"` |
| `items[].schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 これは、パーソナライゼーションアクティビティの作成時に選択したアクティビティタイプに関連します。 | `"schema": "https://ns.adobe.com/personalization/json-content-item",` |
| `items[].meta.activity.id` | オファーアクティビティの一意の ID。 通常は 6 桁の数値です。 | `"activity.id": "140281"` |
| `items[].meta.activity.name` | ユーザー指定のオファーアクティビティの名前。 これは、アクティビティの作成手順で提供されます。 | `"activity.name": "Server API Form"` |
| `items[].meta.experience.id` | パーソナライゼーションエクスペリエンスの一意の ID。 | `"experience.id": "0"` |
| `items[].meta.experience.name` | パーソナライゼーションエクスペリエンスの一意の名前。 | `"experience.name": "Experience A"` |
| `items[].data.id` | 提案されたオファーの ID。 | `"id": "282484"` |
| `items[].data.format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format: "application/json` |
| `items[].data.content` | 提案されたオファーに関連付けられたコンテンツ。 これは、呼び出し元のアプリケーションのコンテンツのパーソナライゼーションに使用されます。 | `"content": "<CONTENT CONFIGURED IN TARGET>"` |
