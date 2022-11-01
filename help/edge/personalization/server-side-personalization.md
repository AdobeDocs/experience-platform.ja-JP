---
title: Edge Network Server API を使用したサーバー側のパーソナライゼーション
description: この記事では、Edge Network Server API を使用して、Web プロパティにサーバー側のパーソナライゼーションをデプロイする方法について説明します。
keywords: パーソナライゼーション；server api;エッジネットワーク；サーバー側；
source-git-commit: 3e7084953a5e158059074c857bfce4940a83661b
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 2%

---


# Edge Network Server API を使用したサーバー側のパーソナライゼーション

## 概要 {#overview}

サーバー側のパーソナライゼーションでは、 [Edge Network Server API](../../server-api/overview.md) を使用して、web プロパティ上の顧客体験をパーソナライズします。

この記事で説明する例では、パーソナライゼーションコンテンツは、Server API を使用してサーバー側で取得されます。 次に、取得したHTML化コンテンツに基づいて、パーソナライゼーションがサーバーサイドでレンダリングされます。

次の表に、パーソナライズされたコンテンツと、パーソナライズされていないコンテンツの例を示します。

| パーソナライゼーションを使用しないサンプルページ | パーソナライゼーションを含むサンプルページ |
|---|---|
| ![パーソナライゼーションを使用しない Web ページの例](assets/plain.png) | ![パーソナライゼーションを含む Web ページの例](assets/personalized.png) |

## 注意点 {#considerations}

### Cookie {#cookies}

Cookie は、ユーザー ID とクラスター情報を保持するために使用されます。  サーバー側実装を使用する場合、アプリケーションサーバーは、リクエストのライフサイクル中にこれらの cookie の保存と送信を処理します。

| cookie | 目的 | 保存者 | 送信者 |
|---|---|---|---|
| `kndctr_AdobeOrg_identity` | ユーザー ID の詳細が含まれます。 | アプリケーションサーバー | アプリケーションサーバー |
| `kndctr_AdobeOrg_cluster` | 要求を満たすために使用する Edge ネットワーククラスターを示します。 | アプリケーションサーバー | アプリケーションサーバー |

### リクエストの配置 {#request-placement}

提案を取得し、表示通知を送信するには、パーソナライゼーションリクエストが必要です。 サーバー側実装を使用する場合、アプリケーションサーバーは、Edge Network Server API に対してこれらのリクエストをおこないます。

| リクエスト | 作成者 |
|---|---|
| 提案を取得するためのインタラクションリクエスト | Edge Network Server API を呼び出すアプリケーションサーバー。 |
| 表示通知を送信するインタラクションリクエスト | Edge Network Server API を呼び出すアプリケーションサーバー。 |

## サンプルアプリケーション {#sample-app}

以下で説明するプロセスでは、このタイプのパーソナライゼーションの詳細を試して確認できる、サンプルアプリケーションを基にしています。

このサンプルをダウンロードして、独自のニーズに合わせてカスタマイズできます。 例えば、環境変数を変更して、サンプルアプリが独自の環境設定からオファーを取り込めるようにすることができます。

これをおこなうには、 `.env` ファイルをリポジトリのルートに配置し、設定に従って変数を変更します。 サンプルアプリを再起動すると、独自のパーソナライゼーションコンテンツを使用してテストする準備が整います。

### サンプルの実行 {#running-sample}

以下の手順に従って、サンプルアプリケーションを実行します。

