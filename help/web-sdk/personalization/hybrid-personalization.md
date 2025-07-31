---
title: Web SDKとEdge Network API を使用したハイブリッドパーソナライゼーション
description: この記事では、Web SDKをEdge Network API と組み合わせて使用して、web プロパティにハイブリッドパーソナライゼーションをデプロイする方法について説明します。
keywords: パーソナライゼーション;ハイブリッド;Server API;サーバーサイド;ハイブリッド実装;
exl-id: 506991e8-701c-49b8-9d9d-265415779876
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '1200'
ht-degree: 41%

---

# Web SDKとEdge Network API を使用したハイブリッドパーソナライゼーション

## 概要 {#overview}

ハイブリッドパーソナライゼーションでは、[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) を使用してパーソナライゼーションコンテンツをサーバーサイドで取得し、[Web SDK](../home.md) を使用してクライアント側でレンダリングするプロセスについて説明します。

ハイブリッドパーソナライゼーションを、Adobe Target、Adobe Journey Optimizer、Offer Decisioningなどのパーソナライゼーションソリューションと共に使用できますが、違いは、[!UICONTROL Edge Network API] ペイロードの内容です。

## 前提条件 {#prerequisites}

Web プロパティにハイブリッドパーソナライゼーションを実装する前に、次の条件を満たしていることを確認してください。

* 使用するパーソナライゼーションソリューションを決定しました。 これは、[!UICONTROL Edge Network API] ペイロードの内容に影響を与えます。
* アプリケーションサーバーにアクセスし、そのサーバーを使用して [!UICONTROL Edge Network API] 呼び出しを行います。
* [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) にアクセスできます。
* パーソナライズするページに Web SDKを正しく [ 設定 ](/help/web-sdk/commands/configure/overview.md) およびデプロイしている。

## フロー図 {#flow-diagram}

次のフロー図は、ハイブリッドパーソナライゼーションを配信するための実行ステップの順序を示しています。

![ハイブリッドパーソナライゼーションを配信するための実行ステップの順序を示す視覚的なフロー図](assets/hybrid-personalization-diagram.png)

