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

この `xdm` オブジェクトには、Adobeに送信されたデータペイロードが含まれます。 このオブジェクト内で設定されたフィールドは、データセットのスキーマに定義された要素に直接マッピングされます。

Adobe Experience Platformでは、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。 システム間で一貫してデータを定義することで、意味を保持しやすく、データから価値を得やすくなります。

このオブジェクトの上限は 32 KB です。

## Web SDK 拡張機能を使用して XDM オブジェクトを設定

を **[!UICONTROL XDM]** タグルールのアクション内のオブジェクト。 この [XDM オブジェクト](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object) には、他のデータ要素をそれぞれの XDM フィールドにマッピングするための直感的なインターフェイスが用意されています。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL イベントを送信]**.
1. 目的のオブジェクトを含むデータ要素を **[!UICONTROL XDM]** フィールド。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用して XDM オブジェクトを設定

を `xdm` を実行しているときのオブジェクト `sendEvent` コマンド。 このオブジェクトの階層が、設定済みのデータセットのスキーマと一致することを確認します。 次の両方を含めることができます `xdm` オブジェクトと [`data`](data.md) 同じオブジェクト `sendEvent` コマンド。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference)
});
```

次の例では、 [Commerceの詳細スキーマフィールドグループ](/help/xdm/field-groups/event/commerce-details.md):

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
