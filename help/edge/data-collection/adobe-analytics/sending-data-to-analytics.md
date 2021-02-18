---
title: Adobe Experience PlatformWeb SDKを使用したAdobe Analyticsへのデータの送信
description: Adobe Experience PlatformWeb SDKを使用してAdobe Analyticsにデータを送信する方法を学びます。
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web Interaction;page表示;link tracking;links;track links;clickCollection;click collection;
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---


# データをAdobe Analyticsに送信中

以前は、ページ表示とリンク（例：`s.t(), s.tl()`）を区別する関数が異なっていましたが、Web SDKには`sendEvent`コマンドしかありませんでした。 イベントと共に送信するデータによって、ページ表示とリンクのどちらにするかが決まります。 [リンクの追跡について詳しく説明します](../track-links.md)。

## ページ表示の送信

`web.webPageDetails.pageViews.value=1`変数を設定して、ページ表示を指定できます。

```javascript
alloy("sendEvent", {
  "xdm": {
    "web": {
      "webPageDetails": {
        "pageViews": {
            "value":1
         }
      }
    }
  }
});
```

Analyticsでは、この変数が設定されていない場合でもページ表示が技術的に記録されますが、ページ表示を記録してデータに明示的に含めたり、将来の配達確認に導入する場合は、常にこの変数を設定することをお勧めします。
