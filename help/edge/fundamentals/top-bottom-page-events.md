---
title: ページの上部と下部のイベントの使用
description: この記事では、Web SDK でページイベントの上部と下部を使用する方法について説明します。
source-git-commit: 221a9348803e111a1842b3abf2e74f7408da5994
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 2%

---


# Web SDK でのページイベントの上部および下部の使用

パーソナライズされたエクスペリエンスを顧客に提供する場合は、Web ページの読み込み時間が不可欠です。

読み込み時間を最適化し、パーソナライゼーションを可能な限り迅速に配信するために、Web SDK はページイベントの上部と下部の設定をサポートします。

ページイベントの上部と下部では、ページの読み込み時間を最小限に抑えながら、ページ内の様々な要素を非同期で読み込む方法を説明します。

この設定により、パーソナライズされたコンテンツが読み込まれるまでユーザーが待つ時間を最小限に抑えることができます。

指標の精度では、Adobe Analyticsでページイベントの先頭を無視できるので、1 ページのヒットのみが記録され（ページイベントの下部）、より正確な指標が記録されます。

## ユースケース {#use-cases}

スポーツアパレル小売業者は、訪問者指標を正確に収集できながら、Web サイトを訪問する際のユーザーの摩擦を最小限に抑えながら、パーソナライズされたエクスペリエンスを買い物客に提供したいと考えています。

Web SDK でページの上部と下部のイベントを使用すると、マーケティングチームは、最適な方法でパーソナライゼーション配信を設定できます。

* Web SDK は、ページの読み込みが開始するとすぐに読み込まれるパーソナライゼーションリクエストを送信します。 これはページイベントの先頭です。
* ページの読み込みが終了すると、ページビューイベントが記録されます。 これは、ページの読み込みプロセスの後半で発生します。 これはページイベントの下部です。

## ページの先頭のイベントの例 {#top-of-page}

以下のコードサンプルは、パーソナライゼーションをリクエストするが、をリクエストしないページイベント設定の例です [ディスプレイイベントを送信](../personalization/display-events.md#send-sendEvent-calls) 自動的にレンダリングされた提案用。 The [イベントを表示](../personalization/display-events.md#send-sendEvent-calls) は、ページ下部のイベントの一部として送信されます。

>[!BEGINTABS]

>[!TAB ページイベントの先頭]

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
| `type` | 必須 | このパラメーターをに設定します。 `decisioning.propositionFetch`. この特別なイベントタイプは、Adobe Analyticsにこのイベントをドロップするように指示します。 Customer Journey Analyticsを使用する場合、これらのイベントをドロップするフィルターを設定することもできます。 |
| `renderDecisions` | 必須 | このパラメーターをに設定します。 `true`. このパラメーターは、Edge Network から返される決定をレンダリングするよう Web SDK に指示します。 |
| `personalization.sendDisplayEvent` | 必須 | このパラメーターをに設定します。 `false`. これにより、表示イベントの送信が停止します。 |

>[!ENDTABS]

## ページ下部のイベントの例 {#bottom-of-page}

>[!BEGINTABS]

>[!TAB 自動レンダリングされた提案]

以下のコード例は、ページ上で自動的にレンダリングされたが、で表示イベントが抑制された提案の表示イベントを送信するページイベント設定の下部を示しています。 [ページの先頭](#top-of-page) イベント。

>[!NOTE]
>
>このシナリオでは、ページの下部にあるイベントを呼び出す必要があります _次より後_ 1 ページ目の先頭 ただし、ページの一番上のイベントが完了するまで、ページの一番下のイベントを待つ必要はありません。

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
| `personalization.includeRenderedPropositions` | 必須 | このパラメーターをに設定します。 `true`. これにより、ページイベントの先頭で抑制された表示イベントを送信できます。 |
| `xdm` | オプション | このセクションを使用して、ページイベントの下部に必要なすべてのデータを含めます。 |

>[!TAB 手動でレンダリングされた提案]

以下のコード例は、ページ上で手動でレンダリングされた提案の表示イベントを送信するページイベント設定の下部を示しています（例：カスタム決定スコープまたはサーフェス）。

>[!NOTE]
>
>このシナリオでは、ページの下部のイベントは、ページのイベントの上部が完了するまで待ち、提案をレンダリングしてページの下部のイベントを記録する必要があります。

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
| `xdm._experience.decisioning.propositions` | 必須 | この節では、手動でレンダリングした提案を定義します。 提案を含める必要があります `ID`, `scope`、および `scopeDetails`. 方法に関するドキュメントを参照してください。 [パーソナライゼーションの手動レンダリング](../personalization/rendering-personalization-content.md#manually) 手動でレンダリングしたコンテンツの表示イベントを記録する方法の詳細については、を参照してください。 手動でレンダリングしたパーソナライゼーションコンテンツをページヒットの下部に含める必要があります。 |
| `xdm._experience.decisioning.propositionEventType` | 必須 | このパラメーターをに設定します。 `display: 1`. |
| `xdm` | オプション | このセクションを使用して、ページイベントの下部に必要なすべてのデータを含めます。 |

>[!ENDTABS]


## 上部および下部ページのヒットがある単一ページアプリケーション {#spa-example}


>[!BEGINTABS]

>[!TAB 最初のページ表示]

以下の例に、必要な `xdm.web.webPageDetails.viewName` パラメーター。 このため、単一ページアプリケーションになります。 The `viewName` この例では、ページ読み込み時に読み込まれるビューです。

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

>[!TAB 2 番目のページビュー（オプション 1）]

この例では、ページのパーソナライゼーションは既に取得されているので、ページの上下に分割する必要はありません。

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


>[!TAB 2 番目のページビュー（オプション 2）]

それでもページヒットの一番下の処理を遅らせる必要がある場合は、 `applyPropositions` ページヒットのトップ。 パーソナライゼーションを取得する必要も、Analytics データを記録する必要もないので、Edge ネットワークにリクエストを送信する必要はありません。

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

次の場所にあるサンプル： [この住所](https://github.com/adobe/alloy-samples/tree/main/top-and-bottom) では、Experience Platformと Web SDK を使用して、ページの上部でパーソナライゼーションをリクエストし、下部で analytics 指標を送信する方法について説明します。 サンプルをダウンロードしてローカルで実行し、ページイベントの上部と下部の動作を理解できます。
