---
title: Analyticsでの変数の手動マッピング
seo-title: Web SDKを使用したAnalyticsの変数の手動マッピング
description: 処理ルールを使用して、変数を手動でAnalyticsにマッピングする方法
seo-description: Web SDKの処理ルールを使用して、変数を手動でAnalyticsにマッピングする
translation-type: tm+mt
source-git-commit: 71193ad346c3976f80b14ee0d6e5b12055a17473
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 15%

---


# Analyticsでの変数の手動マッピング

Adobe Experience Platform(AEP)Web SDKは、特定の変数を自動的にマッピングできますが、カスタム変数は手動でマッピングする必要があります。

XDMデータが自動的にAnalyticsにマッピングされない場合は、 [コンテキストデータ](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/vars/page-vars/contextdata.html) を使用して [スキーマに合わせることができます](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html)。 次に、 [処理ルールを使用してAnalytics変数にマッピングし](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html) 、Analytics変数を設定できます。

また、デフォルトのアクションと製品リストのセットを使用して、AEP Web SDKでデータを送信または取得できます。 これを行うには、「 [製品](https://docs.adobe.com/content/help/en/experience-platform/edge/implement/commerce.html)」を参照してください。

## コンテキストデータ

Analyticsが使用するために、XDMデータはドット表記を使用してフラット化され、として使用でき `contextData`ます。 次の値のペアのリストは、次の例を示していま `context data`す。

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

All data collected by the edge network can be accessed via [processing rules](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html). Analyticsでは、処理ルールを使用して、コンテキストデータをAnalytics変数に組み込むことができます。

例えば、次のルールでは、Analyticsを設定して **内部検索用語(eVar2)に** a.x_atag.search.term(Context Data)に関連付けられたデータ **を入力します**。

![](assets/examplerule.png)


## XDMスキーマ

Experience Platformでは、スキーマを使用して、一貫した再利用可能な方法でデータの構造を記述します。 システム間で一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。 Analyticsのコンテキストデータは、スキーマで定義された構造と連携します。

次の例は、AEP Web SDKでデータを送信および取得する [`event`](https://docs.adobe.com/content/help/en/experience-platform/edge/fundamentals/tracking-events.html)`xdm` オプションでコマンドを使用する方法を示しています。 この例では、 `event` コマンドはExperienceEvent Commerce Detailsスキーマに一致し [、productListItems](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md) と `name``SKU` 値が追跡されるようにします。


```
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

AEP Web SDKを使用したイベントのトラッキングについて詳しくは、 [トラッキングイベントを参照してください](https://docs.adobe.com/content/help/en/experience-platform/edge/fundamentals/tracking-events.html)。
