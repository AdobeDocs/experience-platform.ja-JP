---
title: Adobe Analytics と Platform Web SDK の使用
description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します。
keywords: Adobe Analytics;Analytics;マッピングされたデータ;マッピングされた var;
exl-id: b18d1163-9edf-4a9c-b247-cd1aa7dfca50
source-git-commit: 3272db15283d427eb4741708dffeb8141f61d5ff
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 92%

---

# Adobe Analytics と Platform Web SDK の使用

Adobe Experience Platform [!DNL Web SDK] は、Adobe Analytics にデータを送信できます。これは、`xdm` を、Adobe Analytics で使用できる形式に変換することによって機能します。

## セットアップ

顧客設定 UI でレポートスイートがマッピングされている場合、Adobe Analytics はユーザーが送信するデータを自動的に取得します。ここでは、1 つ以上のレポートを特定の設定にマッピングできます。レポートスイートのマッピングが完了したら、データのフローが自動的に開始します。

## XDM フィールドグループ

最も一般的な Adobe Analytics 指標を簡単に取り込めるように、使用できる Analytics フィールドグループを提供しています。このスキーマについて詳しくは、[Adobe Analytics ExperienceEvent 完全拡張スキーマフィールドグループ](../../../xdm/field-groups/event/analytics-full-extension.md)のドキュメントを参照してください

## 自動的にマッピングされたデータ

Adobe Experience Platform [!DNL Edge Network] は、多くの XDM 変数を自動的にマッピングします。これらの変数の完全なリストについては、[こちら](automatically-mapped-vars.md)を参照してください。

## 手動でマッピングされたデータ

[!DNL Edge Network] によって自動的にマッピングされていないデータは、処理ルールを使用してアクセスできます。データはドット表記を使用して統合され、contextData として使用できます。

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

![処理ルールインターフェイス](./assets/edge_analytics_processing_rules.png)

>[!NOTE]
>
>Edge ネットワークコレクションを使用すると、すべてのイベントが Analytics に加えて、データストリーム用に設定した他のサービスに送信されます。 例えば、Analytics と Target の両方をサービスとして設定しており、パーソナライゼーションと Analytics に対して個別の呼び出しを行う場合、両方のイベントが Analytics と Target に送信されます。これらのイベントは、Analytics レポートに記録され、バウンス率などの指標に影響を与える可能性があります。
