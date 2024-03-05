---
title: clickCollectionEnabled
description: リンククリックデータを自動的に収集するかどうかを指定します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---


# `clickCollectionEnabled`

The `clickCollectionEnabled` プロパティは、Web SDK がリンクデータを自動的に収集するかどうかを決定するブール値です。 このプロパティは、リンクデータを手動で追跡する場合に役立ちます。

無効にしない場合、次の XDM 要素にデータが自動的に入力されます。

* `xdm.web.webInteraction.name`
* `xdm.web.webInteraction.type`
* `xdm.web.webInteraction.URL`

## 自動リンクトラッキングロジック

Web SDK は、 `<a>` および `<area>` HTML要素 ( `onClick` 属性。 クリックは、 [キャプチャ](https://www.w3.org/TR/uievents/#capture-phase) ドキュメントに添付されているイベントリスナーをクリックします。 有効なリンクがクリックされると、次のロジックが順番に実行されます。

1. リンクが、 [`downloadLinkQualifier`](downloadlinkqualifier.md)、またはリンクに `download` HTML属性 `xdm.web.webInteraction.type` が `"download"`.
1. リンクターゲットドメインが現在のドメインと異なる場合 `window.location.hostname`, `xdm.web.webInteraction.type` が `"exit"`.
1. リンクが `"download"` または `"exit"`, `xdm.web.webInteraction.type` が `"other"`.

どの場合でも、 `xdm.web.webInteraction.name` がリンクテキストラベルに設定され、 `xdm.web.webInteraction.URL` はリンク先 URL に設定されます。 リンク名を URL にも設定する場合は、 [`onBeforeLinkClickSend`](onbeforelinkclicksend.md).

## Web SDK タグ拡張機能を使用した自動リンクトラッキングの有効化

を選択します。 **[!UICONTROL クリックデータの収集を有効にする]** チェックボックス [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL データ収集] 「 」セクションで、「 」チェックボックスをオンにします。 **[!UICONTROL クリックデータの収集を有効にする]**.
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用した自動リンクトラッキングの有効化

を設定します。 `clickCollectionEnabled` 実行時のブール値 `configure` コマンドを使用します。 Web SDK を設定する際にこのプロパティを省略した場合、デフォルトはになります。 `true`. この値をに設定します。 `false` を手動で設定する場合は、 `xdm.web.webInteraction.type` および `xdm.web.webInteraction.value`.

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "clickCollectionEnabled": false
});
```
