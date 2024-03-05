---
title: documentUnloading
description: JavaScript sendBeacon API を使用して、データをAdobeに送信します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 2%

---

# `documentUnloading`

The `documentUnloading` プロパティを使用すると、JavaScript の [`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) メソッドを使用してデータをAdobeに送信します。 一般的なリクエストに時間がかかりすぎる場合、ブラウザーはリクエストをキャンセルできます。 Web SDK に、 `sendBeacon` ページから移動した後でリクエストがバックグラウンドで実行されるようにするために使用します。 このプロパティを有効にすると、データ要求がアンロード時にブラウザーによってキャンセルされるのを防ぐことができます。

一部のブラウザーでは、で送信できるデータ量に対して 64 KB の制限が設けられています。 `sendBeacon` 一度に ペイロードが大きすぎるのでブラウザーがイベントを拒否した場合、Web SDK は、通常の転送方法を使用するようにフォールバックします。

## Web SDK タグ拡張を使用したドキュメントのアンロードの設定

を有効にします。 **[!UICONTROL ドキュメントはアンロードされます]** 」チェックボックスをオンにします。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. を有効にします。 **[!UICONTROL ドキュメントはアンロードされます]** 」チェックボックスをオンにします。 [!UICONTROL データ] 」セクションに入力します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したドキュメントのアンロードの設定

を設定します。 `documentUnloading` 実行時のブール値 `sendEvent` コマンドを使用します。 デフォルト値は `false` です。このプロパティをに設定します。 `true` を使用する場合、 `sendBeacon` メソッドを使用してデータをAdobeに送信します。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "documentUnloading": true
});
```
