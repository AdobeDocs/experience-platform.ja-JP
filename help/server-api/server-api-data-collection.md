---
title: データ収集
description: Adobe Experience Platform Edge Networkサーバー API で収集したデータを構造化する方法について説明します。
source-git-commit: f52603f7e65ac553e00a2b632857561cd07ae441
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 8%

---


# データ収集

[!DNL Server API] には、次の 2 種類のデータ収集エンドポイントがあります。

* [ インタラクティブなデータ収集エンドポイント ](interactive-data-collection.md)：クライアントがサーバーによって応答が返されることを想定している場合に使用されます。 また、これらのエンドポイントは、データ収集を実行しながら、他のEdge Networkサービスからコンテンツを返すこともできます。
* [ 非インタラクティブイベントデータ収集 ](non-interactive-data-collection.md) は、サーバーからの応答が期待されない場合に使用されます。 これらのエンドポイントは、データ収集にのみ使用されます。

## `Event` オブジェクト {#event-object}

[!DNL Server API] で収集されたデータは、`Event` オブジェクト内に構造化されます。 このオブジェクトの構造については、以下で説明します。

```json
{
   "xdm":{
      "identityMap":{
         "Email_LC_SHA256":[
            {
               "id":"0c7e6a405862e402eb76a70f8a26fc732d07c32931e9fae9ab1582911d2e8a3b",
               "primary":true
            }
         ]
      },
      "eventType":"web.webpagedetails.pageViews",
      "web":{
         "webPageDetails":{
            "URL":"https://alloystore.dev/",
            "name":"home-demo-Home Page",
            "pageViews":{
               "value":1
            }
         }
      },
      "placeContext":{
         "localTime":"2021-08-09T17:09:20.859+03:00",
         "localTimezoneOffset":-180
      },
      "timestamp":"2021-08-09T14:09:20.859Z"
   },
   "data":{
      "prop1":"custom value",
      "var10":"search (organic)"
   }
}
```

| 属性 | タイプ | 説明 |
| --- | --- | --- |
| `xdm` | オブジェクト | *必須*。データセットスキーマに対応する、XDM 形式のデータを含む JSON オブジェクト。 |
| `data` | オブジェクト | *オプション*。Edge Networkが XDM にマッピングできる自由形式のデータを含む JSON オブジェクト。 |

