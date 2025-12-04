---
title: autoCollectPropositionInteractions
description: リンクがクリックされたときに自動的にデータを収集します。
exl-id: c70db76a-3f2f-45a6-86ab-36efcb18d20f
source-git-commit: c2564f1b9ff036a49c9fa4b9e9ffbdbc598a07a8
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 1%

---

# `autoCollectPropositionInteractions`

`autoCollectPropositionInteractions` プロパティは、Web SDKが提案インタラクションを自動的に収集するかどうかを決定するオプション設定です。 この値は決定プロバイダーのマップで、各決定プロバイダーには、自動提案インタラクションの処理方法を示す値が設定されています。

自動提案インタラクショントラッキングを有効にすると、DOM にレンダリングされた提案要素内のクリックがすべて web SDKによって自動的に収集されます。 このコレクションには、Web SDKによって DOM に自動的にレンダリングされたエクスペリエンスと、[`applyPropositions`](../applypropositions.md) コマンドを使用して DOM にレンダリングされたエクスペリエンスが含まれます。

Web SDKの設定時にこのプロパティを省略した場合、デフォルトは `{"AJO": "always", "TGT": "never"}` になります。 提案のインタラクションを自動的に追跡しない場合は、値を `{"AJO": "never", "TGT": "never"}` に設定します。

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "autoCollectPropositionInteractions": {
    "AJO": "always",
    "TGT": "never"
  }
});
```

このオブジェクトでサポートされるプロパティは次のとおりです。

| プロパティ | 説明 |
| --- | --- |
| **`AJO`** | Adobe Journey Optimizer。 |
| **`TGT`** | Adobe Target。 |

各プロパティの指定可能な値は次のとおりです。

| 値 | 説明 |
| --- | --- |
| **`always`** | 提案に関連付けられた任意の要素に対して、常に `interact` イベントを自動的に収集します。 |
| **`never`** | 提案に関連付けられた要素のイベント `interact` 自動的に収集しません。 |
| **`decoratedElementsOnly`** | 提案 `interact` ラベルまたはトークンを指定するデータ属性が要素に含まれている場合、その提案に関連付けられた要素のイベントを自動的に収集します。 |

## データ属性 {#data-attributes}

要素にデータ属性を使用して、インタラクションに特異性を追加できます。

| 名前 | データ属性 | 説明 |
| --- | --- | --- |
| **[!UICONTROL Label]** | `data-aep-click-label` | クリックされた要素に label data 属性が存在する場合、Edge Networkに送信されるインタラクションの詳細と共に含まれます。 Web SDKは、要素をクリックして DOM ツリーを上に移動し、ラベルデータ属性を探します。 Web SDKは、最初に見つかったラベルを使用します。 |
| **[!UICONTROL Token]** | `data-aep-click-token` | このトークンは、[Adobe Journey Optimizer コードベースのキャンペーン ](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/code-based-experience/get-started-code-based) で決定ポリシーを活用する際に使用します。 トークンを使用すると、クリックされた決定ポリシー項目を区別できます。 クリックされた要素に token data 属性が存在する場合、Edge Networkに送信されるインタラクションの詳細と共に含まれます。 Web SDKは、要素をクリックして DOM ツリーを上に移動しながら、トークン data 属性を探します。 Web SDKは、最初に見つけたトークンを使用します。 |
| **[!UICONTROL Interact ID]** | `data-aep-interact-id` | 提案をレンダリングする際に、Web SDKがコンテナ要素にこの一意の ID を自動的に追加します。 Web SDKは、この ID を使用して DOM 要素を提案に関連付けます。 これは web SDKで必要な ID なので、一切変更しないでください。 無視しても問題ありません。 |

## 例

```html
<div class="row movies" data-aep-interact-id="5">
  <div class="col-md-4 movie" data-aep-click-token="wlpk/z/qyDGoFGF1E47O0w">
    <img src="/img/alpha.jpg" class="poster" />
    <h2>Example Movie Alpha</h2>
    <p class="description"> A lighthearted story about exploration and friendship set on a distant world. Follow a curious rover who discovers that small actions can lead to big changes.</p>
    <p>
      <button class="btn btn-default" data-aep-click-label="view-movie-Example-Alpha">View details</button>
    </p>
  </div>
  <div class="col-md-4 movie" data-aep-click-token="6ZUrou9BVKIsINIAqxylzw">
    <img src="/img/bravo.jpg" class="poster" />
    <h2>Example Movie Bravo</h2>
    <p class="description">An uplifting tale of a determined chef who overcomes unlikely odds to create culinary masterpieces in a bustling city bistro.</p>
    <p>
      <button class="btn btn-default" data-aep-click-label="view-movie-Example-Bravo">View details</button>
    </p>
  </div>
  <div class="col-md-4 movie" data-aep-click-token="QuuXntMRGnCP/AsZHf4pnQ">
    <img src="/img/charlie.jpg" class="poster" />
    <h2>Example Movie Charlie</h2>
    <p class="description">A vibrant adventure following a young musician who journeys into a fantastical realm to find the true meaning of family and tradition.</p>
    <p>
      <button class="btn btn-default" data-aep-click-label="view-movie-Example-Charlie">View details</button>
    </p>
  </div>
</div>
```

### `autoCollectPropositionInteractions` コマンドでの `applyPropositions` の使用 {#apply-propositions}

[`applyPropositions`](../applypropositions.md) コマンドは、DOM に提案をレンダリングする便利な方法です。 ただし、JSON を使用するコードベースのキャンペーンの場合、このコマンドを使用して、既存の DOM 要素（または JSON 値に基づいて画面にレンダリングされたアプリケーションコード）を提案と関連付けることができます。

この関連付けにより、その要素の自動インタラクショントラッキングがアクティブ化され、その要素に適切な提案が割り当てられます。 それには、`actionType` を `track` に設定します。

```javascript
alloy("sendEvent", {
    renderDecisions: true,
}).then((result) => {
    const {
        propositions = []
    } = result;
    const proposition = propositions.find(
        (proposition) => proposition.scope === "web://example.com/#weather-widget"
    );

    if (proposition) {
        renderWeatherWidget(proposition); // custom code that renders the weather widget based on the code-based campaign JSON

        alloy("applyPropositions", {
            propositions: [proposition],
            metadata: {
                "web://example.com/#weather-widget": {
                    selector: "#weather-widget",
                    actionType: "track",
                },
            },
        });
    }
});
```

## Web SDK タグ拡張機能の自動提案インタラクションを設定する

Web SDKのタグ拡張機能を設定する際の次の 2 つのドロップダウンメニューは、このオブジェクトのタグに相当します。

* [[!UICONTROL Auto click collection for Adobe Journey Optimizer]](/help/tags/extensions/client/web-sdk/configure/personalization.md#auto-click-collection-for-adobe-journey-optimizer)
* [[!UICONTROL Auto click collection for Adobe Target]](/help/tags/extensions/client/web-sdk/configure/personalization.md#auto-click-collection-for-adobe-target)
