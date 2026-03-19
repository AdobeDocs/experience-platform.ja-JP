---
title: ChatGPT アプリ（MCP データ収集）で分析を収集してパーソナライゼーションを適用する
description: ハイブリッド MCP サーバーと web SDKの applyResponse パターンを使用して、イベントをAdobe Experience Platform Edge Networkに送信し、ChatGPT アプリ UI 内でパーソナライゼーションをレンダリングします。
keywords: Adobe Experience Platform, Web SDK, Edge Network, MCP, ChatGPT アプリ，applyResponse, インタラクションエンドポイント，パーソナライゼーション，分析
source-git-commit: c848f821ea911c82531c6784a17df0116572cd86
workflow-type: tm+mt
source-wordcount: '1126'
ht-degree: 0%

---

# ChatGPT アプリ（MCP データ収集）で分析を収集してパーソナライゼーションを適用する

このユースケースは、ChatGPT アプリ（モデルコンテキストプロトコルサーバー+ オプションの UI コンポーネント）をAdobe Experience Platform Edge Networkに接続する方法を示しています。 このタイプのデータ収集を使用すると、ツールを呼び出す対話型インタラクションの分析を記録し、ChatGPT でレンダリングされるウィジェットにEdge Networkからパーソナライゼーションの意思決定を配信できます。

>[!NOTE]
>
>このドキュメントは、Adobe データ収集チームの両方から入手可能な最新の更新と、OpenAI からの最新のテクノロジー更新に基づいて管理されています。 そのため、Adobeでは、このドキュメントが時間の経過と共に進化することを期待しており、更新を確認することをお勧めします。

このユースケースでは、データを収集するためのサーバーサイド実装と、パーソナライズされたコンテンツをレンダリングするためのクライアントサイド実装の両方を使用して、ハイブリッドアプローチを好みます。 MCP ツールの呼び出しは分析を収集する最も信頼できる瞬間なので、このアプローチは理想的です。 ウィジェットはブラウザーコンテキストで実行され、（cookie 内の） ID を保存し、パーソナライゼーションの決定を適用する適切な場所です。

