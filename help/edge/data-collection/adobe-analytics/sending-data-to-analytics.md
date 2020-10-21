---
title: Adobe Analyticsとのページおよびリンクの追跡
seo-title: Adobe Experience PlatformWeb SDKを使用したAdobe Analyticsへのリンクトラッキング
description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
seo-description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web Interaction;page views;link tracking;links;track links;clickCollection;click collection;
translation-type: tm+mt
source-git-commit: c9d777f4350f0b039608c4f9b01d5206994e2572
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 0%

---


# データをAdobe Analyticsに送信中

以前は、ページ表示とリンク(例えば `s.t(), s.tl()`)を区別する機能が異なっていましたが、Web SDKでは、 `sendEvent` コマンドのみが存在しました。 イベントと共に送信するデータによって、ページ表示とリンクのどちらにするかが決まります。 [リンクの追跡について詳しく説明します](../track-links.md)。

## ページ表示の送信

ページの表示は、 `web.webPageDetails.pageViews.value=1` 変数を設定することで指定できます。

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
