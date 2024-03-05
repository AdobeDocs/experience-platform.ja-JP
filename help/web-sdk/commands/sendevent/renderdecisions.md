---
title: renderDecisions
description: 自動レンダリングの対象となる、パーソナライズされたコンテンツをレンダリングします。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# `renderDecisions`

The `renderDecisions` プロパティを使用すると、Web SDK に対して、自動レンダリングの対象となるパーソナライズされたコンテンツのレンダリングを強制できます。

## Web SDK タグ拡張機能を使用してパーソナライズされたコンテンツをレンダリングする

を選択します。 **[!UICONTROL 視覚的なパーソナライゼーションの決定をレンダリング]** 」チェックボックスをオンにします。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. 下にスクロールして、 [!UICONTROL パーソナライズ] 「 」セクションで、 **[!UICONTROL 視覚的なパーソナライゼーションの決定をレンダリング]** チェックボックス。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用してパーソナライズされたコンテンツをレンダリング

を設定します。 `renderDecisions` 実行時のブール値 `sendEvent` コマンドを使用します。 省略した場合、このプロパティはデフォルトでになります。 `false`. このプロパティをに設定します。 `true` パーソナライズされたコンテンツを自動的にレンダリングする場合

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "renderDecisions": true
});
```
