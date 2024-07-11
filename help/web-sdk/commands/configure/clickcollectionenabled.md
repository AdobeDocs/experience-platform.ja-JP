---
title: clickCollectionEnabled
description: リンククリックデータが自動的に収集されるかどうかを確認するように Web SDK を設定する方法を説明します。
source-git-commit: 660d4e72bd93ca65001092520539a249eae23bfc
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# `clickCollectionEnabled`

この `clickCollectionEnabled` プロパティは、Web SDK がリンクデータを自動的に収集するかどうかを決定するブール値です。 この変数を設定しない場合、デフォルト値はです `true` つまり、デフォルトでは、リンクトラッキングデータが自動的に収集されます。 このプロパティをに設定します。 `false` は、リンクデータを手動で追跡する場合に役立ちます。

条件 `clickCollectionEnabled` が有効な場合、次の XDM 要素のデータが自動的に入力されます。

* `xdm.web.webInteraction.name`
* `xdm.web.webInteraction.type`
* `xdm.web.webInteraction.URL`

このブール値を有効にすると、デフォルトで内部リンク、ダウンロードリンク、離脱リンクがすべて自動的に追跡されます。 自動リンクトラッキングの制御を強化する場合、Adobeでは次を使用することをお勧めします。 [`clickCollection`](clickcollection.md) オブジェクト。

## 自動リンクトラッキングロジック

Web SDK は、のすべてのクリックを追跡します `<a>` および `<area>` HTML要素（存在しない場合） `onClick` 属性。 クリックは [キャプチャ](https://www.w3.org/TR/uievents/#capture-phase) ドキュメントに添付されているイベントリスナーをクリックします。 有効なリンクをクリックすると、次のロジックが順に実行されます。

1. リンクが、の値に基づく条件に一致する場合 [`downloadLinkQualifier`](downloadlinkqualifier.md)リンクにが含まれている場合はです `download` HTML属性、 `xdm.web.webInteraction.type` はに設定されています。 `"download"` （if `clickCollection.downloadLinkEnabled` 有効）。
1. リンクターゲットドメインが現在のと異なる場合 `window.location.hostname`, `xdm.web.webInteraction.type` はに設定されています。 `"exit"` （if `clickCollection.exitLinkEnabled` 有効）。
1. リンクがのいずれかに該当しない場合 `"download"` または `"exit"`, `xdm.web.webInteraction.type` はに設定されています。 `"other"`.

いずれの場合も、 `xdm.web.webInteraction.name` は、リンクテキストラベルに設定されます。 `xdm.web.webInteraction.URL` は、リンク先の URL に設定されます。 URL へのリンク名も設定する場合は、を使用してこの XDM フィールドを上書きできます `filterClickDetails` でのコールバック `clickCollection` オブジェクト。

## Web SDK タグ拡張機能を使用した自動リンクトラッキングの有効化 {#tag-extension}

「」を選択します **[!UICONTROL クリックデータ収集の有効化]** 次の場合にチェックボックス [タグ拡張機能の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL 設定]** 日 [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. にスクロール ダウンします。 [!UICONTROL データ収集] 「」セクションで、「」チェックボックスを選択します **[!UICONTROL クリックデータ収集の有効化]**.
1. クリック **[!UICONTROL 保存]**&#x200B;を作成してから、変更を公開します。

## Web SDK JavaScript ライブラリを使用して自動リンクトラッキングを有効にします {#library}

を `clickCollectionEnabled` を実行している場合はブール値 `configure` コマンド。 Web SDK の設定時にこのプロパティを省略すると、デフォルトは次のようになります。 `true`. この値をに設定 `false` を設定する場合 `xdm.web.webInteraction.type` および `xdm.web.webInteraction.value` 手動で。

```js
alloy(configure, {
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: false
});
```
