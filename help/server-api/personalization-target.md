---
title: Adobe Target を使用したパーソナライゼーション
description: Server API を使用して、Adobe Targetで作成されたパーソナライズされたエクスペリエンスを配信およびレンダリングする方法について説明します。
exl-id: c9e2f7ef-5022-4dc4-82b4-ecc210f27270
source-git-commit: ddffe9bf30741b457f7de1099b50ac1624fca927
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 3%

---

# Adobe Target を使用したパーソナライゼーション

## 概要 {#overview}

Edge Networkサーバー API は、[ フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) を利用して、Adobe Targetで作成されたパーソナライズされたエクスペリエンスを配信およびレンダリングできます。

>[!IMPORTANT]
>
>[Target Visual Experience Composer （VEC）を使用して作成されたPersonalization エクスペリエンスは ](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)Server API では完全にはサポートされません。 Server API は、VEC で作成されたアクティビティを **取得** できますが、Server API は、VEC で作成されたアクティビティを **レンダリング** できません。 VEC で作成されたアクティビティをレンダリングする場合は、Web SDK とEdge Networkサーバー API を使用して [ ハイブリッドパーソナライゼーション ](../web-sdk/personalization/hybrid-personalization.md) を実装してください。

## データストリームの設定 {#configure-your-datastream}

Server API をAdobe Targetと組み合わせて使用する前に、データストリーム設定でAdobe Target パーソナライゼーションを有効にする必要があります。

Adobe Targetを有効にする方法について詳しくは、[ データストリームへのサービスの追加に関するガイド ](../datastreams/overview.md#adobe-target-settings) を参照してください。

データストリームを設定する際に、[!DNL Property Token]、[!DNL Target Environment ID]、[!DNL Target Third Party ID Namespace] の値を（オプションで）指定できます。

![Adobe Targetが選択された状態のデータストリームサービス設定画面を示す UI 画像 ](assets/target-datastream.png)

## カスタムパラメーター {#custom-parameters}

各リクエストの [!DNL XDM] 部分にあるほとんどのフィールドは、ドット表記にシリアル化され、カスタムパラメーターまたは [!DNL mbox] パラメーターとして Target に送信されます。


### 例 {#custom-parameters-example}

以下の XDM サンプルを考えると、

```json
"xdm":{
   "marketing":{
      "campaignGroup":"winter22",
      "campaignName":"homeOwnerPromo22",
      "trackingCode":"hop22"
   }
}
```

Target でオーディエンスを作成する場合、次の値をカスタムパラメーターとして使用できます。

* `marketing.campaignGroup`
* `marketing.campaignName`
* `marketing.trackingCode`

## Target プロファイルの更新 {#profile-update}

[!DNL Server API] を使用すると、Target プロファイルを更新できます。 ターゲットプロファイルを更新するには、プロファイルデータがリクエストの `data` の部分に次の形式で渡されます。

```json
"data":  {
    "__adobe.target": {
        "profile.eyeColor": "brown",
        "profile.hairColor": "brown"
    }
}
```

## Target アクティビティのクエリ {#querying-target-activities}

### スキーマ {#schemas}

リクエストのクエリ部分によって、Target から返されるコンテンツが決まります。 `personalization` オブジェクトの下で、`schemas` によって返されるコンテンツのタイプが決定されます。

取得するオファーの種類が不明な場合は、パーソナライゼーションクエリに次の 4 つのスキーマをすべてEdge Networkに含める必要があります。

* **HTMLベースのオファー：**
https://ns.adobe.com/personalization/html-content-item
* **JSON ベースのオファー：**
https://ns.adobe.com/personalization/json-content-item
* **Target リダイレクトオファー**
https://ns.adobe.com/personalization/redirect-item
* **Target DOM 操作オファー**
https://ns.adobe.com/personalization/dom-action

### 決定範囲 {#decision-scopes}

適切なコンテンツを返すには、Adobe Target [!DNL mbox] 名を `decisionScopes` 配列に含める必要があります。

#### 例 {#decision-scopes-example}

次の例では、4 つのオファータイプすべてと、`serverapimbox` という Target アクティビティがリクエストされます。

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

完全な XDM オブジェクト、プロファイルパラメーターおよび適切な Target クエリを含む完全なリクエストの概要を以下に示します。

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

Edge Networkは、以下のような応答を返します。

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

Adobe Targetに送信されたデータに基づいて、訪問者がパーソナライゼーションアクティビティの対象となった場合、関連するアクティビティのコンテンツは `handle` オブジェクトの下に見つかり、タイプは `personalization:decisions` になります。

その他のコンテンツも `handle` の下に返される場合があります。 その他のコンテンツタイプは、Target のパーソナライゼーションには関係ありません。 訪問者が複数のアクティビティの対象となる場合、各アクティビティは、配列内の個別の `personalization` オブジェクトになります。

次の表に、応答のその部分の主な要素を説明します。

| プロパティ | 説明 | 例 |
|---|---|---|
| `scope` | 提案されたオファーが生成された Target mbox 名。 | `"scope": "serverapimbox"` |
| `items[].schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 これは、パーソナライゼーションアクティビティの作成時に選択したアクティビティタイプに関連しています。 | `"schema": "https://ns.adobe.com/personalization/json-content-item",` |
| `items[].meta.activity.id` | オファーアクティビティの一意の ID。 通常は 6 桁の数値です。 | `"activity.id": "140281"` |
| `items[].meta.activity.name` | ユーザー指定のオファーアクティビティの名前。 これは、アクティビティ作成手順の最中に行います。 | `"activity.name": "Server API Form"` |
| `items[].meta.experience.id` | パーソナライゼーションエクスペリエンスの一意の ID。 | `"experience.id": "0"` |
| `items[].meta.experience.name` | パーソナライゼーションエクスペリエンスの一意の名前。 | `"experience.name": "Experience A"` |
| `items[].data.id` | 提案されたオファーの ID。 | `"id": "282484"` |
| `items[].data.format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format: "application/json` |
| `items[].data.content` | 提案されたオファーに関連付けられたコンテンツ。 これは、呼び出し元のアプリケーションのコンテンツのパーソナライゼーションに使用されます。 | `"content": "<CONTENT CONFIGURED IN TARGET>"` |

## サーバーサイドパーソナライゼーションサンプルアプリケーション {#sample}

[ この URL](https://github.com/adobe/alloy-samples/tree/main/target/personalization-server-side) にあるサンプルアプリケーションは、Adobe Experience Platformを使用してAdobe Targetからパーソナライゼーションコンテンツを取得する方法を示しています。 Web ページは、返されるパーソナライゼーションコンテンツに基づいて変更されます。

このサンプルは _パーソナライゼーションコンテンツの取得に [!DNL Web SDK] のようなクライアントサイドライブラリに依存していません_。 代わりに、Adobe Experience Platform API を使用してパーソナライゼーションコンテンツを取得します。 次に、実装は、返されたパーソナライゼーションコンテンツに基づいて、サーバーサイドでHTMLを生成します。
