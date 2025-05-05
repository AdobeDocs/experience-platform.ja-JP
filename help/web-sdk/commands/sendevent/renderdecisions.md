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

`renderDecisions` プロパティを使用すると、自動レンダリングの対象となるパーソナライズされたコンテンツを Web SDK で強制的にレンダリングできます。

## Web SDK タグ拡張機能を使用してパーソナライズされたコンテンツをレンダリング

タグルールのアクション内の **[!UICONTROL ビジュアルパーソナライゼーション決定をレンダリング]** チェックボックスを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL &#x200B; 拡張機能 &#x200B;]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL イベントを送信]** に設定します。
1. 「[!UICONTROL Personalization]」セクションまでスクロールダウンし、「**[!UICONTROL ビジュアルパーソナライゼーションの決定をレンダリング]** チェックボックスを選択します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用してパーソナライズされたコンテンツをレンダリング

`sendEvent` コマンドを実行するときは、`renderDecisions` のブール値を設定します。 省略すると、このプロパティは既定で `false` に設定されます。 パーソナライズされたコンテンツを自動的にレンダリングする場合は、このプロパティを `true` に設定します。

>[!IMPORTANT]
>
>`renderDecisions` プロパティは [`documentUnloading`](documentunloading.md) プロパティと互換性がありません。 両方のプロパティを同時に `true` に設定しないでください。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "renderDecisions": true
});
```
