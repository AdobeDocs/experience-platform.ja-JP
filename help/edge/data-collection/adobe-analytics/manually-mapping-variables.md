---
title: Adobe Experience Platform Web SDK でのAdobe Analytics変数の手動マッピング
description: Experience PlatformWeb SDK の処理ルールを使用して、Adobe Analyticsに手動で変数をマッピングする方法を説明します。
seo-description: Manually map variables into Adobe Analytics using processing rules with Web SDK
keywords: adobe analytics;analytics；変数；マッピング変数；map 変数；contextData；コンテキストデータ；処理ルール；xdm；スキーマ；
exl-id: 395050c1-8d39-4da8-acea-6e618ed662dd
source-git-commit: 9392a90b70699b79949095e178ea77dd34d313a3
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 25%

---

# Adobe Analyticsでの変数の手動マッピング

Adobe Experience Platform [!DNL Web SDK] では特定の変数を自動的にマッピングできますが、カスタム変数は手動でマッピングする必要があります。

[!DNL Analytics] に自動的にマッピングされない XDM データの場合、[ コンテキストデータ ](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/contextdata.html?lang=ja) を使用して、[ スキーマ ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja) と一致させることができます。 次に、[ 処理ルール ](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html?lang=ja) を使用して [!DNL Analytics] にマッピングし、[!DNL Analytics] 変数を設定できます。

また、デフォルトのアクションと製品リストのセットを使用して、Adobe Experience Platform Web SDK でデータを送信または取得できます。 これをおこなうには、[ コマースおよび製品情報の収集 ](https://experienceleague.adobe.com/docs/experience-platform/edge/data-collection/collect-commerce-data.html) を参照してください。

## コンテキストデータ

[!DNL Analytics] で使用するために、XDM データはドット表記を使用してフラット化され、`contextData` として使用できます。 次の値のペアのリストは、`context data` がフラット化された場合の例を示しています。

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

エッジネットワークによって収集されたすべてのデータへは、[処理ルール](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html)を介してアクセスできます。[!DNL Analytics] では、処理ルールを使用して、コンテキストデータを [!DNL Analytics] 変数に組み込むことができます。

例えば、次のルールでは、Adobe Analyticsを設定して **内部検索用語 (eVar2)** に **a.x._atag.search.term(Context Data)** に関連付けられたデータを入力します。

![](assets/examplerule.png)


## XDM スキーマ

Adobe Experience Platformでは、スキーマを使用して、一貫した再利用可能な方法でデータの構造を記述します。 システムをまたいで一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。 [!DNL Analytics] コンテキストデータは、スキーマで定義された構造と連携します。

次の例は、[`event` コマンド ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja) を `xdm` オプションと共に使用して、Adobe Experience Platform Web SDK でデータを送信および取得する方法を示しています。 この例では、`event` コマンドは [ExperienceEvent Commerce 詳細スキーマ](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md)に一致し、productListItems の `name` と `SKU` 値が追跡されるようにします。


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

Adobe Experience Platform [!DNL Web SDK] を使用したイベントの追跡について詳しくは、[ イベントの追跡 ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html) を参照してください。
