---
title: Web SDKでのページイベントの上部と下部の設定
description: ここでは、Web SDKでページイベントの上部と下部を使用する方法について説明します。
exl-id: 43c6d53a-6bf9-45f8-b001-d148adaff829
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 2%

---


# Web SDKでのページイベントの上部と下部の設定

パーソナライズされたエクスペリエンスを顧客に提供するには、web ページの読み込み時間が不可欠です。

読み込み時間を最適化し、できるだけ早くパーソナライゼーションを実現するために、Web SDKではページイベントの上部と下部の設定をサポートしています。

ページイベントの上部と下部は、ページの読み込み時間を最小限に抑えながら、ページ内の様々な要素を非同期で読み込む方法を示しています。

この設定により、パーソナライズされたコンテンツが読み込まれるまでのユーザーの待機時間を最小限に抑えることができます。

指標の精度に関しては、Adobe Analyticsではページの上部のイベントを無視できるので、より正確な指標の記録につながります。これは、1 つのページヒットしか記録されないからです（ページイベントの下部）。

## ユースケース {#use-cases}

あるスポーツアパレル retailerは、正確に訪問者指標を収集できると同時に、web サイトを訪問する際のユーザーフリクションを最小限に抑え、パーソナライズされたエクスペリエンスを買い物客に提供したいと考えています。

Web SDKのページイベントの上部と下部を使用することで、マーケティングチームは最適な方法でパーソナライゼーション配信を設定できます。

* Web SDKがパーソナライゼーションリクエストを送信し、ページの読み込みが開始されるとすぐに読み込まれます。 これはページの先頭のイベントです。
* ページの読み込みが完了すると、ページビューイベントが記録されます。 これは、ページの読み込みプロセスの後半で発生します。 これはページイベントの下部です。

## Top of page イベントの例 {#top-of-page}

