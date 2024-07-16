---
title: xdm
description: XDM スキーマ整合オブジェクトを介してAdobeにデータを送信する方法を説明します。
exl-id: 1d8ef191-aed6-4c8b-a1fd-614bd8ed73da
source-git-commit: 8c652e96fa79b587c7387a4053719605df012908
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---

# `xdm`

`xdm` オブジェクトには、Adobeに送信されたデータペイロードが含まれています。 このオブジェクト内で設定されたフィールドは、データセットのスキーマに定義された要素に直接マッピングされます。

Adobe Experience Platformでは、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。 システム間で一貫してデータを定義することで、意味を保持しやすく、データから価値を得やすくなります。

このオブジェクトの上限は 32 KB です。

## Web SDK 拡張機能を使用して XDM オブジェクトを設定

タグルールのアクション内で **[!UICONTROL XDM]** オブジェクトを設定します。 [XDM オブジェクト ](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object) は、他のデータ要素をそれぞれの XDM フィールドにマッピングするための直感的なインターフェイスを提供します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  アクション ] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL  拡張機能 ]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL  アクションタイプ ] を **[!UICONTROL イベントを送信]** に設定します。
1. 目的のオブジェクトを含むデータ要素を **[!UICONTROL XDM]** フィールドで指定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用して XDM オブジェクトを設定します

`sendEvent` コマンドの実行時に `xdm` オブジェクトを設定します。 このオブジェクトの階層が、設定済みのデータセットのスキーマと一致することを確認します。 `xdm` オブジェクトと [`data`](data.md) オブジェクトの両方を同じ `sendEvent` コマンドに含めることができます。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference)
});
```

次の例では、[Commerceの詳細スキーマフィールドグループ ](/help/xdm/field-groups/event/commerce-details.md) を使用しています。

```javascript
alloy("sendEvent",{
  "xdm":{
    "commerce":{
      "productViews":{
        "value":1
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"Large field hat",
      },
      {
        "SKU":"HT104",
        "name":"Small field hat",
      }
    ]
  }
});
```
