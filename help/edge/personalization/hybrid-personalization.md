---
title: Web SDK と Edge Network Server API を使用したハイブリッドパーソナライゼーション
description: この記事では、Web SDK を Server API と組み合わせて使用して、web プロパティにハイブリッドパーソナライゼーションをデプロイする方法について説明します。
keywords: パーソナライゼーション;ハイブリッド;Server API;サーバーサイド;ハイブリッド実装;
exl-id: 506991e8-701c-49b8-9d9d-265415779876
source-git-commit: a9887535b12b8c4aeb39bb5a6646da88db4f0308
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 100%

---

# Web SDK と Edge Network Server API を使用したハイブリッドパーソナライゼーション

## 概要 {#overview}

ハイブリッドパーソナライゼーションでは、[Edge Network Server API](../..//server-api/overview.md) を使用してパーソナライゼーションコンテンツをサーバーサイドで取得し、[Web SDK](../home.md) を使用してクライアント側でレンダリングするプロセスについて説明します。

ハイブリッドパーソナライゼーションを、Adobe Target や Offer Decisioning などのパーソナライゼーションソリューションと共に使用できますが、違いは、[!UICONTROL Server API] ペイロードの内容です。

## 前提条件 {#prerequisites}

Web プロパティにハイブリッドパーソナライゼーションを実装する前に、次の条件を満たしていることを確認してください。

* 使用するパーソナライゼーションソリューションを決定しました。 これは、[!UICONTROL Server API] ペイロードの内容に影響を与えます。
* アプリケーションサーバーにアクセスし、そのサーバーを使用して [!UICONTROL Server API] 呼び出しを行います。
* [Edge Network Server API](../../server-api/authentication.md) にアクセスできます。
* パーソナライズするページに Web SDK を正しく[設定しデプロイ](../fundamentals/configuring-the-sdk.md)しました。

## フロー図 {#flow-diagram}

次のフロー図は、ハイブリッドパーソナライゼーションを配信するための実行ステップの順序を示しています。

![ハイブリッドパーソナライゼーションを配信するための実行ステップの順序を示す視覚的なフロー図](assets/hybrid-personalization-diagram.png)

1. ブラウザーによって以前に保存された `kndctr_` で始まる既存の Cookie は、ブラウザーリクエストに含まれます。
1. クライアントの web ブラウザーは、アプリケーションサーバーに web ページをリクエストします。
1. アプリケーションサーバーがページリクエストを受信すると、`POST` リクエストを [Server API インタラクティブデータ収集エンドポイント](../../server-api/interactive-data-collection.md)に送信して、パーソナライゼーションコンテンツを取得します。 この `POST` リクエストには `event` と `query` が含まれています。前のステップで作成した Cookie がある場合は、`meta>state>entries` 配列に含まれています。
1. Server API は、パーソナライゼーションコンテンツをアプリケーションサーバーに返します。
1. アプリケーションサーバーは、[ID とクラスターの Cookie](#cookies) を含んだ HTML 応答をクライアントブラウザーに返します。
1. クライアントページでは、[!DNL Web SDK] `applyResponse` コマンドが呼び出され、前のステップの [!UICONTROL Server API] 応答のヘッダーと本文が渡されます。
1. `renderDecisions` フラグが `true` に設定されているので、[!DNL Web SDK] は [[!DNL Visual Experience Composer (VEC)]](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja) が提供するページの読み込みを自動的にレンダリングします。
1. フォームベースの [!DNL JSON] オファーは、`applyPersonalization` メソッドを使用して手動で適用され、パーソナライゼーション オファーに基づいて [!DNL DOM] を更新します。
1. フォームベースのアクティビティの場合、オファーがいつ表示されたかを示すために、表示イベントを手動で送信する必要があります。これは、`sendEvent` コマンドを使用して行われます。

## Cookie {#cookies}

Cookie は、ユーザー ID とクラスター情報を保持するために使用されます。ハイブリッド実装を使用する場合、web アプリケーションサーバーは、リクエストのライフサイクル中にこれらの Cookie の保存と送信を処理します。

| Cookie | 目的 | 保存者 | 送信者 |
|---|---|---|---|
| `kndctr_AdobeOrg_identity` | ユーザー ID の詳細が含まれます。 | アプリケーションサーバー | アプリケーションサーバー |
| `kndctr_AdobeOrg_cluster` | リクエストを満たすために使用する Edge Network クラスターを示します。 | アプリケーションサーバー | アプリケーションサーバー |

## リクエストの配置 {#request-placement}

提案を取得し表示通知を送信するには、Server API リクエストが必要です。 ハイブリッド実装を使用する場合、アプリケーションサーバーはこれらのリクエストを Server API に対して行います。

| リクエスト | 作成者 |
|---|---|
| 提案を取得するためのインタラクションリクエスト | アプリケーションサーバー |
| 表示通知を送信するためのインタラクションリクエスト | アプリケーションサーバー |

## Analytics への影響 {#analytics}

ハイブリッドパーソナライゼーションを実装する場合、Analytics でページヒットが複数回カウントされないように、特に注意する必要があります。

Analytics 用に[データストリームを設定](../datastreams/overview.md)すると、ページヒットが取り込まれるように、イベントが自動転送されます。

この実装のサンプルでは、次の 2 つの異なるデータストリームを使用しています。

* Analytics 用に設定されたデータストリーム。 このデータストリームは、Web SDK のインタラクションに使用されます。
* Analytics 設定のない 2 つ目のデータストリーム。 このデータストリームは、Server API リクエストに使用されます。

このように、サーバーサイドのリクエストは Analytics イベントを登録しませんが、クライアントサイドのリクエストでは登録します。 これにより、Analytics リクエストが正確にカウントされます。


## サーバーサイドリクエスト {#server-side-request}

以下のサンプルリクエストでは、アプリケーションサーバーがパーソナライゼーションコンテンツを取得するために使用できる Server API リクエストを示します。

>[!IMPORTANT]
>
>このサンプルリクエストでは、Adobe Target をパーソナライゼーションソリューションとして使用します。リクエストは、選択したパーソナライゼーションソリューションによって異なる場合があります。


**API 形式**

```http
POST /ee/v2/interact
```

### リクエスト {#request}

```shell
curl -X POST "https://edge.adobedc.net/ee/v2/interact?dataStreamId={DATASTREAM_ID}" 
-H "Content-Type: text/plain" 
-d '{
   "event":{
      "xdm":{
         "web":{
            "webPageDetails":{
               "URL":"http://localhost/"
            },
            "webReferrer":{
               "URL":""
            }
         },
         "identityMap":{
            "FPID":[
               {
                  "id":"xyz",
                  "authenticatedState":"ambiguous",
                  "primary":true
               }
            ]
         },
         "timestamp":"2022-06-23T22:21:00.878Z"
      },
      "data":{
         
      }
   },
   "query":{
      "identity":{
         "fetch":[
            "ECID"
         ]
      },
      "personalization":{
         "schemas":[
            "https://ns.adobe.com/personalization/default-content-item",
            "https://ns.adobe.com/personalization/html-content-item",
            "https://ns.adobe.com/personalization/json-content-item",
            "https://ns.adobe.com/personalization/redirect-item",
            "https://ns.adobe.com/personalization/dom-action"
         ],
         "decisionScopes":[
            "__view__",
            "sample-json-offer"
         ]
      }
   },
   "meta":{
      "state":{
         "domain":"localhost",
         "cookiesEnabled":true,
         "entries":[
            {
               "key":"kndctr_XXX_AdobeOrg_identity",
               "value":"abc123"
            },
            {
               "key":"kndctr_XXX_AdobeOrg_cluster",
               "value":"or2"
            }
         ]
      }
   }
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `dataStreamId` | `String` | はい。 | Edge Network にインタラクションを渡すために使用するデータストリームの ID。データストリームの設定方法については、[データストリームの概要](../datastreams/overview.md)を参照してください。 |
| `requestId` | `String` | いいえ | 内部サーバーリクエストを関連付けるためのランダム ID。何も指定されない場合、Edge Network によって生成され、応答で返されます。 |

### サーバーサイド応答 {#server-response}

以下のサンプル応答では、Server API 応答がどのようになるかを示します。


```json
{
   "requestId":"5c539bd0-33bf-43b6-a054-2924ac58038b",
   "handle":[
      {
         "payload":[
            {
               "id":"XXX",
               "namespace":{
                  "code":"ECID"
               }
            }
         ],
         "type":"identity:result"
      },
      {
         "payload":[
            {
               "..."
            },
            {
               "..."
            }
         ],
         "type":"personalization:decisions",
         "eventIndex":0
      }
   ]
}
```

## クライアントサイドリクエスト {#client-request}

クライアントページでは、[!DNL Web SDK] `applyResponse` コマンドを呼び出して、サーバーサイド応答のヘッダーと本文を渡します。

```js
   alloy("applyResponse", {
      "renderDecisions": true,
      "responseHeaders": {
         "cache-control": "no-cache, no-store, max-age=0, no-transform, private",
         "connection": "close",
         "content-encoding": "deflate",
         "content-type": "application/json;charset=utf-8",
         "date": "Mon, 11 Jul 2022 19:42:01 GMT",
         "server": "jag",
         "strict-transport-security": "max-age=31536000; includeSubDomains",
         "transfer-encoding": "chunked",
         "vary": "Origin",
         "x-adobe-edge": "OR2;9",
         "x-content-type-options": "nosniff",
         "x-konductor": "22.6.78-BLACKOUTSERVERDOMAINS:7fa23f82",
         "x-rate-limit-remaining": "599",
         "x-request-id": "5c539bd0-33bf-43b6-a054-2924ac58038b",
         "x-xss-protection": "1; mode=block"
      },
      "responseBody": {
         "requestId": "5c539bd0-33bf-43b6-a054-2924ac58038b",
         "handle": [
         {
            "payload": [
               {
               "id": "XXX",
               "namespace": {
                  "code": "ECID"
               }
               }
            ],
            "type": "identity:result"
         },
         {
            "payload": [
               {...}, 
               {...}
            ],
            "type": "personalization:decisions",
            "eventIndex": 0
         }
         ]
      }
   }
   ).then(applyPersonalization("sample-json-offer"));
```

フォームベースの [!DNL JSON] オファーでは、`applyPersonalization` メソッドを通じて手動で適用し、パーソナライゼーションオファーに基づいて [!DNL DOM] を更新します。フォームベースのアクティビティの場合、オファーがいつ表示されたかを示すために、表示イベントを手動で送信する必要があります。これは、`sendEvent` コマンドで実行します。

```js
function sendDisplayEvent(decision) {
    const { id, scope, scopeDetails = {} } = decision;

    alloy("sendEvent", {
        xdm: {
            eventType: "decisioning.propositionDisplay",
            _experience: {
                decisioning: {
                    propositions: [
                        {
                            id: id,
                            scope: scope,
                            scopeDetails: scopeDetails,
                        },
                    ],
                },
            },
        },
    });
}
```

## サンプルアプリケーション {#sample-app}

このタイプのパーソナライゼーションの実験や詳細の確認に役立つように、ダウンロードしてテストに使用できるサンプルアプリケーションを提供しています。この [GitHub リポジトリ](https://github.com/adobe/alloy-samples)から、アプリケーションとその使用方法に関する詳細な手順をダウンロードできます。
