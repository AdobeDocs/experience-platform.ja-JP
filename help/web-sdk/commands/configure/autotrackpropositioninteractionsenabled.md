---
title: autoTrackPropositionInteractionsEnabled
description: リンクデータを自動収集するようにExperience Platform Web SDK を設定する方法について説明します。
source-git-commit: ec5fd1c8228388ced96f58476e0174c8a0ff00df
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 1%

---


# `autoTrackPropositionInteractionsEnabled`

`autoTrackPropositionInteractionsEnabled` プロパティは、Web SDK が提案インタラクションを自動的に収集するかどうかを決定するオプション設定です。

この値は決定プロバイダーのマップで、各決定プロバイダーには、自動提案インタラクションの処理方法を示す値が設定されています。

## サポートされている値 {#supported-values}

デフォルトでは、自動提案インタラクションは、Adobe Journey Optimizer（`AJO`）には _常に_ 収集され、Adobe Target（`TGT`）には _収集されません_。

デフォルト値の `autoTrackPropositionInteractions` は次のとおりです。

```json
{
  "AJO": "always",
  "TGT": "never"
}
```

各決定プロバイダーでサポートされる設定値について詳しくは、以下の表を参照してください。

| 値 | 説明 |
| --- | --- |
| `always` | 提案に関連付けら [!DNL Web SDK] た任意の要素に対するイベント `interact` 常に自動的に収集されます。 |
| `never` | 提案 [!DNL Web SDK] 関連付けられた要素のイベント `interact` 自動的に収集されることはありません。 |
| `decoratedElementsOnly` | [!DNL Web SDK] は、提案に関連付けられた要素の `interact` イベントを自動的に収集しますが、その要素にラベルまたはトークンを指定するデータ属性が含まれている場合に限ります。 |

## 自動提案インタラクショントラッキング {#logic}

自動提案インタラクショントラッキングを有効にすると、DOM にレンダリングされた提案要素内のクリックが、[!DNL Web SDK] によって自動的に収集されます。 これには、[!DNL Web SDK] によって DOM に自動的にレンダリングされたエクスペリエンスと、[`applyPropositions`](../applypropositions.md) コマンドを使用して DOM にレンダリングされたエクスペリエンスが含まれます。

### データ属性 {#data-attributes}

要素にデータ属性を使用して、インタラクションに特異性を追加できます。

| 名前 | データ属性 | 説明 |
| --- | --- | --- |
| [!DNL Label] | `data-aep-click-label` | クリックされた要素に label データ属性が存在する場合、[!DNL Edge Network] ーザーに送信されたインタラクションの詳細と共に含まれます。 [!DNL Web SDK] は、要素をクリックして DOM ツリーを上に移動し、ラベルデータ属性を探します。 [!DNL Web SDK] は、最初に見つかったラベルを使用します。 |
| [!DNL Token] | `data-aep-click-token` | このトークンは、[Adobe Journey Optimizer コードベースのキャンペーン ](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/code-based-experience/get-started-code-based) で決定ポリシーを活用する際に使用します。 トークンを使用すると、クリックされた決定ポリシー項目を区別できます。 クリックされた要素に token data 属性が存在する場合、Edge Networkに送信されたインタラクションの詳細と共に含まれます。 [!DNL Web SDK] は、要素をクリックして DOM ツリーを上に移動して、トークン データ属性を探します。 [!DNL Web SDK] は、最初に見つけたトークンを使用します。 |
| [!DNL Interact ID] | `data-aep-interact-id` | [!DNL Web SDK] は、提案のレンダリング時に、この一意の ID をコンテナ要素に自動的に追加します。 Web SDK はこの ID を使用して、[!DNL DOM] 要素を提案に関連付けます。 これは [!DNL Web SDK] に必要な ID なので、一切変更しないでください。 無視しても問題ありません。 |

**例**

データ属性の使用例を表示するには、以下のコードスニペットを参照してください。

