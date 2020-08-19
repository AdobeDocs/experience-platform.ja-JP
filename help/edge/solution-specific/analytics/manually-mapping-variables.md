---
title: Analyticsでの変数の手動マッピング
seo-title: Analyticsの変数とWeb SDKの手動マッピング
description: 処理ルールを使用して変数をAnalyticsに手動でマッピングする方法
seo-description: Web SDKの処理ルールを使用して、変数を手動でAnalyticsにマッピングする
translation-type: tm+mt
source-git-commit: 075d71353877045e12985b3914aaeeb478ed46d6
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 47%

---


# Analyticsでの変数の手動マッピング

Adobe Experience Platform(AEP)は、特定の変数を自動的にマッピングできますが、カスタム変数は手動でマッピングする必要があります。 [!DNL Web SDK]

For XDM data that is not automatically mapped to [!DNL Analytics], you can use [context data](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/vars/page-vars/contextdata.html) to match your [schema](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html). その後、 [!DNL Analytics] 処理ルールを使用してにマッピングし [、](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html)[!DNL Analytics] 変数を設定できます。

Also, you can use a default set of actions and product lists to send or retrieve data with the AEP [!DNL Web SDK]. これをおこなうには、「[製品](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/implement/commerce.html)」を参照してください。

## コンテキストデータ

To be used by [!DNL Analytics], XDM data is flattened using dot notation and made available as `contextData`. 次の値のペアのリストは、`context data` の例を示しています。

```javascript
{
          "bh": "900",
          "bw": "1680",
          "c": "24",
          "c.a.d.key.[0]": "value1",
          "c.a.d.key.[1]": "value2",
          "c.a.d.object.key1": "value1",
          "c.a.d.object.key2.[0]": "value2",
          "c.a.x.environment.browserdetails.javascriptenabled": "true",
          "c.a.x.environment.type": "browser",
          "cust_hit_time_gmt": "1579781427",
          "g": "http://example.com/home",
          "gn": "home",
          "j": "1.8.5",
          "k": "Y",
          "s": "1680x1050",
          "tnta": "218287:1:0|0,218287:1:0|2,218287:1:0|1,218287:1:0|32767,218287:1:0|1,218287:1:0|0,218287:1:0|1,218287:1:0|0,218287:1:0|1",
          "user_agent": "Mozilla/5.0 AppleWebKit/537.36 Safari/537.36",
          "v": "Y"
        }
```

## 処理ルール

エッジネットワークによって収集されたすべてのデータへは、[処理ルール](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html)を介してアクセスできます。In [!DNL Analytics], you can use processing rules to incorporate context data into [!DNL Analytics] variables.

例えば、次のルールでは、Analytics を設定して&#x200B;**内部検索用語（eVar2）** に **a.x_atag.search.term（コンテキストデータ）**&#x200B;に関連付けられたデータを入力します。

![](assets/examplerule.png)


## XDM スキーマ

[!DNL Experience Platform] では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システム間で一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。 [!DNL Analytics] コンテキストデータは、スキーマで定義された構造と連携します。

The following example shows how the [`event` command](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/fundamentals/tracking-events.html) can be used with the `xdm` option to send and retrieve data with the AEP [!DNL Web SDK]. この例では、`event` コマンドは [ExperienceEvent Commerce 詳細スキーマ](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md)に一致し、productListItems の `name` と `SKU` 値が追跡されるようにします。


```javascript
alloy("event",{
  "xdm":{
    "commerce":{
      "productViews":{
        "value":1
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"Large Field Hat",
      },
      {
        "SKU":"HT104",
        "name":"Small Field Hat",
      }
    ]
  }
});
```

For more information on tracking events with the AEP [!DNL Web SDK], see [Tracking events](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/fundamentals/tracking-events.html).