1. 複製 [このリポジトリ](https://github.com/adobe/alloy-samples) ローカルマシンに送信します。
2. ターミナルを開き、 `personalization-server-side` フォルダー。
3. 実行 `npm install`.
4. 実行 `npm start`.
5. Web ブラウザーを開き、に移動します。 `http://localhost`.

## プロセスの概要 {#process}

この節では、パーソナライゼーションコンテンツを取得する際に使用する手順について説明します。

1. [速達](https://expressjs.com/) は、低レベルのサーバー側実装に使用されます。 これは、基本的なサーバー要求とルーティングを処理します。
2. ブラウザーが Web ページをリクエストします。 ブラウザーによって以前に保存された Cookie。先頭に「 」が付いている `kndctr_`、が含まれます。
3. アプリサーバーからページがリクエストされると、 [インタラクティブデータ収集エンドポイント](../../../server-api/interactive-data-collection.md) パーソナライゼーションコンテンツを取得します。 サンプルアプリでは、ヘルパーメソッドを使用して、リクエストの作成と API への送信を簡単におこなえます ( [aepEdgeClient.js](https://github.com/adobe/alloy-samples/blob/main/common/aepEdgeClient.js)) をクリックします。 この `POST` リクエストに `event` および `query`. 前の手順で作成した Cookie がある場合は、 `meta>state>entries` 配列。

   ```js
   fetch(
   "https://edge.adobedc.net/ee/v2/interact?dataStreamId=abc&requestId=123",
   {
      headers: {
         accept: "*/*",
         "accept-language": "en-US,en;q=0.9",
         "cache-control": "no-cache",
         "content-type": "text/plain; charset=UTF-8",
         pragma: "no-cache",
         "sec-fetch-dest": "empty",
         "sec-fetch-mode": "cors",
         "sec-fetch-site": "cross-site",
         "sec-gpc": "1",
         "Referrer-Policy": "strict-origin-when-cross-origin",
         Referer: "http://localhost/",
      },
      body: JSON.stringify({
         event: {
         xdm: {
            web: {
               webPageDetails: {
               URL: "http://localhost/",
               },
               webReferrer: {
               URL: "",
               },
            },
            identityMap: {
               FPID: [
               {
                  id: "xyz",
                  authenticatedState: "ambiguous",
                  primary: true,
               },
               ],
            },
            timestamp: "2022-06-23T22:21:00.878Z",
         },
         data: {},
         },
         query: {
         identity: {
            fetch: ["ECID"],
         },
         personalization: {
            schemas: [
               "https://ns.adobe.com/personalization/default-content-item",
               "https://ns.adobe.com/personalization/html-content-item",
               "https://ns.adobe.com/personalization/json-content-item",
               "https://ns.adobe.com/personalization/redirect-item",
               "https://ns.adobe.com/personalization/dom-action",
            ],
            decisionScopes: ["__view__", "sample-json-offer"],
         },
         },
         meta: {
         state: {
            domain: "localhost",
            cookiesEnabled: true,
            entries: [
               {
               "key": "kndctr_XXX_AdobeOrg_identity",
               "value": "abc123"
               },
               {
               "key": "kndctr_XXX_AdobeOrg_cluster",
               "value": "or2"
               }
            ],
         },
         },
      }),
      method: "POST",
   }
   ).then((res) => res.json());
   ```

4. フォームベースのアクティビティの Target オファーは、応答から読み取られ、応答の作成時にHTMLされます。
5. フォームベースのアクティビティの場合、表示イベントは、オファーがいつ表示されたかを示すために、実装時に手動で送信する必要があります。 この例では、リクエストのライフサイクル中に、通知がサーバー側で送信されます。

   ```js
   function sendDisplayEvent(aepEdgeClient, req, propositions, cookieEntries) {
   const address = getAddress(req);
   
   aepEdgeClient.interact(
      {
         event: {
         xdm: {
            web: {
               webPageDetails: { URL: address },
               webReferrer: { URL: "" },
            },
            timestamp: new Date().toISOString(),
            eventType: "decisioning.propositionDisplay",
            _experience: {
               decisioning: {
               propositions: propositions.map((proposition) => {
                  const { id, scope, scopeDetails } = proposition;
   
                  return {
                     id,
                     scope,
                     scopeDetails,
                  };
               }),
               },
            },
         },
         },
         query: { identity: { fetch: ["ECID"] } },
         meta: {
         state: {
            domain: "",
            cookiesEnabled: true,
            entries: [...cookieEntries],
         },
         },
      },
      {
         Referer: address,
      }
   );
   }
   ```

6. [!DNL Visual Experience Composer (VEC)] オファーは Web SDK 経由でのみレンダリングできるので、無視されます。
7. HTMLの応答が返されると、ID とクラスターの Cookie がアプリケーションサーバーによって応答に設定されます。