1. ブラウザーによって以前に保存された、接頭辞 `kndctr_` が付いている既存の Cookie は、ブラウザーリクエストに含まれます。
1. クライアントの web ブラウザーは、アプリケーションサーバーに web ページをリクエストします。
1. アプリケーションサーバーがページリクエストを受け取ると、`POST`Edge Network API インタラクティブデータ収集エンドポイントに対して [ リクエストを実行し ](https://developer.adobe.com/data-collection-apis/docs/endpoints/interact/) パーソナライゼーションコンテンツを取得します。 この `POST` リクエストには `event` と `query` が含まれています。前のステップで作成した Cookie がある場合は、`meta>state>entries` 配列に含まれています。
1. Edge Network API は、パーソナライゼーションコンテンツをアプリケーションサーバーに返します。
1. アプリケーションサーバーは、[ID とクラスターの Cookie](#cookies) を含んだ HTML 応答をクライアントブラウザーに返します。
1. クライアントページでは、[!DNL Web SDK] `applyResponse` コマンドが呼び出され、前のステップの [!UICONTROL Edge Network API] 応答のヘッダーと本文が渡されます。
1. [!DNL Web SDK] フラグが [[!DNL Visual Experience Composer (VEC)] に設定されているので、](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) は Target `renderDecisions``true` オファーおよびJourney Optimizer web チャネル項目を自動的にレンダリングします。
1. Target フォームベースの [!DNL HTML]/[!DNL JSON] オファーとJourney Optimizer コードベースのエクスペリエンスは、`applyProposition` メソッドを使用して手動で適用され、提案のパーソナライゼーションコンテンツに基づいて [!DNL DOM] を更新します。
1. Target フォームベースの [!DNL HTML]/[!DNL JSON] オファーおよびJourney Optimizer コードベースのエクスペリエンスの場合、返されたコンテンツがいつ表示されたかを示すために、表示イベントを手動で送信する必要があります。 これは、`sendEvent` コマンドを使用して行われます。

## Cookie {#cookies}

Cookie は、ユーザー ID とクラスター情報を保持するために使用されます。ハイブリッド実装を使用する場合、web アプリケーションサーバーは、リクエストのライフサイクル中にこれらの Cookie の保存と送信を処理します。

| Cookie | 目的 | 保存者 | 送信者 |
|---|---|---|---|
| `kndctr_AdobeOrg_identity` | ユーザー ID の詳細が含まれます。 | アプリケーションサーバー | アプリケーションサーバー |
| `kndctr_AdobeOrg_cluster` | リクエストを満たすために使用する Edge Network クラスターを示します。 | アプリケーションサーバー | アプリケーションサーバー |

## リクエストの配置 {#request-placement}

提案を取得し、表示通知を送信するには、Edge Network API リクエストが必要です。 ハイブリッド実装を使用する場合、アプリケーションサーバーはこれらのリクエストをEdge Network API に対して行います。

| リクエスト | 作成者 |
|---|---|
| 提案を取得するためのインタラクションリクエスト | アプリケーションサーバー |
| 表示通知を送信するためのインタラクションリクエスト | アプリケーションサーバー |


## Edge Network地域のホストの設置 {#regional-host}

Edge Network地域ホストを確立するには、まず `kndctr_<orgId>_AdobeOrg_cluster` cookie から場所のヒントを読み取ります。この cookie には、次の値を指定できます。

* `va6`
* `or2`
* `irl1`
* `ind1`
* `sgp3`
* `jpn3`
* `aus3`

例：`kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_cluster: va6`

Edge Network地域ホストは、`<location_hint>.server.adobedc.net` の形式を使用し、次の値を持つことができます。

* `va6.server.adobedc.net`
* `or2.server.adobedc.net`
* `irl1.server.adobedc.net`
* `ind1.server.adobedc.net`
* `sgp3.server.adobedc.net`
* `jpn3.server.adobedc.net`
* `aus3.server.adobedc.net`

これらの専用ホストを使用すると、リクエストはユーザーが以前に訪問したのと同じEdge Networkの場所にリダイレクトされ、ユーザーデータが存在するので、システムは最適なエクスペリエンスを提供できます。

場所のヒント（cookie など）がない場合は、デフォルトのホスト `server.adobedc.net` を使用します。

>[!TIP]
>
>ベストプラクティスとして、許可されている場所のリストを使用してください。 これにより、クライアントサイド cookie を介して提供されるので、場所のヒントが改ざんされるのを防ぎます。

## Analytics への影響 {#analytics}

ハイブリッドパーソナライゼーションを実装する場合、Analytics でページヒットが複数回カウントされないように、特に注意する必要があります。

Analytics 用に[データストリームを設定](../../datastreams/overview.md)すると、ページヒットが取り込まれるように、イベントが自動転送されます。

この実装のサンプルでは、次の 2 つの異なるデータストリームを使用しています。

* Analytics 用に設定されたデータストリーム。 このデータストリームは、Web SDK のインタラクションに使用されます。
* Analytics 設定のない 2 つ目のデータストリーム。 このデータストリームは、Edge Network API リクエストに使用されます。 このデータストリームは、Analytics 用に設定したデータストリームと同じ宛先設定で設定する必要があります。

このように、サーバーサイドのリクエストは Analytics イベントを登録しませんが、クライアントサイドのリクエストでは登録します。 これにより、Analytics リクエストが正確にカウントされます。

## サーバーサイドリクエストの作成 {#server-side-request}

以下のサンプルリクエストでは、アプリケーションサーバーがパーソナライゼーションコンテンツを取得するために使用できるEdge Network API リクエストを示しています。

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
         "entries": [{
           "key": "kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_identity",
           "value":"CiY0NzE0NzkwMTUyMzYzMzI4NDAxMjc3NDcwNzA2NTcxMjI3OTI1NVIRCJ_S-uCRMRABGAEqBElSTDHwAZ_S-uCRMQ=="
         }, {
           "key": "kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_consent",
           "value": "general=in"
         }, {
            "key": "kndctr_53A16ACB5CC1D3760A495C99_AdobeOrg_cluster",
            "value": "va6"
         }]
      }
   }
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `dataStreamId` | `String` | はい。 | Edge Network にインタラクションを渡すために使用するデータストリームの ID。データストリームの設定方法については、[データストリームの概要](../../datastreams/overview.md)を参照してください。 |
| `requestId` | `String` | いいえ | 内部サーバーリクエストを関連付けるためのランダム ID。何も指定されない場合、Edge Network によって生成され、応答で返されます。 |

