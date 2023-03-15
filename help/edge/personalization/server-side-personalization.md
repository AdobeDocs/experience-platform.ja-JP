---
title: Edge Network Server API を使用したサーバーサイドパーソナライゼーション
description: この記事では、Edge Network Server API を使用して、web プロパティにサーバーサイドパーソナライゼーションをデプロイする方法について説明します。
keywords: パーソナライゼーション; server api; edge network; サーバーサイド;
source-git-commit: 3e7084953a5e158059074c857bfce4940a83661b
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 100%

---


# Edge Network Server API を使用したサーバーサイドパーソナライゼーション

## 概要 {#overview}

サーバーサイドパーソナライゼーションでは、[Edge Network Server API](../../server-api/overview.md) を使用して、web プロパティでの顧客体験をパーソナライズします。

この記事で説明する例では、Server API を使用して、サーバーサイドでパーソナライゼーションコンテンツを取得します。次に、取得したパーソナライゼーションコンテンツに基づいて、HTML がサーバーサイドでレンダリングされます。

次の表に、パーソナライズされたコンテンツと、パーソナライズされていないコンテンツの例を示します。

| パーソナライゼーションを使用していないサンプルページ | パーソナライゼーションを使用したサンプルページ |
|---|---|
| ![パーソナライゼーションを使用していない web ページの例](assets/plain.png) | ![パーソナライゼーションを使用した web ページの例](assets/personalized.png) |

## 注意点 {#considerations}

### Cookie {#cookies}

Cookie は、ユーザー ID とクラスター情報を保持するために使用されます。サーバーサイド実装を使用する場合、アプリケーションサーバーは、リクエストのライフサイクル中にこれらの Cookie の保存と送信を処理します。

| Cookie | 目的 | 保存者 | 送信者 |
|---|---|---|---|
| `kndctr_AdobeOrg_identity` | ユーザー ID の詳細が含まれます。 | アプリケーションサーバー | アプリケーションサーバー |
| `kndctr_AdobeOrg_cluster` | リクエストを満たすために使用する Edge Network クラスターを示します。 | アプリケーションサーバー | アプリケーションサーバー |

### リクエストの配置 {#request-placement}

提案を取得し、表示通知を送信するには、パーソナライゼーションリクエストが必要です。サーバーサイド実装を使用する場合、アプリケーションサーバーは、Edge Network Server API に対してこれらのリクエストを行います。

| リクエスト | 作成者 |
|---|---|
| 提案を取得するためのインタラクションリクエスト | Edge Network Server API を呼び出すアプリケーションサーバー。 |
| 表示通知を送信するためのインタラクションリクエスト | Edge Network Server API を呼び出すアプリケーションサーバー。 |

## サンプルアプリケーション {#sample-app}

以下に説明するプロセスでは、このタイプのパーソナライゼーションについて実験し、詳しく学ぶための出発点として使用できる、サンプルアプリケーションを使用します。

このサンプルをダウンロードすると、独自のニーズに合わせてカスタマイズできます。例えば、環境変数を変更して、サンプルアプリが独自の Experience Platform 設定からオファーを取り込めるようにすることができます。

これを行うには、リポジトリのルートにある `.env` ファイルを開き、設定に従って変数を変更します。サンプルアプリを再起動すると、独自のパーソナライゼーションコンテンツを使用して実験する準備が整います。

### サンプルの実行 {#running-sample}

以下の手順に従って、サンプルアプリを実行します。

1. ローカルマシンに[このリポジトリ](https://github.com/adobe/alloy-samples)のクローンを作成します。
2. ターミナルを開き、`personalization-server-side` フォルダーに移動します。
3. `npm install` を実行します。
4. `npm start` を実行します。
5. Web ブラウザーを開き、`http://localhost` に移動します。

## プロセスの概要 {#process}

この節では、パーソナライゼーションコンテンツを取得する際に使用する手順について説明します。

1. [Express](https://expressjs.com/) は、リーン方式のサーバーサイド実装に使用されます。これは、基本的なサーバーリクエストとルーティングを処理します。
2. ブラウザーは web ページをリクエストします。ブラウザーによって以前に保存された、先頭に `kndctr_` が付いているすべての Cookie が含まれます。
3. アプリサーバーからページがリクエストされると、[インタラクティブデータ収集エンドポイント](../../../server-api/interactive-data-collection.md)にイベントが送信され、パーソナライゼーションコンテンツが取得されます。サンプルアプリでは、ヘルパーメソッドを使用して、API へのリクエストの作成と送信を簡素化します（[aepEdgeClient.js](https://github.com/adobe/alloy-samples/blob/main/common/aepEdgeClient.js) を参照）。この `POST` リクエストには `event` と `query` が含まれています。前のステップで作成した Cookie がある場合は、`meta>state>entries` 配列に含まれています。

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

4. フォームベースのアクティビティからの Target オファーは、応答から読み取られ、HTML 応答の生成時に使用されます。
5. フォームベースのアクティビティの場合、オファーがいつ表示されたかを示すために、実装時に表示イベントを手動で送信する必要があります。この例では、通知はリクエストのライフサイクル中にサーバーサイドで送信されます。

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
7. HTML 応答が返されると、ID とクラスターの Cookie がアプリケーションサーバーによって応答に設定されます。