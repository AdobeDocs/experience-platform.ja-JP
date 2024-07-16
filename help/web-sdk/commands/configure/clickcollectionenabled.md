---
title: clickCollectionEnabled
description: リンククリックデータが自動的に収集されるかどうかを確認するように Web SDK を設定する方法を説明します。
source-git-commit: 660d4e72bd93ca65001092520539a249eae23bfc
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# `clickCollectionEnabled`

`clickCollectionEnabled` プロパティは、Web SDK がリンクデータを自動的に収集するかどうかを決定するブール値です。 この変数を設定しない場合、デフォルト値は `true` になります。つまり、リンクトラッキングデータはデフォルトで自動的に収集されます。 このプロパティを `false` に設定すると、リンクデータを手動で追跡する場合に便利です。

`clickCollectionEnabled` が有効な場合、次の XDM 要素が自動的にデータを入力します。

* `xdm.web.webInteraction.name`
* `xdm.web.webInteraction.type`
* `xdm.web.webInteraction.URL`

このブール値を有効にすると、デフォルトで内部リンク、ダウンロードリンク、離脱リンクがすべて自動的に追跡されます。 自動リンクトラッキングをより詳細に制御する場合、Adobeでは [`clickCollection`](clickcollection.md) オブジェクトを使用することをお勧めします。

## 自動リンクトラッキングロジック

`onClick` 属性がない場合、Web SDK は `<a>` および `<area>` のHTML要素に対するすべてのクリックを追跡します。 クリックは、ドキュメントに添付された [capture](https://www.w3.org/TR/uievents/#capture-phase) クリックイベントリスナーでキャプチャされます。 有効なリンクをクリックすると、次のロジックが順に実行されます。

1. リンクが [`downloadLinkQualifier`](downloadlinkqualifier.md) 内の値に基づいて条件に一致する場合、またはリンクに `download`HTML属性が含まれている場合、`xdm.web.webInteraction.type` は `"download"` に設定されます（`clickCollection.downloadLinkEnabled` が有効になっている場合）。
1. リンク ターゲット ドメインが現在の `window.location.hostname` と異なる場合、`xdm.web.webInteraction.type` は `"exit"` に設定されます（`clickCollection.exitLinkEnabled` が有効な場合）。
1. リンクが `"download"` または `"exit"` の対象として認定されない場合、`xdm.web.webInteraction.type` は `"other"` に設定されます。

いずれの場合も、`xdm.web.webInteraction.name` はリンクテキストラベルに設定され、`xdm.web.webInteraction.URL` はリンク先の URL に設定されます。 URL へのリンク名を設定する場合は、`clickCollection` オブジェクトで `filterClickDetails` コールバックを使用して、この XDM フィールドを上書きできます。

## Web SDK タグ拡張機能を使用した自動リンクトラッキングの有効化 {#tag-extension}

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL クリックデータ収集を有効にする]**」チェックボックスを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 ] をクリックします。
1. 「[!UICONTROL  データ収集 ]」セクションまでスクロールし、「**[!UICONTROL クリックデータ収集を有効にする]**」チェックボックスを選択します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用して自動リンクトラッキングを有効にします {#library}

`configure` コマンドを実行するときは、`clickCollectionEnabled` のブール値を設定します。 Web SDK の設定時にこのプロパティを省略すると、デフォルトは `true` になります。 `xdm.web.webInteraction.type` と `xdm.web.webInteraction.value` を手動で設定する場合は、この値を `false` に設定します。

```js
alloy(configure, {
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: false
});
```
