---
title: documentUnloading
description: JavaScript sendBeacon API を使用して、Adobeにデータを送信します。
exl-id: 7683c0c4-ae2e-46ec-8471-628a10e17afc
source-git-commit: f12d222e81a39a26bd71ab4bede05aa992889605
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# `documentUnloading`

この `documentUnloading` プロパティでは JavaScript のを使用できます [`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) Adobeにデータを送信する方法。 一般的なリクエストの所要時間が長すぎる場合、ブラウザーはリクエストをキャンセルできます。 使用するように Web SDK に指示できます `sendBeacon` そのため、ページから移動した後、リクエストはバックグラウンドで実行されます。 このプロパティを有効にすると、アンロード時にブラウザーによってデータリクエストがキャンセルされるのを防ぐことができます。

複数のブラウザーでは、で送信できるデータ量に 64 KB の制限が適用されます `sendBeacon` 一度に。 ペイロードが大きすぎるためにブラウザーがイベントを拒否した場合、Web SDK はフォールバックして、通常のトランスポート方法を使用します。

## Web SDK タグ拡張機能を使用したドキュメントのアンロードの設定

を有効にする **[!UICONTROL ドキュメントはアンロードされます]** タグルールのアクション内のチェックボックス。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL イベントを送信]**.
1. を有効にする **[!UICONTROL ドキュメントはアンロードされます]** のチェックボックス [!UICONTROL データ] セクション。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したドキュメントのアンロードの設定

を `documentUnloading` を実行している場合はブール値 `sendEvent` コマンド。 デフォルト値はです `false`. このプロパティをに設定 `true` を使用する場合 `sendBeacon` Adobeにデータを送信する方法。

>[!IMPORTANT]
>
>この `documentUnloading` プロパティは、と互換性がありません [`renderDecisions`](renderdecisions.md) プロパティ。 両方のプロパティをに設定しないでください。 `true` 同時。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "documentUnloading": true
});
```
