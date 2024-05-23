---
title: renderDecisions
description: 自動レンダリングの対象となる、パーソナライズされたコンテンツをレンダリングします。
exl-id: 6f7a3531-c2b6-4e90-a7ad-9f0fe4dc39e9
source-git-commit: f12d222e81a39a26bd71ab4bede05aa992889605
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 0%

---

# `renderDecisions`

この `renderDecisions` プロパティを使用すると、自動レンダリングの対象となるパーソナライズされたコンテンツを Web SDK で強制的にレンダリングできます。

## Web SDK タグ拡張機能を使用してパーソナライズされたコンテンツをレンダリング

「」を選択します **[!UICONTROL ビジュアルパーソナライゼーションの決定のレンダリング]** タグルールのアクション内のチェックボックス。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL イベントを送信]**.
1. にスクロール ダウンします。 [!UICONTROL Personalization] 「」セクションを選択し、 **[!UICONTROL ビジュアルパーソナライゼーションの決定のレンダリング]** チェックボックス。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用してパーソナライズされたコンテンツをレンダリング

を `renderDecisions` を実行している場合はブール値 `sendEvent` コマンド。 省略すると、このプロパティのデフォルト値はになります `false`. このプロパティをに設定 `true` パーソナライズされたコンテンツを自動的にレンダリングする場合。

>[!IMPORTANT]
>
>この `renderDecisions` プロパティは、と互換性がありません [`documentUnloading`](documentunloading.md) プロパティ。 両方のプロパティをに設定しないでください。 `true` 同時。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "renderDecisions": true
});
```