### プロキシヘッダー {#proxy-headers}

リクエストを正しく処理するには、次のヘッダーが必要です。

* `Referer`
* `X-Forwarded-For`
* `X-Forwarded-Proto`
* `X-Forwarded-Host`

実際のクライアント情報を指すように、これらを正しく設定してください。 例えば、`X-Forwarded-For` ヘッダーには、適切な位置情報を取得するためのクライアント IP を含める必要があります。

### User-Agent ヘッダー {#user-agent-headers}

次の user-agent ヘッダーを使用して、リクエストを正しく処理します。

**デフォルト**

* `User-Agent`

**低エントロピー（必須）:**

* `Sec-CH-UA`
* `Sec-CH-UA-Mobile`
* `Sec-CH-UA-Platform`

**高エントロピー（オプション）:**

* `Sec-CH-UA-Platform-Version`
* `Sec-CH-UA-Arch`
* `Sec-CH-UA-Model`
* `Sec-CH-UA-Bitness`
* `Sec-CH-UA-WoW64`

リクエストは、[Edge Network API 仕様 ](https://developer.adobe.com/data-collection-apis/docs/endpoints/interact/) に示されているように送信する必要があります。 ユースケースで必要な場合は、[ パーソナライゼーションドキュメント ](https://developer.adobe.com/data-collection-apis/docs/getting-started/personalization/) を参照してください。

### サーバーサイド応答 {#server-response}

Edge Network応答には `state:store` の命令が含まれます。この命令は、`Set-Cookie` ヘッダーに変換する必要があります。 これらはブラウザーに保存され、web SDK実装で使用できます。

Cookie は、クライアントインスタンスだけでなく、サーバー実装にもリクエストと共に送信されるように、トップレベルドメインに設定する必要があります。 （または、少なくとも両方の実装で使用される共通のサブドメイン）

例：

* サーバーサイド呼び出しでは `api.example.com` を使用します
* クライアントサイド呼び出しでは `adobe.example.com` を使用

どちらの場合でも cookie が共有されるように、`.example.com` で cookie を設定する必要があります。

サーバーサイドの応答は、データストリーム設定に応じて生成される、`Handles` と呼ばれるフラグメントで構成されます。 例えば、リアルタイムパーソナライゼーションエンジンは `personalization:decisions` ハンドルを返し、リアルタイムアクティベーションエンジンは `activation:pull` ハンドルを生成します。

以下のサンプル応答は、Edge Network API 応答を示しています。

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
        "xdm": {
            "eventType": "decisioning.propositionDisplay",
            "_experience": {
                "decisioning": {
                    "propositions": [{
                        "id": id,
                        "scope": scope,
                        "scopeDetails": scopeDetails
                    }],
                    "propositionEventType": {
                        "display": 1
                    }
                }
            }
        }
    });
}
```

## サンプルアプリケーション {#sample-app}

このタイプのパーソナライゼーションの実験や詳細の確認に役立つように、ダウンロードしてテストに使用できるサンプルアプリケーションを提供しています。この [GitHub リポジトリ](https://github.com/adobe/alloy-samples)から、アプリケーションとその使用方法に関する詳細な手順をダウンロードできます。
