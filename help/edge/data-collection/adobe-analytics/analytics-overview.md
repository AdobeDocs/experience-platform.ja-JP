---
title: Adobe Analytics へのデータの送信
seo-title: Adobe Experience Platform Web SDK を使用した Adobe Analytics へのデータの送信
description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します
seo-description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します
keywords: adobe analytics;analytics；マップされたデータ；マップされた変数；
translation-type: tm+mt
source-git-commit: 723711ee0c2b7b5ca4aea617a81241dbebbc839c
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 77%

---


# データをAdobe Analyticsに送信中

Adobe Experience Platform[!DNL Web SDK]は、Adobe Analyticsにデータを送信できます。 これは、`xdm` を、Adobe Analytics で使用できる形式に変換することによって機能します。

## セットアップ

顧客設定 UI でレポートスイートがマッピングされている場合、Adobe Analytics はユーザーが送信するデータを自動的に取得します。ここでは、1 つ以上のレポートを特定の設定にマッピングできます。レポートスイートのマッピングが完了したら、データのフローが自動的に開始します。

## 自動的にマッピングされたデータ

Adobe Experience Platform[!DNL Edge Network]は自動的に多くのXDM変数をマッピングします。 これらの変数の完全なリストは、[ここ](automatically-mapped-vars.md)に記載されています。

## 手動でマッピングされたデータ

エッジネットワークによって収集されたすべてのデータへは、処理ルールを介してアクセスできます。データはドット表記を使用して統合され、contextData として使用できます。

次のようなスキーマがある場合。

```javascript
{
  key:value,
  object:{
    key1:value1,
    key2:value2
  },
  array:[
    "v0",
    "v1",
    "v2"
  ],
  arrayofobjects:[
    {
      obj1key:objval0
    },
    {
      obj2key:objval1
    }
  ]
}
```

これらは、使用可能なコンテキストデータキーになります。

```javascript
a.x.key //value
a.x.object.key1 //value1
a.x.object.key2 //value2
a.x.array.0 //v0
a.x.array.1 //v1
a.x.array.2 //v2
a.x.arrayofobjects.0.obj1key //objval0
a.x.arrayofobjects.1.obj2key //objval1
```

次に、このデータを使用する処理ルールの例を示します。

![処理ルールインターフェイス](../../../assets/edge_analytics_processing_rules.png)
