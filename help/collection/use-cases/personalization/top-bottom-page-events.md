---
title: Web SDKでのページの上部および下部のイベントの設定
description: この記事では、Web SDKでページの上部および下部のイベントを使用する方法について説明します。
exl-id: 43c6d53a-6bf9-45f8-b001-d148adaff829
source-git-commit: 8058ee470717b95d30269a8072b12385c920c85f
workflow-type: tm+mt
source-wordcount: '1170'
ht-degree: 1%

---


# Web SDKでのページの上部および下部のイベントの設定

パーソナライズされた体験を提供するには、web ページの読み込み時間が不可欠です。 ユーザーがパーソナライズされたコンテンツを待つ時間を最小限に抑えるために、Web SDKではページイベントの上下の設定がサポートされています。

ページの上部および下部のイベントは、ページの読み込み時間を最小限に抑えながら、ページ内のさまざまな要素を非同期で読み込む方法を説明します。

* ページの先頭のイベントは、ページの読み込みが始まるとすぐにパーソナライゼーションを要求します。
* ページの読み込みが完了すると、ページの下部にページビューが記録されます。

Adobe Analyticsでは、ページトップのイベントが無視されます。これにより、1回のページヒットのみが記録されるため、より正確な指標の記録が可能になります（ページイベントの下部）。