このユースケースには、完全に動作するコードの例が付随しています。 サンプルコードと実装手順については、GitHub の [&#x200B; リポジトリにある &#x200B;](https://github.com/adobe/alloy-samples/tree/main/chatgpt-app)ChatGPT アプリ + Adobe Experience Platform Edge`alloy-samples` を参照してください。

>[!IMPORTANT]
>
>このページでは、統合パターンを示すためのリファレンス実装の概要を説明します。 アプリケーションでアプローチを採用する前に、セキュリティ、プライバシー、同意、実稼働要件を確認します。

## アーキテクチャ

大まかに言えば、5 つの可動部分があります。

1. **MCP ホスト（ChatGPT）**:ChatGPT は、MCP サーバーによって公開されたツールを呼び出し、リクエストメタデータに安定した偽名ユーザー識別子を提供します。
1. **MCP サーバー（バックエンド）**：組織が所有します。 項目の一覧表示、詳細の取得、リクエストの送信などのツールを実装します。
1. **サーバー**: MCP Adobe IMSがAdobe Data Collection API を呼び出すために使用するアクセス トークンを発行します。
1. **Adobe Experience Platform Edge Network**: MCP サーバーから送信されたエクスペリエンスイベントを受信し、分析の確認、状態の更新（ID など）、パーソナライゼーションの決定を返します。
1. **埋め込み web UI （MCP ホストによってレンダリングされるフロントエンドウィジェット）**：構造化された結果を表示し、MCP サーバーバックエンドから受信したAdobe メタデータを適用します。

## データフロー

1. **ユーザー** は、MCP サーバーを使用して **ChatGPT** にプロンプトを表示します。
1. **ChatGPT** はプロンプトのインテントを解釈し、適切な **バックエンド MCP ツール** を呼び出します。
1. **バックエンド MCP サーバー** は、Data Collection API （`interact` エンドポイント）を使用してエクスペリエンスイベントを **Edge Network** に送信し、Analytics の収集とオプションのパーソナライゼーションを行います。
1. **Edge Network** は、状態の更新やパーソナライズ機能の決定などの応答ハンドルを **バックエンド MCP ツール** に返します。
1. **バックエンド MCP ツール** は、ビジネスデータを `structuredContent` に、Adobe メタデータを `_meta`ChatGPT **に** して含むツール結果を返します。
1. **ChatGPT** はツールの結果を **frontend widget** に配信します。このウィジェットはビジネスデータをレンダリングし、web SDK JavaScript ライブラリの `applyResponse` コマンドを使用してAdobe メタデータを適用します。 このコマンドは、クライアントサイドの状態をハイドレートし、UI で適格なパーソナライゼーション決定をレンダリングします。

以下の各節では、各手順を詳しく説明します。

## ステップ 1:MCP サーバーを使用して ChatGPT にプロンプトを表示する

このステップは、ワークフローのエントリポイントです。 ユーザーは自然言語の意図を次のように提供します。

```text
"Use the Adobe Office Information Tool to show me details about which office that is the most pet-friendly."
```

詳しくは、OpenAI Developers のドキュメントの [MCP サーバーの構築 &#x200B;](https://developers.openai.com/apps-sdk/build/mcp-server/) を参照してください。

## 手順 2:ChatGPT がインテントを解釈し、MCP ツールを呼び出す

MCP サーバーのメタデータに基づいて、ChatGPT はインテントを解釈し、MCP サーバー上の適切なツールハンドラーを呼び出します。 このツール呼び出しは、UI レンダリングの成功とは独立した、インタラクションのサーバーサイドの信頼できるポイントを作成します。 ツールの 1 つに次のメタデータが含まれている場合があります。

```json
{
  "name": "office_details",
  "description": "Fetch details for a single office by ID and return personalization handles for the UI.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "sessionId": { "type": "string", "description": "Server-issued session identifier." },
      "officeId": { "type": "string", "description": "Office identifier." }
    },
    "required": ["sessionId", "officeId"],
    "additionalProperties": false
  },
  "_meta": {
    "ui": {
      "visibility": ["model", "app"]
    }
  }
}
```

各 MCP ツールの機能を ChatGPT に伝える方法について詳しくは、OpenAI Developers のドキュメントの [&#x200B; ツールの定義 &#x200B;](https://developers.openai.com/apps-sdk/plan/tools/) を参照してください。

## 手順 3:MCP サーバーがエクスペリエンスイベントをEdge Networkに送信する

MCP サーバーはリクエストを受け取ると、Adobe Experience Platform Edge Networkへの呼び出しをトリガーして分析データを記録し、オプションで意思決定/パーソナライゼーションをリクエストします。 このリクエストはサーバー間なので、[`interact` データ収集 API](https://developer.adobe.com/data-collection-apis/docs/endpoints/interact/) の一部として認証済みの [&#128279;](https://developer.adobe.com/data-collection-apis/docs/) エンドポイントを使用します。 Adobeでは、[&#x200B; カスタム名前空間 &#x200B;](https://experienceleague.adobe.com/ja/docs/platform-learn/implement-web-sdk/initial-configuration/configure-identities) を使用して、OpenAI の一意の ID を渡すことをお勧めします。 ID UI で作成した名前空間と、呼び出しで定義した ID 名前空間が一致すること（大文字と小文字を区別）を確認します。

```sh
curl -X POST "https://server.adobedc.net/ee/v2/interact?datastreamId={DATASTREAM_ID}"
  -H "Authorization: Bearer {TOKEN}"
  -H "x-gw-ims-org-id: {ORG_ID}"
  -H "x-api-key: {API_KEY}"
  -H "Content-Type: application/json"
  -d '{
    "event": {
      "xdm": {
        "eventType": "office.details.view",
        "identityMap": {
          "{IDENTITY_NAMESPACE}": [
            { "id": "{PSEUDONYMOUS_SUBJECT_ID}", "primary": true }
          ]
        },
        "timestamp": "YYYY-02-20T19:00:00.000Z"
      }
    },
    "query": {
      "personalization": {
        "decisionScopes": ["__view__"]
      }
    },
    "meta": {
      "state": {
        "entries": [
          { "key": "kndctr_orgid_cluster", "value": "{CLUSTER_HINT_IF_KNOWN}" },
          { "key": "kndctr_orgid_identity", "value": "{ECID_BLOB_IF_KNOWN}" }
        ]
      }
    }
  }'
```

## 手順 4:Edge Networkからハンドルが返される

Edge Networkは、`interact` 呼び出しを受け取ると、`handle` 配列で応答します。 この配列には、データストリーム設定に応じて、ID とパーソナライゼーションの決定を含めることができます。 応答の例は次のようになります。

```json
{
  "requestId": "60a2f...2294d",
  "handle": [
    {
      "type": "locationHint:result",
      "payload": [
        { "scope": "EdgeNetwork", "hint": "or2", "ttlSeconds": 1800 }
      ]
    },
    {
      "type": "state:store",
      "payload": [
        { "key": "kndctr_..._identity", "value": "CiYzM...snTI=", "maxAge": 34128000 },
        { "key": "kndctr_..._cluster", "value": "or2", "maxAge": 1800 }
      ]
    }
  ]
}
```

その後、MCP サーバーがEdge Network応答から情報を抽出し、ID 情報を保持できます。

```ts
type EdgeHandle = { type: string; payload?: Array<{ key?: string; value?: string }> };

export function extractStateStore(handles: EdgeHandle[]) {
  const store = handles.find(h => h.type === "state:store");
  const entries = store?.payload ?? [];

  const identity = entries.find(e => e.key?.includes("_identity"))?.value;
  const cluster  = entries.find(e => e.key?.includes("_cluster"))?.value;

  return { identity, cluster };
}
```

## 手順 5:MCP サーバーが構造化ツールの出力とAdobe メタデータを ChatGPT に返す

MCP ツールの応答には、構造化されたツールの出力とEdge Networkからのパーソナライゼーションの両方が含まれます。

* `structuredContent` オブジェクトには、ChatGPT が安全に読み取って読み上げることができるビジネスデータが含まれています。
* `_meta` オブジェクトには、Adobeの応答ハンドルとサーバーが計算した `identityMap` が含まれているので、ChatGPT に公開することなく読み取ることができます。 この情報を `_meta.adobe` に保持することで、そのデータが配置されている場所に関する一貫性を保つことができます。 同じ `identityMap` を渡すと、後の UI 側イベントでウィジェットが同じカスタム ID を使用するのに役立ちます。

```json
{
  "content": "Displayed details for office seattle.",
  "structuredContent": {
    "office": {
      "id": "seattle",
      "name": "Seattle",
      "amenities": ["Pet Friendly", "Cafe", "Bike Storage"]
    }
  },
  "_meta": {
    "adobe": {
      "identityMap": {
        "{IDENTITY_NAMESPACE}": [
          { "id": "{PSEUDONYMOUS_SUBJECT_ID}", "primary": true }
        ]
      },
      "handles": [
        {
          "type": "state:store",
          "payload": [
            { "key": "kndctr_..._identity", "value": "..." }
          ]
        },
        {
          "type": "personalization:decisions",
          "payload": [
            { "id": "..." }
          ]
        }
      ]
    }
  }
}
```

詳しくは、『 OpenAI デベロッパーリファレンス』の [&#x200B; ツールの結果 &#x200B;](https://developers.openai.com/apps-sdk/reference/#tool-results) を参照してください。

## 手順 6：ウィジェットが結果をレンダリングし、`_adobe.handles` を使用して `applyResponse` 適用する

ウィジェットは、`structuredContent` からビジネスデータをレンダリングした後、`_meta.adobe` からAdobe メタデータを読み取ります。 ChatGPT では、互換性レイヤーを通じて同じデータがウィジェットに表示されます。

* `window.openai.toolOutput` contains `structuredContent`
* `window.openai.toolResponseMetadata` contains `_meta`

このウィジェットは、web SDK JavaScript ライブラリの [`applyResponse`](../../js/commands/applyresponse.md) コマンドを使用して、クライアントサイドのステートを改善し、サーバーサイドの `interact` 呼び出しによって返されるパーソナライゼーションの決定をレンダリングします。 [`configure`](../../js/commands/configure/overview.md) を呼び出す前に、必ず `applyResponse` コマンドを呼び出してください。 MCP サーバが `interact` 呼び出しを実行したため、ツール呼び出しのインタラクションで [`sendEvent`](../../js/commands/sendevent/overview.md) コマンドを直ちに呼び出す必要はありません。

```js
// Configure the Web SDK before any other commands.
alloy("configure", {
  datastreamId: "YOUR_DATASTREAM_ID",
  orgId: "YOUR_EXPERIENCE_CLOUD_ORG_ID"
});

// Business data exposed to ChatGPT and the widget.
const { office } = window.openai?.toolOutput ?? {};

// Adobe metadata available only to the widget.
const adobe = window.openai?.toolResponseMetadata?.adobe ?? {};
const { identityMap, handles } = adobe;

// Hydrate client-side state and render personalization decisions from the
// server-side interact response.
alloy("applyResponse", {
  renderDecisions: true,
  responseBody: { handle: handles ?? [] }
});
```

ウィジェットが後で追加の UI サイドイベントを送信する場合、これらの呼び出しに同じ `identityMap` を含めることができます。

```js
alloy("sendEvent", {
  xdm: {
    eventType: "office.details.widgetView",
    identityMap
  }
});
```

このパターンにより、サーバーサイドの ID 呼び出しを analytics の収集および決定の情報源のままにしながら、サーバーサイドと UI サイドの `interact` 使用の整合性を維持します。

## 検証

上記の手順をすべて設定したら、次の点を検証できます。

* **データ収集：** イベントが目的のデータセットに到達しており、各イベントが期待どおりに処理されていることを確認します。
* **Personalization:** 決定がEdge Networkによって返され、それらの決定がウィジェットによってレンダリングされることを確認します。

## セキュリティとプライバシーの考慮事項

* ChatGPT の識別子は、偽名であるにもかかわらず、機密として扱います。
* このワークフローには、組織の同意とデータガバナンスの手法を必ず適用してください。
* Adobeでは、認証に OAuth 2.1 ワークフローを使用することをお勧めします。
* アクセストークンとシークレットがクライアントや UI に到達しないことを確認します。
