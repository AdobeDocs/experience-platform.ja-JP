---
title: Analyticsでの変数の手動マッピング
seo-title: Web SDKを使用したAnalyticsの変数の手動マッピング
description: 処理ルールを使用して、変数を手動でAnalyticsにマッピングする方法
seo-description: Web SDKの処理ルールを使用して、変数を手動でAnalyticsにマッピングする
translation-type: tm+mt
source-git-commit: 7b07a974e29334cde2dee7027b9780a296db7b20
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 20%

---


# Analyticsでの変数の手動マッピング

Adobe Experience Platform(AEP)は、特定の変数を自動的にマッピングで [!DNL Web SDK] きますが、カスタム変数は手動でマッピングする必要があります。

XDMデータが自動的にマッピングされない場合 [!DNL Analytics]は、コン [テキストデータを使用して](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/vars/page-vars/contextdata.html) スキーマと一致させることができます [](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html)。 その後、 [!DNL Analytics] 処理ルールを使用してにマッピングし [、](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html)[!DNL Analytics] 変数を設定できます。

また、デフォルトのアクションと製品リストのセットを使用して、AEPと共にデータを送信または取得でき [!DNL Web SDK]ます。 これを行うには、「 [製品](https://docs.adobe.com/content/help/en/experience-platform/edge/implement/commerce.html)」を参照してください。

## コンテキストデータ

で使用するに [!DNL Analytics]は、XDMデータをドット表記を使用してフラット化し、として使用できるようにし `contextData`ます。 次の値のペアのリストは、次の例を示していま `context data`す。

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

All data collected by the edge network can be accessed via [processing rules](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules-configuration/t-processing-rules.html). では、処理ルールを使用 [!DNL Analytics]して、コンテキストデータを [!DNL Analytics] 変数に組み込むことができます。

例えば、次のルールでは、Analyticsを設定して、 **内部検索用語(eVar2)に** a.x_atag.search.term（コンテキストデータ）に関連付けられたデータ **を入力しま**&#x200B;す。

![](assets/examplerule.png)


## XDMスキーマ

[!DNL Experience Platform] では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システム間で一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。 [!DNL Analytics] コンテキストデータは、スキーマで定義された構造と連携します。

次の例は、AEPでデータを送信および取得する [`event` オプションと共にこのコマンドを使用する方法を示し](https://docs.adobe.com/content/help/en/experience-platform/edge/fundamentals/tracking-events.html)`xdm`[!DNL Web SDK]ます。 この例では、 `event` コマンドはExperienceEvent Commerce Detailsスキーマに一致し [、productListItems](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md) と `name``SKU` 値が追跡されるようにします。


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

AEPを使用したイベントの追跡について詳し [!DNL Web SDK]くは、「イベントの [追跡](https://docs.adobe.com/content/help/en/experience-platform/edge/fundamentals/tracking-events.html)」を参照してください。
