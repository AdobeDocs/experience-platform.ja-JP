---
title: Web SDK と Edge Network Server API を使用したハイブリッドパーソナライゼーション
description: この記事では、Web SDK を Server API と組み合わせて使用し、Web プロパティにハイブリッドパーソナライゼーションをデプロイする方法について説明します。
keywords: パーソナライゼーション；ハイブリッド；server api;サーバー側；ハイブリッド実装；
source-git-commit: f280d4cbcde434ccf36df37e95f1902cfd02c96c
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 3%

---


# Web SDK と Edge Network Server API を使用したハイブリッドパーソナライゼーション

## 概要 {#overview}

ハイブリッドパーソナライゼーションでは、 [Edge Network Server API](../..//server-api/overview.md)を参照し、を使用してクライアント側でレンダリングします。 [Web SDK](../home.md).

ハイブリッドパーソナライゼーションを、Adobe TargetやOffer decisioningなどのパーソナライゼーションソリューションと共に使用できます。違いは、 [!UICONTROL サーバー API] ペイロード。

## 前提条件 {#prerequisites}

Web プロパティにハイブリッドパーソナライゼーションを実装する前に、次の条件を満たしていることを確認してください。

* 使用するパーソナライゼーションソリューションを決定しました。 これは、 [!UICONTROL サーバー API] ペイロード。
* アプリケーションサーバーにアクセスでき、そのサーバーを使用して [!UICONTROL サーバー API] 呼び出し。
* 次にアクセスできます： [Edge Network Server API](../../server-api/authentication.md).
* 正しい [構成および導入済み](../fundamentals/configuring-the-sdk.md) パーソナライズするページ上の Web SDK。

## フロー図 {#flow-diagram}

次のフロー図は、ハイブリッドパーソナライゼーションを配信するために実行される手順の順序を示しています。

![ハイブリッドパーソナライゼーションを配信するために実行される手順の順序を示す視覚的なフロー図です。](assets/hybrid-personalization-diagram.png)

1. ブラウザーによって以前に保存された既存の Cookie。先頭に「 」が付いている `kndctr_`は、ブラウザーリクエストに含まれます。
1. クライアントの Web ブラウザーは、アプリケーションサーバーに Web ページをリクエストします。
1. アプリケーションサーバーは、ページリクエストを受け取ると、 `POST` にリクエスト [Server API インタラクティブデータ収集エンドポイント](../../server-api/interactive-data-collection.md) パーソナライゼーションコンテンツを取得します。 この `POST` リクエストに `event` および `query`. 前の手順で作成した Cookie がある場合は、 `meta>state>entries` 配列。
1. Server API は、パーソナライゼーションコンテンツをアプリケーションサーバーに返します。
1. アプリケーションサーバーが、HTML応答を含むクライアントブラウザーに返します。 [id とクラスターの cookie](#cookies).
1. クライアントページでは、 [!DNL Web SDK] `applyResponse` コマンドを呼び出し、 [!UICONTROL サーバー API] 前の手順の応答。
1. この [!DNL Web SDK] ページ読み込みをレンダリング [[!DNL Visual Experience Composer (VEC)]](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=en) は、自動的にオファーするので、 `renderDecisions` フラグが `true`.
1. フォームベース [!DNL JSON] オファーは、 `applyPersonalization` メソッドを使用して、 [!DNL DOM] パーソナライゼーションオファーに基づいて
1. フォームベースのアクティビティの場合、表示イベントを手動で送信して、オファーがいつ表示されたかを示す必要があります。 これは、 `sendEvent` コマンドを使用します。

## Cookie {#cookies}

Cookie は、ユーザー ID とクラスター情報を保持するために使用されます。  ハイブリッド実装を使用する場合、Web アプリケーションサーバーは、リクエストのライフサイクル中にこれらの Cookie の保存と送信を処理します。

| cookie | 目的 | 保存者 | 送信者 |
|---|---|---|---|
| `kndctr_AdobeOrg_identity` | ユーザー ID の詳細が含まれます。 | アプリケーションサーバー | アプリケーションサーバー |
| `kndctr_AdobeOrg_cluster` | 要求を満たすために使用する Edge ネットワーククラスターを示します。 | アプリケーションサーバー | アプリケーションサーバー |

## リクエストの配置 {#request-placement}

提案を取得し、表示通知を送信するには、サーバー API リクエストが必要です。 ハイブリッド実装を使用する場合、アプリケーションサーバーは Server API に対してこれらのリクエストをおこないます。

| リクエスト | 作成者 |
|---|---|
| 提案を取得するためのインタラクションリクエスト | アプリケーションサーバー |
| 表示通知を送信するインタラクションリクエスト | アプリケーションサーバー |

## Analytics への影響 {#analytics}

ハイブリッドパーソナライゼーションを実装する場合、Analytics でページのヒットが複数回カウントされないように、特に注意する必要があります。

次の場合： [データストリームの設定](../datastreams/overview.md) Analytics では、ページのヒットが取り込まれるように、イベントが自動的に転送されます。

この実装のサンプルでは、2 つの異なるデータストリームを使用します。

* Analytics 用に設定されたデータストリーム。 このデータストリームは、Web SDK のインタラクションで使用されます。
* Analytics 設定のない 2 つ目のデータストリーム。 このデータストリームは、Server API リクエストに使用されます。

この方法では、サーバー側リクエストは Analytics イベントを登録しませんが、クライアント側リクエストは Analytics イベントを登録します。 これにより、Analytics リクエストが正確にカウントされます。


## サーバー側リクエスト {#server-side-request}

以下のリクエスト例は、アプリケーションサーバーがパーソナライゼーションコンテンツを取得する際に使用できる Server API リクエストを示しています。

>[!IMPORTANT]
>
>このサンプルリクエストでは、Adobe Targetをパーソナライゼーションソリューションとして使用します。 リクエストは、選択したパーソナライゼーションソリューションによって異なる場合があります。


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
| `dataStreamId` | `String` | はい。 | Edge ネットワークにインタラクションを渡す際に使用するデータストリームの ID。 詳しくは、 [データストリームの概要](../datastreams/overview.md) を参照してください。 |
| `requestId` | `String` | × | 内部サーバーリクエストを関連付けるためのランダム ID。 何も指定されない場合、Edge ネットワークはそれらを生成し、応答で返します。 |

### サーバー側の応答 {#server-response}

以下のレスポンスのサンプルは、Server API の応答がどのようになるかを示しています。


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

## クライアント側のリクエスト {#client-request}

クライアントページでは、 [!DNL Web SDK] `applyResponse` コマンドを呼び出して、サーバー側応答のヘッダーと本文を渡します。

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

フォームベース [!DNL JSON] オファーは、 `applyPersonalization` メソッドを使用して、 [!DNL DOM] パーソナライゼーションオファーに基づいて フォームベースのアクティビティの場合、表示イベントを手動で送信して、オファーがいつ表示されたかを示す必要があります。 これは、 `sendEvent` コマンドを使用します。

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

このタイプのパーソナライゼーションの実験や詳細の確認に役立つように、ダウンロードしてテストに使用できるサンプルアプリケーションを提供しています。 ここから、アプリケーションの使用方法に関する詳細な手順と共に、アプリケーションをダウンロードできます。 [GitHub リポジトリ](https://github.com/adobe/alloy-samples).






