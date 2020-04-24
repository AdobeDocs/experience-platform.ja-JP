---
title: Adobe Analytics へのデータの送信
seo-title: Adobe Experience Platform Web SDK を使用した Adobe Analytics へのデータの送信
description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します
seo-description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）Adobe Analytics へのデータの送信

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Web SDK は、Adobe Analytics にデータを送信できます。これは、`xdm` を、Adobe Analytics で使用できる形式に変換することによって機能します。

## セットアップ

顧客設定 UI でレポートスイートがマッピングされている場合、Adobe Analytics はユーザーが送信するデータを自動的に取得します。ここでは、1 つ以上のレポートを特定の設定にマッピングできます。レポートスイートのマッピングが完了したら、データのフローが自動的に開始します。

## 自動的にマッピングされたデータ

Adobe Experience Platform Edge Network は、多くの XDM 変数を自動的にマッピングします。自動的にマッピングされた変数の完全なリストについては、[こちら](../analytics/automatically-mapped-vars.md)をご覧ください。

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
    v1,
    v2,
    v3
  ],
  arrayofobjects:[
    {
      obj1key:objval1
    },
    {
      obj2key:objval2
    }
  ]
}
```

これらは、使用可能なコンテキストデータキーになります。

```javascript
a.x.key //value
a.x.object.key1 //value1
a.x.object.key2 //value2
a.x.array[0] //v1
a.x.array[1] //v2
a.x.array[3] //v3
a.x.arrayofobjects[1].obj1key //objval1
a.x.arrayofobjects[2].obj2key //objval2
```

次に、このデータを使用する処理ルールの例を示します。

![処理ルールインターフェイス](../../../assets/edge_analytics_processing_rules.png)
