---
title: データの収集
description: Adobe Experience Platform Edge Network Server API が収集したデータを構造化する方法を説明します
seo-description: Learn how the Adobe Experience Platform Edge Network Server API structures the collected data
keywords: データ収集；コレクション；Adobe Experience Platform Edge Network;API；構造
source-git-commit: 2b501c9384fecb016489a21a308a595186153f03
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 8%

---


# データの収集

この [!DNL Server API] は、次の 2 種類のデータ収集エンドポイントを提供します。

* [インタラクティブデータ収集エンドポイント](interactive-data-collection.md)：クライアントがサーバーから応答が返されることを想定する場合に使用されます。 また、これらのエンドポイントは、データ収集を実行中に、他の Edge ネットワークサービスからコンテンツを返すこともできます。
* [非インタラクティブイベントデータ収集](non-interactive-data-collection.md)：サーバーからの応答が期待されない場合に使用します。 これらのエンドポイントは、データ収集にのみ使用されます。

## `Event` オブジェクト {#event-object}

で収集されたデータ [!DNL Server API] が `Event` オブジェクト。 このオブジェクトの構造を以下に示します。

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
| `xdm` | オブジェクト | *必須*。データセットスキーマに対応する XDM 形式のデータを含む JSON オブジェクト。 |
| `data` | オブジェクト | *オプション*. フリーフォームデータを含む JSON オブジェクト。Edge ネットワークによって XDM にマッピングできます。 |

