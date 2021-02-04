---
title: Adobe Analyticsでの変数の手動マッピング
seo-title: Web SDKを使用したAdobe Analyticsの変数の手動マッピング
description: 処理ルールを使用して手動で変数をAdobe Analyticsにマッピングする方法
seo-description: Web SDKでの処理ルールを使用した、手動による変数のAdobe Analyticsへのマッピング
keywords: adobe analytics;analytics；変数；マッピング変数；map変数；contextData;contextData；処理ルール；xdm;スキーマ;
translation-type: tm+mt
source-git-commit: 206b5addd6baf5a120b469b21313ee86ac1fe53b
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 34%

---


# Adobe Analyticsでの変数の手動マッピング

Adobe Experience Platform[!DNL Web SDK]は、特定の変数を自動的にマッピングできますが、カスタム変数は手動でマッピングする必要があります。

[!DNL Analytics]に自動的にマップされないXDMデータの場合は、[コンテキストデータ](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/vars/page-vars/contextdata.html)を使用して[スキーマ](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html)に一致させることができます。 次に、[処理ルール](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html)を使用して[!DNL Analytics]にマッピングし、[!DNL Analytics]変数を設定できます。

また、デフォルトのアクションと製品リストのセットを使用して、Adobe Experience PlatformWeb SDKでデータを送信または取得できます。 これをおこなうには、「[製品](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/implement/commerce.html)」を参照してください。

## コンテキストデータ

[!DNL Analytics]で使用するために、XDMデータはドット表記を使用してフラット化され、`contextData`として利用できます。 次の値のペアのリストは、`context data` の例を示しています。

```json
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

エッジネットワークによって収集されたすべてのデータへは、[処理ルール](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html)を介してアクセスできます。[!DNL Analytics]では、処理ルールを使用して、コンテキストデータを[!DNL Analytics]変数に組み込むことができます。

例えば、次のルールでは、Adobe Analyticsを設定して、**内部検索用語(eVar2)**&#x200B;に&#x200B;**a.x._atag.search.term（コンテキストデータ）**&#x200B;に関連付けられたデータを入力します。

![](assets/examplerule.png)


## XDMスキーマ

Adobe Experience Platformでは、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。 システム間で一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。 [!DNL Analytics] コンテキストデータは、スキーマで定義された構造と連携します。

次の例は、[`event`コマンド](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/fundamentals/tracking-events.html)を`xdm`オプションと組み合わせて使用し、Adobe Experience PlatformWeb SDKでデータを送信および取得する方法を示しています。 この例では、`event` コマンドは [ExperienceEvent Commerce 詳細スキーマ](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md)に一致し、productListItems の `name` と `SKU` 値が追跡されるようにします。


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

Adobe Experience Platform[!DNL Web SDK]とのイベントの追跡について詳しくは、[イベントの追跡](https://docs.adobe.com/content/help/en/experience-platform/edge/fundamentals/tracking-events.html)を参照してください。