```html
<div class="row movies" data-aep-interact-id="5">
  <div class="col-md-4 movie" data-aep-click-token="wlpk/z/qyDGoFGF1E47O0w">
    <img src="/img/walle.jpg" class="poster" />
    <h2>WALL·E</h2>
    <p class="description"> In a distant, but not so unrealistic, future where mankind has abandoned earth because it has become covered with trash from products sold by the powerful multi-national Buy N Large corporation, WALL-E, a garbage collecting robot has been left to clean up the mess. </p>
    <p>
      <button class="btn btn-default" data-aep-click-label="view-movie-WALL·E"> View details >> </button>
    </p>
  </div>
  <div class="col-md-4 movie" data-aep-click-token="6ZUrou9BVKIsINIAqxylzw">
    <img src="/img/ratatouille.jpg" class="poster" />
    <h2>Ratatouille</h2>
    <p class="description"> A rat named Remy dreams of becoming a great French chef despite his family's wishes and the obvious problem of being a rat in a decidedly rodent-phobic profession. When fate places Remy in the sewers of Paris, he finds himself ideally situated beneath a restaurant made famous by his culinary hero, Auguste Gusteau. </p>
    <p>
      <button class="btn btn-default" data-aep-click-label="view-movie-Ratatouille"> View details >> </button>
    </p>
  </div>
  <div class="col-md-4 movie" data-aep-click-token="QuuXntMRGnCP/AsZHf4pnQ">
    <img src="/img/coco.jpg" class="poster" />
    <h2>Coco</h2>
    <p class="description"> Despite his family's baffling generations-old ban on music, Miguel dreams of becoming an accomplished musician like his idol, Ernesto de la Cruz. Desperate to prove his talent, Miguel finds himself in the stunning and colorful Land of the Dead following a mysterious chain of events. </p>
    <p>
      <button class="btn btn-default" data-aep-click-label="view-movie-Coco"> View details >> </button>
    </p>
  </div>
</div>
```

### `applyPropositions` コマンド {#apply-propositions}

このコマンドの動作については、[`applyPropositions`](../applypropositions.md) のドキュメントを参照してください。

`applyPropositions` コマンドは、[!DNL DOM] に提案をレンダリングする便利な方法です。 ただし、`JSON` を使用したコードベースのキャンペーンの場合、このコマンドを使用して、既存の [!DNL DOM] 要素（または `JSON` の値に基づいて画面にレンダリングされたアプリケーションコード）を提案に関連付けることができます。

この関連付けにより、その要素の自動インタラクショントラッキングがアクティブ化され、その要素に適切な提案が割り当てられます。 それには、`actionType` を `track` に設定します。

**例**

```javascript
alloy("sendEvent", {
    renderDecisions: true,
}).then((result) => {
    const {
        propositions = []
    } = result;
    const proposition = propositions.find(
        (proposition) => proposition.scope === "web://mywebsite.com/#weather-widget"
    );

    if (proposition) {
        renderWeatherWidget(proposition); // custom code that renders the weather widget based on the code-based campaign JSON

        alloy("applyPropositions", {
            propositions: [proposition],
            metadata: {
                "web://mywebsite.com/#weather-widget": {
                    selector: "#weather-widget",
                    actionType: "track",
                },
            },
        });
    }
});
```

## Web SDK タグ拡張機能を使用した自動提案およびインタラクションのクリック追跡を有効にします {#tag-extension}

1. Adobe ID資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
2. **データ収集**/**タグ** に移動します。
3. Desire タグプロパティを選択します。
4. **拡張機能** に移動し、Adobe Experience Platform Web SDK カードで **設定** を選択します。
5. 「**[!UICONTROL データ収集]**」セクションまでスクロールし、「**提案とインタラクションリンクトラッキングを有効にする**」チェックボックスを選択します。
6. 「**保存**」を選択して、変更を公開します。

## Web SDK JavaScript ライブラリを介した自動提案およびインタラクション リンクトラッキングを有効にします {#library}

[!DNL Web SDK] では、提案トラッキングがデフォルトで有効になっています。 ただし、[`configure`](../configure/overview.md) コマンドを実行する際に `autoTrackPropositionInteractionsEnabled` 値を使用すると、さらに設定できます。

Web SDK の設定時にこのプロパティを省略すると、デフォルトは `{"AJO": "always", "TGT": "never"}` になります。 提案のインタラクションを自動的に追跡しない場合は、値を `{"AJO": "never", "TGT": "never"}` に設定します。

```javascript
alloy("configure", {
   "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
   "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
   "autoTrackPropositionInteractionsEnabled": {"AJO": "always", "TGT": "never"}
});
```
