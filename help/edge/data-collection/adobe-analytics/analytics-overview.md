---
title: Adobe Analyticsと Platform Web SDK の使用
description: Adobe Experience Platform Web SDK を使用してAdobe Analyticsにデータを送信する方法について説明します。
keywords: adobe analytics;analytics；マッピングされたデータ；マッピングされた var;
exl-id: b18d1163-9edf-4a9c-b247-cd1aa7dfca50
source-git-commit: 836fa7814a6966903639e871bfaea0563847f363
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 34%

---

# Adobe Analyticsと Platform Web SDK の使用

ザAdobe Experience Platform [!DNL Web SDK] はAdobe Analyticsにデータを送信できます。 これは、`xdm` を、Adobe Analytics で使用できる形式に変換することによって機能します。

## セットアップ

顧客設定 UI でレポートスイートがマッピングされている場合、Adobe Analytics はユーザーが送信するデータを自動的に取得します。ここでは、1 つ以上のレポートを特定の設定にマッピングできます。レポートスイートのマッピングが完了したら、データのフローが自動的に開始します。

## XDM フィールドグループ

最も一般的なAdobe Analytics指標を簡単に取り込めるように、使用できる Analytics フィールドグループを提供しています。 このスキーマについて詳しくは、 [Adobe Analytics ExperienceEvent Full Extension スキーマフィールドグループ](../../../xdm/field-groups/event/analytics-full-extension.md)

## 自動的にマッピングされたデータ

ザAdobe Experience Platform [!DNL Edge Network] は多くの XDM 変数を自動的にマッピングします。 これらの変数の完全なリストが表示されます [ここ](automatically-mapped-vars.md).

## 手動でマッピングされたデータ

が [!DNL Edge Network] は、処理ルールを使用してアクセスできます。 データはドット表記を使用して統合され、contextData として使用できます。

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
>Experience Edge コレクションを使用すると、すべてのイベントが Analytics に加えて、お客様のデータストリームに設定した他のサービスに送信されます。 例えば、Analytics と Target の両方をサービスとして設定し、パーソナライゼーションと Analytics に対して別々に呼び出す場合、両方のイベントが Analytics および Target に送信されます。 これらのイベントは、Analytics レポートに記録され、バウンス率などの指標に影響を与える可能性があります。
