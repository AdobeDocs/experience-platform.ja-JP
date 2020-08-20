---
title: Adobe Analyticsとのページおよびリンクの追跡
seo-title: Adobe Experience PlatformWeb SDKを使用したAdobe Analyticsへのリンクトラッキング
description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
seo-description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web Interaction;page views;link tracking;links;track links;clickCollection;click collection;
translation-type: tm+mt
source-git-commit: 8c256b010d5540ea0872fa7e660f71f2903bfb04
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 2%

---


# Adobe Analytics へのデータの送信

以前は、ページ表示とリンク(例えば `s.t(), s.tl()`)の間で識別する関数が異なっていましたが、Web SDKでは、 `sendEvent` コマンドしかありません。 イベントと共に送信するデータによって、ページ表示とリンクのどちらにするかが決まります。

## ページ表示の送信

ページの表示は、 `web.webPageDetails.pageViews.value=1` 変数を設定することで指定できます。

```javascript
alloy("sendEvent", {
  "xdm": {
    "web": {
      "webPageDetailsr": {
        "pageViews": {
            "value":1
      }
    }
  }
});
```

Analyticsでは、この変数が設定されていない場合でもページ表示が技術的に記録されますが、ページ表示をデータに明示的に記録し、導入を後で実行する場合は常に、この変数を設定することをお勧めします。

## リンクの追跡

スキーマの `web.webInteraction` 部分に詳細を追加することで、リンクを設定できます。 次の3つの必須変数があります。 `web.webInteraction.name`、 `web.webInteraction.type` および `web.webInteraction.linkClicks.value`。

```javascript
alloy("sendEvent", {
  "xdm": {
    "web": {
      "webInteraction": {
        "linkClicks": {
            "value":1
      },
      "name":"My Custom Link", //Name that shows up in the custom links report
      "URL":"https://myurl.com", //the URL of the link
      "type":"other", // values: other, download, exit
    }
  }
});
```

リンクタイプは、次の3つの値のいずれかになります。

* **`other`:** カスタムリンク
* **`download`:** ダウンロードリンク（これらはライブラリで自動的に追跡できます）
* **`exit`:** 離脱リンク

### 自動リンクトラッキング

Web SDKは、clickCollectionを有効にすることで、すべてのリンクのクリックを自動的に追跡でき [ます](../../fundamentals/configuring-the-sdk.md#clickCollectionEnabled)。

ダウンロードリンクは、よく使用されるファイルタイプに基づいて自動的に検出されます。 ダウンロードの分類方法のロジックを設定できます。
