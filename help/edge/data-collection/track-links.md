---
title: Adobe Experience Platform Web SDK を使用したリンクの追跡
description: Experience PlatformWeb SDK を使用してAdobe Analyticsにリンクデータを送信する方法について説明します
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web インタラクション；ページビュー；リンクトラッキング；リンク；clickCollection;click collection;
exl-id: d5a1804c-8f91-4083-a46e-ea8f7edf36b6
source-git-commit: dac14cd358922b577c71f8d9b7f7c9b7e1b4f87d
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# リンクのトラッキング

リンクは手動で設定することも、追跡することもできます [自動](#automaticLinkTracking). 手動トラッキングは、 `web.webInteraction` スキーマの一部。 次の 3 つの必須変数があります。

* `web.webInteraction.name`
* `web.webInteraction.type`
* `web.webInteraction.linkClicks.value`

```javascript
alloy("sendEvent", {
  "xdm": {
    "web": {
      "webInteraction": {
        "linkClicks": {
            "value": 1
        },
        "name": "My Custom Link", // Name that shows up in the custom links report
        "URL": "https://myurl.com", // The URL of the link
        "type": "other" // values: other, download, exit
      }
    }
  }
});
```

リンクタイプは、次の 3 つの値のいずれかになります。

* **`other`:** カスタムリンク
* **`download`:** ダウンロードリンク
* **`exit`:** 出口リンク

これらの値は次のとおりです。 [自動的にマッピング](adobe-analytics/automatically-mapped-vars.md) Adobe Analyticsに [設定済み](adobe-analytics/analytics-overview.md) そうする必要がある。

## 自動リンクトラッキング {#automaticLinkTracking}

デフォルトでは、Web SDK は、対象となるリンクタグのクリックを取得、ラベル付けし、記録します。 クリックは、 [キャプチャ](https://www.w3.org/TR/uievents/#capture-phase) ドキュメントに添付されているイベントリスナーをクリックします。

自動リンクトラッキングは、 [設定](../fundamentals/configuring-the-sdk.md#clickCollectionEnabled) Web SDK。

```javascript
clickCollectionEnabled: false
```

### リンクトラッキングに適したタグは何ですか？{#qualifyingLinks}

アンカーの自動リンクトラッキングが実行されました `A` および `AREA` タグ。 ただし、これらのタグがリンクトラッキングの対象とならないのは、 `onclick` ハンドラ

### リンクにラベルは付きますか？{#labelingLinks}

アンカータグにダウンロード属性が含まれる場合、またはリンクが一般的なファイル拡張子で終わる場合、リンクはダウンロードリンクとしてラベル付けされます。 ダウンロードリンク修飾子は次の場合があります。 [設定済み](../fundamentals/configuring-the-sdk.md) を次の正規表現で指定します。

```javascript
downloadLinkQualifier: "\\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
```

リンクのターゲットドメインが現在のドメインと異なる場合、リンクは出口リンクとしてラベル付けされます `window.location.hostname`.

ダウンロードリンクまたは出口リンクと見なされないリンクは、「その他」とラベル付けされます。

### リンクトラッキング値をフィルターするにはどうすればよいですか？

自動リンクトラッキングで収集されたデータを調べ、フィルタリングするには、 [onBeforeEventSend コールバック関数](../fundamentals/tracking-events.md#modifying-events-globally).

リンクトラッキングデータのフィルタリングは、Analytics レポート用のデータを準備する際に役立ちます。 自動リンクトラッキングでは、リンク名とリンク URL の両方がキャプチャされます。 Analytics レポートでは、リンク名がリンク URL よりも優先されます。 リンク URL を報告する場合は、リンク名を削除する必要があります。 次の例は、 `onBeforeEventSend` 関数を使用して、ダウンロードリンクのリンク名を削除できます。

```javascript
alloy("configure", {
  onBeforeEventSend: function(options) {
    if (options
      && options.xdm
      && options.xdm.web
      && options.xdm.web.webInteraction) {
        if (options.xdm.web.webInteraction.type === "download") {
          options.xdm.web.webInteraction.name = undefined;
        }
    }
  }
});
```