Web SDK JavaScript ライブラリ （`alloy()`）を直接呼び出すか、Adobe Experience Platform タグ UIでWeb SDK タグ拡張機能を使用することで、ページの上部および下部のイベントを設定できます。 タグ拡張機能の[[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md) アクションには、&#39;[!UICONTROL Use guided events]&#39; （ページの上部）および&#39;[!UICONTROL Request personalization]&#39; （ページの下部）シナリオのフィールド値を事前設定する&#39;[!UICONTROL Collect analytics]&#39; オプションが含まれています。 以下の各例は、両方の実装を示しています。

## ページトップイベント {#top-of-page}

次の例では、パーソナライゼーションを要求するページ上部イベントを設定しますが、自動的にレンダリングされる提案に対して[表示イベント ](display-events.md)を抑制します。 これらの表示イベントは、代わりにページ下部のイベントと共に送信されます。

>[!BEGINTABS]

>[!TAB JavaScript library]

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
| --- | --- | --- |
| `type` | 必須 | このパラメーターを`decisioning.propositionFetch`に設定します。 この特別なイベントタイプは、Adobe Analyticsにこのイベントをドロップするように指示します。 Customer Journey Analyticsを使用する場合は、これらのイベントをドロップするフィルターを設定することもできます。 詳しくは、[Adobe AnalyticsのEdge Network イベントタイプ ](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/hit-types)を参照してください。 |
| `renderDecisions` | 必須 | このパラメーターを`true`に設定します。 このパラメーターは、Web SDKに対して、Edge Networkから返される決定をレンダリングするように指示します。 |
| `personalization.sendDisplayEvent` | 必須 | このパラメーターを`false`に設定します。 このパラメーターは、表示イベントの送信を停止します。 |

>[!TAB Web SDK タグ拡張機能]

ページの上部で発生するルールで[[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md) アクションを設定します。 **[!UICONTROL Use guided events]**&#x200B;を有効にし、**[!UICONTROL Request personalization]**&#x200B;を選択します。 このオプションは、&#39;[!UICONTROL Type]&#39;を&#39;[!UICONTROL Decisioning Proposition Fetch]&#39;にロックし、&#39;[!UICONTROL Render visual personalization decisions]&#39;を有効にし、&#39;[!UICONTROL Automatically send a display event]&#39;を無効にします。

代わりに、これらのフィールドを手動で設定するには、**[!UICONTROL Use guided events]**&#x200B;を無効のままにして、各フィールドを自分で設定します。

>[!ENDTABS]

## ページボトムイベントの例 {#bottom-of-page}

### 自動レンダリング提案 {#bottom-auto-rendered}

次の例では、ページの最下部イベントを設定します。このイベントは、ページ上で自動的にレンダリングされたものの、[ ページの最上部](#top-of-page) イベントで抑制された提案に対して表示イベントを送信します。

>[!BEGINTABS]

>[!TAB JavaScript library]

```js
alloy("sendEvent", {
  personalization: {
    includeRenderedPropositions: true
  },
  xdm: { ... }
});
```

| パラメーター | 必須／オプション | 説明 |
| --- | --- | --- |
| `personalization.includeRenderedPropositions` | 必須 | このパラメーターを`true`に設定します。 このパラメーターを使用すると、ページ上部のイベントで抑制された表示イベントを送信できます。 |
| `xdm` | オプション | このオブジェクトを使用すると、ページの下部のイベントに必要なすべてのデータを含めることができます。 |

>[!TAB Web SDK タグ拡張機能]

ページの下部で発生するルールで[[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md) アクションを設定します。 **[!UICONTROL Use guided events]**&#x200B;を有効にし、**[!UICONTROL Collect analytics]**&#x200B;を選択します。 このオプションは、&#39;[!UICONTROL Include rendered propositions]&#39;を有効にするようにロックします。

代わりに、このフィールドを手動で設定するには、**[!UICONTROL Use guided events]**&#x200B;を無効のままにし、**[!UICONTROL Include rendered propositions]**&#x200B;を直接有効にします。 オプションで、**[!UICONTROL XDM]** フィールドに、ページデータを格納する[XDM オブジェクト ](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object) データ要素を入力します。

>[!ENDTABS]

### 手動でレンダリングされた提案 {#bottom-manually-rendered}

次の例では、ページ上で手動でレンダリングされた提案（カスタム決定範囲またはサーフェス）に対して表示イベントを送信するページ下部イベントを設定します。

>[!NOTE]
>
>このシナリオでは、ページの下部のイベントは、ページの上部のイベントが完了するまで待つ必要があります。これにより、提案をレンダリングして記録できます。

>[!BEGINTABS]

>[!TAB JavaScript library]

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
| --- | --- | --- |
| `xdm._experience.decisioning.propositions` | 必須 | このセクションでは、手動でレンダリングされた提案を定義します。 提案`id`、`scope`および`scopeDetails`を含める必要があります。 詳しくは、[表示イベントの管理](display-events.md)を参照してください。 手動でレンダリングされたパーソナライゼーションコンテンツは、ページイベントの下部に含める必要があります。 |
| `xdm._experience.decisioning.propositionEventType` | 必須 | このパラメーターを`display: 1`に設定します。 |
| `xdm` | オプション | このオブジェクトを使用すると、ページの下部のイベントに必要なすべてのデータを含めることができます。 |

>[!TAB Web SDK タグ拡張機能]

&#39;[!UICONTROL Use guided events]&#39; オプションはこのシナリオをカバーしていないので、アクションを手動で設定します：

1. [XDM オブジェクト ](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object) （または[変数](/help/tags/extensions/client/web-sdk/data-element-types.md#variable)）データ要素を作成し、レンダリングされた各提案の`_experience.decisioning.propositions`、`id`および`scope`を`scopeDetails`に入力し、`_experience.decisioning.propositionEventType.display`を`1`に設定します。 詳しくは、[表示イベントの管理](display-events.md)を参照してください。
1. ページ ルールの下部の[[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md) アクションで、**[!UICONTROL Use guided events]**&#x200B;を無効のままにして、**[!UICONTROL XDM]** フィールドからデータ要素を参照します。

>[!ENDTABS]

## ページの上部および下部にイベントを表示するシングルページアプリケーション {#spa-example}

シングルページアプリケーションでは、Web SDKがページの上部に正しいパーソナライゼーションをレンダリングし、ページの下部に正しいビューを記録するように、各ビューの変更時にビュー名を指定する必要があります。

### 最初のページビュー {#spa-first-view}

この例では、`home`は最初のページ読み込み時に読み込まれたビューです。

>[!BEGINTABS]

>[!TAB JavaScript library]

最上位の呼び出しは、Analytics ヒットを記録したり、表示イベントを起動したりすることなく、`home` ビューのパーソナライゼーションを要求します。 一番下の呼び出しは、ページビューを記録し、非表示の表示イベントを発生させます。 ビューが一貫して記録されるように、両方の呼び出しに同じ`viewName`を含めます。

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

>[!TAB Web SDK タグ拡張機能]

1. [をビューの名前に設定する](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object)XDM オブジェクト `web.webPageDetails.viewName` データ要素を作成します（例：`home`）。
1. ページ [[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md)の先頭アクションを設定します：**[!UICONTROL Use guided events]**&#x200B;を有効にし、**[!UICONTROL Request personalization]**&#x200B;を選択して、**[!UICONTROL XDM]** フィールドのデータ要素を参照します。
1. ページ **[!UICONTROL Send event]**&#x200B;の下部アクションを設定します：**[!UICONTROL Use guided events]**&#x200B;を有効にし、**[!UICONTROL Collect analytics]**&#x200B;を選択し、**[!UICONTROL XDM]** フィールドで同じデータ要素を参照して、`viewName`が両方のイベントで一致するようにします。

>[!ENDTABS]

### 2番目のページビュー – オプション 1 {#spa-second-view-option-1}

この例では、ページのパーソナライゼーションが既に取得されているため、単一のイベントで十分です。

>[!BEGINTABS]

>[!TAB JavaScript library]

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

>[!TAB Web SDK タグ拡張機能]

1. [を新しいビューの名前に設定する](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object)XDM オブジェクト `web.webPageDetails.viewName` データ要素を作成します（例：`cart`）。
1. ビューの変更時に、単一の[[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md) アクションを設定します。**[!UICONTROL Use guided events]**&#x200B;を無効のままにし、**[!UICONTROL Render visual personalization decisions]**&#x200B;を有効にし、**[!UICONTROL XDM]** フィールドのデータ要素を参照します。

>[!ENDTABS]

### 2番目のページビュー – オプション 2 {#spa-second-view-option-2}

この方法は、ページイベントの下部を遅らせる必要がある場合（例えば、ビューの変更時にページの分析データが準備されていない場合）に使用します。 ビューの変更は、次の2つの手順で処理します。

1. ページの上部で、Edge Network呼び出しを行うことなく、既に取得した提案をレンダリングします。
1. 分析データの準備が整ったら、ページの下部のイベントを送信します。

ビューが一貫して記録されるように、両方の呼び出しに同じ`viewName`を含めます。

>[!BEGINTABS]

>[!TAB JavaScript library]

ページの上部にある[`applyPropositions`](/help/collection/js/commands/applypropositions.md)を呼び出して、新しいビューのキャッシュされた提案をレンダリングします。 次に、ページの下部にある`sendEvent`を`includeRenderedPropositions: true`と呼び出して、非表示の表示イベントが発生するようにします。

```js
// Top of page, render the decisions already fetched for the "cart" view.
alloy("applyPropositions", {
    viewName: "cart"
});

// Bottom of page, send display events for the items that were rendered.
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

>[!TAB Web SDK タグ拡張機能]

1. [を新しいビューの名前に設定する](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object)XDM オブジェクト `web.webPageDetails.viewName` データ要素を作成します（例：`cart`）。
1. ページの上部イベントの場合は、[[!UICONTROL Apply propositions]](/help/tags/extensions/client/web-sdk/actions/apply-propositions.md) アクションを設定し、**[!UICONTROL View name]** フィールドをビューの名前に設定します（例：`cart`）。 このアクションは、Edge Networkに連絡することなく、既に取得された提案をレンダリングします。
1. ページイベントの下部で、[[!UICONTROL Send event]](/help/tags/extensions/client/web-sdk/actions/send-event.md) アクションを設定します。**[!UICONTROL Use guided events]**&#x200B;を有効にし、**[!UICONTROL Collect analytics]**&#x200B;を選択して、**[!UICONTROL XDM]** フィールドのデータ要素を参照します。

>[!ENDTABS]

## GitHub サンプル {#github-sample}

alloy-samples リポジトリ [の](https://github.com/adobe/alloy-samples/tree/main/target/top-and-bottom)top-and-bottom サンプルは、ページの上部でパーソナライゼーションをリクエストし、下部で分析指標を送信する方法を示しています。 サンプルをダウンロードしてローカルで実行し、ページの上部と下部のイベントがどのように機能するかを確認します。 サンプルでは、JavaScript ライブラリを直接使用します。Web SDK タグ拡張機能で同等のルールを設定する場合も同じパターンが適用されます。
