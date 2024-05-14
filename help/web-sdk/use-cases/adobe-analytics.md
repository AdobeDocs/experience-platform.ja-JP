---
title: Web SDK を使用したAdobe Analyticsへのデータの送信
description: Adobe Experience Platform Web SDK を使用して Adobe Analytics にデータを送信する方法について説明します。
exl-id: b18d1163-9edf-4a9c-b247-cd1aa7dfca50
source-git-commit: 8c652e96fa79b587c7387a4053719605df012908
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 2%

---


# Web SDK を使用したAdobe Analyticsへのデータの送信

Experience PlatformWeb SDK は、Experience PlatformEdge Networkを介してAdobe Analyticsにデータを送信できます。 Adobeでは、Web SDK を使用してAdobe Analyticsにデータを送信するためのいくつかのオプションを提供しています。

* を追加 [**[!UICONTROL Adobe Analytics ExperienceEvent フィールドグループ]**](../../xdm/field-groups/event/analytics-full-extension.md) スキーマに追加した後、 [`XDM` オブジェクト](../commands/sendevent/xdm.md).
* の使用 [`data` オブジェクト](../commands/sendevent/data.md) xdm スキーマを使用せずにAdobe Analyticsにデータを送信する。
* 自動生成されたを使用 [コンテキストデータ変数](https://experienceleague.adobe.com/en/docs/analytics/implementation/vars/page-vars/contextdata) および [処理ルール](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/c-processing-rules/c-processing-rules-configuration/processing-rules-about).

## の使用 `XDM` オブジェクト {#use-xdm-object}

Adobe Analytics固有の事前定義済みスキーマを使用する場合は、 [Adobe Analytics ExperienceEvent スキーマフィールドグループ](../../xdm/field-groups/event/analytics-full-extension.md) をスキーマに追加します。 追加したら、を使用してこのスキーマにデータを入力できます。 `xdm` web SDK でレポートスイートにデータを送信するためのオブジェクトです。 Edge Networkにデータが到達すると、XDM オブジェクトがAdobe Analyticsで認識できる形式に変換されます。

Web SDK を使用してAdobe Analyticsにデータを送信する方法は 2 つあります。

* [Web SDK タグ拡張機能を使用したAdobe Analyticsへのデータの送信](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/web-sdk/web-sdk-tag-extension)
* [Web SDK JavaScript ライブラリを使用したAdobe Analyticsへのデータの送信](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/web-sdk/web-sdk-javascript-library)

参照： [Adobe Analyticsへの XDM オブジェクト変数のマッピング](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/xdm-var-mapping) XDM フィールドの完全なリファレンスと、それらが Analytics 変数にマッピングされる方法については、Adobe Analytics実装ガイドを参照してください。

## の使用 `data` オブジェクト {#use-data-object}

XDM オブジェクトを使用する代わりに、データオブジェクトを使用することもできます。 データオブジェクトは、現在AppMeasurementを使用している実装に向けられているので、Web SDK へのアップグレードがはるかに容易になります。

Web SDK への移行方法の詳細については、AppMeasurementと Analytics タグ拡張機能のどちらを使用しているかにより、次のガイドを参照してください。

* [Adobe Analyticsのタグ拡張機能から Web SDK のタグ拡張機能への移行](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/web-sdk/analytics-extension-to-web-sdk)
* [AppMeasurementから Web SDK への移行](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/web-sdk/appmeasurement-to-web-sdk)

のドキュメントを参照してください。 [Adobe Analyticsへのデータオブジェクト変数のマッピング](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/data-var-mapping) データオブジェクトフィールドとその Analytics 変数へのマッピング方法について詳しくは、Adobe Analytics実装ガイドを参照してください。

## コンテキストデータ変数の使用 {#use-context-data-variables}

自動的にマッピングされない変数は、次のように使用できます [コンテキストデータ変数](https://experienceleague.adobe.com/en/docs/analytics/implementation/vars/page-vars/contextdata). その後、を使用できます [処理ルール](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/c-processing-rules/c-processing-rules-configuration/processing-rules-about) コンテキストデータ変数を Analytics 変数にマッピングする 例えば、次のようなカスタム XDM スキーマがあるとします。

```json
{
  "xdm": {
    "key":"value",
    "animal": {
      "species": "Raven",
      "size": "13 inches"
    },
    "array": [
      "v0",
      "v1",
      "v2"
    ],
    "objectArray":[{
      "ad1": "300x200",
      "ad2": "60x240",
      "ad3": "600x50"
    }]
  }
}
```

その後、これらのフィールドは、処理ルールインターフェイスで使用できるコンテキストデータキーになります。

```javascript
a.x.key //value
a.x.animal.species //Raven
a.x.animal.size //13 inches
a.x.array.0 //v0
a.x.array.1 //v1
a.x.array.2 //v2
a.x.objectarray.0.ad1 //300x200
a.x.objectarray.1.ad2 //60x240
a.x.objectarray.2.ad3 //600x50
```

## FAQ

+++ページビューコールを Web SDK のリンクトラッキングコールと区別するにはどうすればよいですか？

Adobe AnalyticsのAppMeasurementでは、ページビューに対して個別のメソッド呼び出しを使用します（[`t()` メソッド](https://experienceleague.adobe.com/en/docs/analytics/implementation/vars/functions/t-method)）とリンクトラッキングコール （[`tl()` メソッド](https://experienceleague.adobe.com/en/docs/analytics/implementation/vars/functions/tl-method)）に設定します。 Web SDK は、代わりにを提供するだけです [`sendEvent`](../commands/sendevent/overview.md) ページビューとリンクトラッキングの両方を送信するコマンド。 イベントに含めるデータによって、そのデータが [ページビュー](https://experienceleague.adobe.com/en/docs/analytics/components/metrics/page-views) または [ページイベント](https://experienceleague.adobe.com/en/docs/analytics/components/metrics/page-events) Adobe Analyticsで。

デフォルトでは、すべてのイベントは、Adobe Analyticsではページビューと見なされます。 Web SDK イベントをAdobe Analytics リンクトラッキング呼び出しに設定する場合は、次のフィールドを設定します。

* **XDM オブジェクト**: `xdm.web.webInteraction.name`, `web.webInteraction.type`、および `web.webInteraction.URL`
* **データオブジェクト**: `data.__adobe.analytics.linkName`, `data.__adobe.analytics.linkType`、および `data.__adobe.analytics.linkURL`
* **コンテキストデータ**：サポート対象外

を参照してください。 [`tl()` メソッド](https://experienceleague.adobe.com/en/docs/analytics/implementation/vars/functions/tl-method) を参照してください。Adobe Analytics実装ガイド

有効にする場合 [`clickCollectionEnabled`](../commands/configure/clickcollectionenabled.md) が含まれる `configure` コマンドを実行すると、これらのフィールドに値が入力されます。

+++

+++データストリームは、Adobe Analytics向けのデータを使用して他のサービスのデータをどのように区別しますか？

データストリームに送信されたすべてのイベントは、設定済みのすべてのサービスに渡されます。 例えば、パーソナライゼーションと Analytics に対して個別の呼び出しを行う場合、両方のイベントが Analytics と Target に送信されます。 これらのイベントは、Analytics レポートに記録され、バウンス率などの指標に影響を与える可能性があります。

Web SDK を使用する場合、通常、これらの呼び出しは [`sendEvent`](../commands/sendevent/overview.md) コマンド。

+++