以下のコードサンプルは、自動的にレンダリングされた提案に対して、パーソナライゼーションをリクエストするが [&#x200B; 表示イベントを送信 &#x200B;](../personalization/display-events.md#send-sendEvent-calls) しない、ページイベントのトップ設定の例を示しています。 [&#x200B; ディスプレイイベント &#x200B;](../personalization/display-events.md#send-sendEvent-calls) は、ページ下部イベントの一部として送信されます。

>[!BEGINTABS]

>[!TAB  ページの先頭イベント ]

```js
alloy("sendEvent", {
  type: "decisioning.propositionFetch",
  renderDecisions: true,
  personalization: {
    sendDisplayEvent: false
  }
});
```

| パラメーター | 必須／オプション | 説明 |
|---|---|---|
| `type` | 必須 | このパラメーターを `decisioning.propositionFetch` に設定します。 この特別なイベントタイプは、Adobe Analyticsにこのイベントをドロップするように指示します。 Customer Journey Analyticsを使用する場合は、これらのイベントをドロップするフィルターを設定することもできます。 |
| `renderDecisions` | 必須 | このパラメーターを `true` に設定します。 このパラメーターは、Edge Networkによって返される決定をレンダリングするよう Web SDKに指示します。 |
| `personalization.sendDisplayEvent` | 必須 | このパラメーターを `false` に設定します。 これにより、表示イベントが送信されなくなります。 |

>[!ENDTABS]

## ページの下部のイベントの例 {#bottom-of-page}

>[!BEGINTABS]

>[!TAB  自動レンダリングの提案 ]

以下のコードサンプルは、ページ上で自動的にレンダリングされたが、「ページの先頭 [&#x200B; イベントで表示イベントが抑制された提案の表示イベントを送信するページイベント設定の下 &#x200B;](#top-of-page) 例を示しています。

>[!NOTE]
>
>このシナリオでは、ページイベントの下部 _後_ をページ 1 の上部と呼ぶ必要があります。 ただし、ページイベントの下部は、ページ 1 の上部が完了するまで待つ必要はありません。

```js
alloy("sendEvent", {
  personalization: {
    includeRenderedPropositions: true
  },
  xdm: { ... }
});
```

| パラメーター | 必須／オプション | 説明 |
|---|---|---|
| `personalization.includeRenderedPropositions` | 必須 | このパラメーターを `true` に設定します。 これにより、ページイベントの先頭で抑制された表示イベントの送信が可能になります。 |
| `xdm` | オプション | このセクションを使用して、ページイベントの下部に必要なすべてのデータを含めます。 |

>[!TAB  手動でレンダリングされた提案 ]

以下のコードサンプルは、ページ上で手動でレンダリングされた提案（つまり、カスタム決定範囲またはサーフェス）の表示イベントを送信するページイベント設定の下部を表しています。

>[!NOTE]
>
>このシナリオでは、提案をレンダリングしてページイベントの下部を記録するために、ページイベントの下部は、ページイベントの上部が完了するまで待機する必要があります。

```js
alloy("sendEvent", {
  xdm: { 
    ... // Optional bottom of page event data
    _experience: {
      decisioning: {
        propositions: propositions.map(function(p) {
          return {
            id: p.id,
            scope: p.scope,
            scopeDetails: p.scopeDetails
          };
        }),
        propositionEventType: {
          display: 1
        }
      }
    }
  }
});
```



| パラメーター | 必須／オプション | 説明 |
|---|---|---|
| `xdm._experience.decisioning.propositions` | 必須 | この節では、手動でレンダリングした提案を定義します。 提案 `ID`、`scope`、`scopeDetails` を含める必要があります。 手動でレンダリングされたコンテンツの表示イベントを記録する方法について詳しくは、[&#x200B; 手動でのパーソナライゼーションのレンダリング &#x200B;](../personalization/rendering-personalization-content.md#manually) に関するドキュメントを参照してください。 手動でレンダリングしたパーソナライゼーションコンテンツを、ページヒットの下部に含める必要があります。 |
| `xdm._experience.decisioning.propositionEventType` | 必須 | このパラメーターを `display: 1` に設定します。 |
| `xdm` | オプション | このセクションを使用して、ページイベントの下部に必要なすべてのデータを含めます。 |

>[!ENDTABS]


## 上および下のページヒットを持つ単一ページアプリケーション {#spa-example}


>[!BEGINTABS]

>[!TAB  最初のページビュー ]

次の例には、必須の `xdm.web.webPageDetails.viewName` パラメーターの追加が含まれています。 これが、単一ページアプリケーションの理由です。 この例の `viewName` は、ページの読み込み時に読み込まれるビューです。

```js
// Top of page, render decisions for the "home" view.
alloy("sendEvent", {
    type: "decisioning.propositionFetch",
    renderDecisions: true,
    personalization: {
        sendDisplayEvent: false
    },
    xdm: {
        web: {
            webPageDetails: {
                viewName: "home"
            }
        }
    }
});

// Bottom of page, send display events for the items that were rendered.
// Note: You need to include the viewName in both top and bottom of page so that the
// correct view is rendered at the top of the page, and the correct view is recorded
// at the bottom of the page.

alloy("sendEvent", {
    personalization: {
        includeRenderedPropositions: true
    },
    xdm: {
        ...,
        web: {
            webPageDetails: {
                viewName: "home"
            }
        }
    }
});
```

>[!TAB 2 番目のページビュー（オプション 1） ]

この例では、ページのパーソナライゼーションは既に取得されているので、ページ分割の上下を行う必要はありません。

```js
alloy("sendEvent", {
  renderDecisions: true,
  xdm: {
    ...,
    web: {
      webPageDetails: {
        viewName: "cart"
      }
    }
  }
});
```


>[!TAB 2 番目のページビュー（オプション 2） ]

それでもページヒットの下部を遅延させる必要がある場合は、ページヒットの上部に `applyPropositions` を使用します。 パーソナライゼーションを取得したり、Analytics データを記録したりする必要がないので、Edge Networkにリクエストを行う必要はありません。

```js
// top of page, render the decisions already fetched for the "cart" view.
alloy("applyPropositions", {
    viewName: "cart"
});

// bottom of page, send display events for the items that were rendered.
// Note: You need to include the viewName in both top and bottom of page so that the
// correct view is rendered at the top of the page, and the correct view is recorded
// at the bottom of the page.
alloy("sendEvent", {
    personalization: {
        includeRenderedPropositions: true
    },
    xdm: {
        ...,
        web: {
            webPageDetails: {
                viewName: "cart"
            }
        }
    }
});
```

>[!ENDTABS]

## GitHub のサンプル {#github-sample}

[&#x200B; このアドレス &#x200B;](https://github.com/adobe/alloy-samples/tree/main/target/top-and-bottom) にあるサンプルは、Experience Platformおよび Web SDKを使用して、ページ上部でパーソナライゼーションをリクエストし、下部で分析指標を送信する方法を示しています。 サンプルをダウンロードしてローカルで実行すると、ページイベントの上位と下位の仕組みを理解できます。
