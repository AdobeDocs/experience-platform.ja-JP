---
title: xdm
description: XDM スキーマ整合オブジェクトを介してAdobeにデータを送信する方法を説明します。
exl-id: 1d8ef191-aed6-4c8b-a1fd-614bd8ed73da
source-git-commit: c6a2b9700f0a688f65fec9febf5622c6c7b6aafa
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# `xdm`

`xdm` オブジェクトには、Adobeに送信されるデータペイロードが含まれます。 このオブジェクト内で設定されたフィールドは、データセットのスキーマに定義された要素に直接マッピングされます。

Adobe Experience Platformでは、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。 システム間で一貫してデータを定義することで、意味を保持しやすく、データから価値を得やすくなります。

`xdm` コマンドの実行時に `sendEvent` オブジェクトを設定します。 このオブジェクトの階層が、設定済みのデータセットのスキーマと一致することを確認します。 `xdm` オブジェクトと [`data`](data.md) オブジェクトの両方を同じ `sendEvent` コマンドに含めることができます。

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

## Web SDK タグ拡張機能を使用して `xdm` オブジェクトを使用します

Web SDK タグ拡張機能を使用する場合、`xdm` オブジェクトは [ 変数データ要素 ](/help/tags/extensions/client/web-sdk/data-element-types.md#variable) または [XDM オブジェクトデータ要素 ](/help/tags/extensions/client/web-sdk/data-element-types.md#xdm-object) として使用できます。 Adobeでは、ほとんどの場合、可変データ要素を使用することをお勧めします。
