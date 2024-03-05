---
title: xdm
description: スキーマに割り当てられたオブジェクト。Adobeに送信されます。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# `xdm`

The `xdm` オブジェクトには、Adobeに送信されるデータペイロードが含まれます。 このオブジェクト内で設定されたフィールドは、データセットのスキーマ内に定義された要素に直接マッピングされます。

Adobe Experience Platformでは、スキーマを使用して、一貫した再利用可能な方法でデータの構造を記述します。 システムをまたいで一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。

このフィールドの上限は 32 KB です。

## Web SDK 拡張機能を使用した XDM オブジェクトの設定

を設定します。 **[!UICONTROL XDM]** フィールドを使用して、タグルールのアクション内に配置できます。 The [XDM オブジェクト](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object) は、他のデータ要素をそれぞれの XDM フィールドにマッピングするための直感的なインターフェイスを提供します。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. 目的のオブジェクトを含むデータ要素を **[!UICONTROL XDM]** フィールドに入力します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用した XDM オブジェクトの設定

を設定します。 `xdm` オブジェクトを `sendEvent` コマンドを使用します。 このオブジェクト内の階層が、設定済みのデータセットのスキーマと一致していることを確認します。 次の `xdm` オブジェクトと [`data`](data.md) 同じ `sendEvent` コマンドを使用します。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference)
});
```

次の例では、 [コマース詳細スキーマフィールドグループ](/help/xdm/field-groups/event/commerce-details.md):

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
