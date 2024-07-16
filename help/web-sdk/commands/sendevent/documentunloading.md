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

`documentUnloading` プロパティを使用すると、JavaScript [`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) メソッドを使用してデータをAdobeに送信できます。 一般的なリクエストの所要時間が長すぎる場合、ブラウザーはリクエストをキャンセルできます。 ページから移動した後にリクエストがバックグラウンドで実行されるように、`sendBeacon` を使用するように Web SDK に指示できます。 このプロパティを有効にすると、アンロード時にブラウザーによってデータリクエストがキャンセルされるのを防ぐことができます。

複数のブラウザーでは、`sendBeacon` と一度に送信できるデータ量に 64 KB の制限が課されています。 ペイロードが大きすぎるためにブラウザーがイベントを拒否した場合、Web SDK はフォールバックして、通常のトランスポート方法を使用します。

## Web SDK タグ拡張機能を使用したドキュメントのアンロードの設定

タグルールのアクション内の「**[!UICONTROL ドキュメントをアンロードします]**」チェックボックスを有効にします。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  アクション ] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL  拡張機能 ]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL  アクションタイプ ] を **[!UICONTROL イベントを送信]** に設定します。
1. 「[!UICONTROL  データ ]」セクションの「**[!UICONTROL ドキュメントはアンロードされます]**」チェックボックスを有効にします。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したドキュメントのアンロードの設定

`sendEvent` コマンドを実行するときは、`documentUnloading` のブール値を設定します。 デフォルト値は `false` です。 `sendBeacon` メソッドを使用してデータをAdobeに送信する場合は、このプロパティを `true` に設定します。

>[!IMPORTANT]
>
>`documentUnloading` プロパティは [`renderDecisions`](renderdecisions.md) プロパティと互換性がありません。 両方のプロパティを同時に `true` に設定しないでください。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "documentUnloading": true
});
```
