---
title: Web SDK を使用したAdobe Analyticsへのデータの送信
description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します。
keywords: Adobe Analytics;Analytics;マッピングされたデータ;マッピングされた var;
exl-id: b18d1163-9edf-4a9c-b247-cd1aa7dfca50
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 9%

---

# Web SDK を使用したAdobe Analyticsへのデータ送信

Adobe Experience Platform Web SDK は、Adobe Experience Platform Edge Network を通じてAdobe Analyticsにデータを送信できます。 データが Edge ネットワークに到達すると、XDM オブジェクトが、Adobe Analyticsが認識できる形式に変換されます。

## XDM フィールドグループ

最も一般的なAdobe Analytics指標を簡単に取り込めるように、Adobeは、Adobe Analytics向けのフィールドグループを提供しています。このグループは、ユーザーが使用できるように、 このスキーマについて詳しくは、 [Adobe Analytics ExperienceEvent Full Extension スキーマフィールドグループ](/help/xdm/field-groups/event/analytics-full-extension.md).

## 変数のマッピング

Edge Network は、多くの XDM 変数を自動的にマッピングします。 詳しくは、 [Edge Network での Analytics 変数マッピング](https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/variable-mapping.html?lang=ja) ( Adobe Analytics実装ガイド ) を参照してください。

自動的にマッピングされない変数は、 [コンテキストデータ変数](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/contextdata.html?lang=ja). その後、 [処理ルール](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/c-processing-rules/c-processing-rules-configuration/processing-rules-about.html) コンテキストデータ変数を Analytics 変数にマッピングする場合。 例えば、次のようなカスタム XDM スキーマがある場合、

```js
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

これらは、処理ルールインターフェイスで使用できるコンテキストデータキーになります。

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

>[!NOTE]
>
>Edge ネットワークコレクションを使用すると、すべてのイベントが Analytics およびお使いのデータストリーム用に設定したその他のサービスに送信されます。 例えば、Analytics と Target の両方をサービスとして設定し、パーソナライゼーションと Analytics に対して別々に呼び出す場合、両方のイベントが Analytics と Target に送信されます。 これらのイベントは Analytics レポートに記録され、バウンス率などの指標に影響を与える可能性があります。

## ページビューとリンクトラッキングコール

Adobe AnalyticsのAppMeasurementでは、ページビューに対して個別のメソッド呼び出し ([`t()` メソッド](https://experienceleague.adobe.com/docs/analytics/implementation/vars/functions/t-method.html)) およびリンクトラッキングコール ([`tl()` メソッド](https://experienceleague.adobe.com/docs/analytics/implementation/vars/functions/tl-method.html)) をクリックします。 Web SDK では、 [`sendEvent`](../commands/sendevent/overview.md) コマンドを使用して、ページビューとリンクトラッキングの両方を送信できます。 イベントに含めるデータによって、イベントが [ページビュー](https://experienceleague.adobe.com/docs/analytics/components/metrics/page-views.html?lang=ja) または [ページイベント](https://experienceleague.adobe.com/docs/analytics/components/metrics/page-events.html) Adobe Analyticsで

デフォルトでは、すべてのイベントがAdobe Analyticsでページビューと見なされます。 Web SDK イベントをAdobe Analyticsリンクトラッキングコールに設定する場合は、次の XDM フィールドを設定します。

* **`web.webInteraction.URL`**：リンク URL。
* **`web.webInteraction.name`**：の値に応じた、カスタムリンク、ダウンロードリンク、または出口リンクのディメンション名 `web.webInteraction.type`
* **`web.webInteraction.type`**：クリックされたリンクのタイプを決定します。 有効な値は `other`（カスタムリンク）、`download`（ダウンロードリンク）、`exit`（離脱リンク）です。

有効にした場合 [`clickCollectionEnabled`](../commands/configure/clickcollectionenabled.md) （内） `configure` コマンドを使用すると、これらの XDM フィールドに値が入力されます。
