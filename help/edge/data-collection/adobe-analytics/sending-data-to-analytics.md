---
title: Adobe Experience Platform Web SDK を使用したAdobe Analyticsへのデータの送信
description: Adobe Experience Platform Web SDK を使用してAdobe Analyticsにデータを送信する方法について説明します。
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web インタラクション；ページビュー；リンクトラッキング；リンク；clickCollection;click collection;
exl-id: cec4a9eb-2079-4386-88da-9b995e0673e6
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# Adobe Analyticsへのデータの送信

一方、以前は、ページビューとリンク ( 例えば、 `s.t(), s.tl()`)、Web SDK には、 `sendEvent` コマンドを使用します。 イベントと共に送信するデータによって、ページビューとリンクのどちらにするかが決まります。 [トラッキングリンクの詳細を説明します](../track-links.md).

## ページビューの送信

ページビューを指定するには、 `web.webPageDetails.pageViews.value=1` 変数を使用します。

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

Analytics は、この変数が設定されていない場合でも、ページビューを技術的に記録しますが、ページビューをデータに明示的に記録し、実装に将来的に配達可能にする場合は、常にこの変数を設定することをお勧めします。
