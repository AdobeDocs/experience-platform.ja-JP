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

* [**[!UICONTROL Adobe Analytics ExperienceEvent フィールドグループ &#x200B;]**](../../xdm/field-groups/event/analytics-full-extension.md) をスキーマに追加してから、[`XDM` オブジェクト &#x200B;](../commands/sendevent/xdm.md) を使用します。
* [`data` オブジェクトを使用して &#x200B;](../commands/sendevent/data.md)XDM スキーマを使用せずにAdobe Analyticsにデータを送信します。
* 自動生成された [&#x200B; コンテキストデータ変数 &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/vars/page-vars/contextdata) および [&#x200B; 処理ルール &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/c-processing-rules/c-processing-rules-configuration/processing-rules-about) を使用します。

## `XDM` オブジェクトの使用 {#use-xdm-object}

Adobe Analyticsに特有の事前定義済みスキーマを使用する場合は、[Adobe Analytics ExperienceEvent スキーマフィールドグループ &#x200B;](../../xdm/field-groups/event/analytics-full-extension.md) をスキーマに追加できます。 追加したら、Web SDK の `xdm` オブジェクトを使用してこのスキーマにデータを入力し、レポートスイートにデータを送信できます。 Edge Networkにデータが到達すると、XDM オブジェクトがAdobe Analyticsで認識できる形式に変換されます。

Web SDK を使用してAdobe Analyticsにデータを送信する方法は 2 つあります。

* [Web SDK タグ拡張機能を使用したAdobe Analyticsへのデータの送信 &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/web-sdk/web-sdk-tag-extension)
* [Web SDK JavaScript ライブラリを使用したAdobe Analyticsへのデータの送信 &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/web-sdk/web-sdk-javascript-library)

XDM フィールドの詳細とAdobe Analytics変数へのマッピング方法については、Adobe Analytics実装ガイドの [XDM オブジェクト変数の Analytics へのマッピング &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/xdm-var-mapping) を参照してください。

## `data` オブジェクトの使用 {#use-data-object}

XDM オブジェクトを使用する代わりに、データオブジェクトを使用することもできます。 データオブジェクトは、現在AppMeasurementを使用している実装に向けられているので、Web SDK へのアップグレードがはるかに容易になります。

Web SDK への移行方法の詳細については、AppMeasurementと Analytics タグ拡張機能のどちらを使用しているかにより、次のガイドを参照してください。

* [Adobe Analyticsのタグ拡張機能から Web SDK のタグ拡張機能に移行する &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/web-sdk/analytics-extension-to-web-sdk)
* [AppMeasurementから Web SDK への移行 &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/web-sdk/appmeasurement-to-web-sdk)

データオブジェクトフィールドの完全なリファレンスとAdobe Analytics変数へのマッピング方法については、Adobe Analytics実装ガイドの [Analytics へのデータオブジェクト変数のマッピング &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/data-var-mapping) に関するドキュメントを参照してください。

## コンテキストデータ変数の使用 {#use-context-data-variables}

自動的にマッピングされない変数は、[&#x200B; コンテキストデータ変数 &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/vars/page-vars/contextdata) として使用できます。 その後、[&#x200B; 処理ルール &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/c-processing-rules/c-processing-rules-configuration/processing-rules-about) を使用して、コンテキストデータ変数を Analytics 変数にマッピングできます。 例えば、次のようなカスタム XDM スキーマがあるとします。

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

Adobe AnalyticsのAppMeasurementでは、ページビューの呼び出し（[`t()` メソッド &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/vars/functions/t-method)）とリンクトラッキングコールの呼び出し（[`tl()` メソッド &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/vars/functions/tl-method)）が別々に使用されます。 代わりに、Web SDK は、ページビューとリンクトラッキングの両方を送信するための [`sendEvent`](../commands/sendevent/overview.md) コマンドのみを提供します。 イベントに含めるデータによって、そのデータがAdobe Analyticsの [&#x200B; ページビュー &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/components/metrics/page-views) または [&#x200B; ページイベント &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics/components/metrics/page-events) かどうかが決まります。

デフォルトでは、すべてのイベントは、Adobe Analyticsではページビューと見なされます。 Web SDK イベントをAdobe Analytics リンクトラッキング呼び出しに設定する場合は、次のフィールドを設定します。

* **XDM オブジェクト**:`xdm.web.webInteraction.name`、`web.webInteraction.type`、`web.webInteraction.URL`
* **データオブジェクト**:`data.__adobe.analytics.linkName`、`data.__adobe.analytics.linkType` および `data.__adobe.analytics.linkURL`
* **コンテキストデータ**：サポートされていません

詳しくは [&#128279;](https://experienceleague.adobe.com/ja/docs/analytics/implementation/vars/functions/tl-method)Adobe Analytics導入ガイドの `tl()` メソッドを参照してください。

`configure` コマンドで [`clickCollectionEnabled`](../commands/configure/clickcollectionenabled.md) を有効にすると、これらのフィールドに値が設定されます。

+++

+++データストリームは、Adobe Analytics向けのデータを使用して他のサービスのデータをどのように区別しますか？

データストリームに送信されたすべてのイベントは、設定済みのすべてのサービスに渡されます。 例えば、パーソナライゼーションと Analytics に対して個別の呼び出しを行う場合、両方のイベントが Analytics と Target に送信されます。 これらのイベントは、Analytics レポートに記録され、バウンス率などの指標に影響を与える可能性があります。

Web SDK を使用する場合、通常、これらの呼び出しは [`sendEvent`](../commands/sendevent/overview.md) コマンドで組み合わされます。

+++
