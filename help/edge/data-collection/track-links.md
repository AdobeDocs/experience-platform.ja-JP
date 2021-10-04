---
title: Adobe Experience Platform Web SDK を使用したリンクの追跡
description: Experience PlatformWeb SDK を使用してAdobe Analyticsにリンクデータを送信する方法を説明します
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web インタラクション；ページビュー；リンクトラッキング；リンク；clickCollection;click collection;
exl-id: d5a1804c-8f91-4083-a46e-ea8f7edf36b6
source-git-commit: d6460e442a136bf9bd26582f228017a6fb52138c
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# リンクのトラッキング

リンクは手動で設定することも、[ 自動的に ](#automaticLinkTracking) 追跡することもできます。 手動トラッキングは、スキーマの `web.webInteraction` 部分に詳細を追加することでおこなわれます。 次の 3 つの必須変数があります。

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
        }
      },
      "name": "My Custom Link", // Name that shows up in the custom links report
      "URL": "https://myurl.com", // The URL of the link
      "type": "other" // values: other, download, exit
    }
  }
});
```

リンクタイプは、次の 3 つの値のいずれかになります。

* **`other`:** カスタムリンク
* **`download`:** ダウンロードリンク
* **`exit`:** 出口リンク

[ 設定 ](adobe-analytics/analytics-overview.md) すると、これらの値は [ 自動的にAdobe Analyticsにマッピングされます。](adobe-analytics/automatically-mapped-vars.md)

## 自動リンクトラッキング {#automaticLinkTracking}

デフォルトでは、Web SDK は、対象となるリンクタグのクリックをキャプチャ、ラベル付け、記録します。 クリック数は、ドキュメントに添付された [capture](https://www.w3.org/TR/uievents/#capture-phase)click イベントリスナーを使用してキャプチャされます。

自動リンクトラッキングは、Web SDK を [ 設定 ](../fundamentals/configuring-the-sdk.md#clickCollectionEnabled) することで無効にできます。

```javascript
clickCollectionEnabled: false
```

### リンクトラッキングに適したタグは何ですか？{#qualifyingLinks}

自動リンクトラッキングは、アンカーの `A` タグと `AREA` タグに対しておこなわれます。 ただし、`onclick` ハンドラーが添付されている場合、これらのタグはリンクトラッキングの対象とは見なされません。

### リンクにはどのようなラベルが付けられますか。{#labelingLinks}

アンカータグにダウンロード属性が含まれる場合や、リンクの末尾が一般的なファイル拡張子の場合、リンクにダウンロードリンクというラベルが付けられます。 ダウンロードリンク修飾子は、正規表現を使用して [ 設定 ](../fundamentals/configuring-the-sdk.md) できます。

```javascript
downloadLinkQualifier: "\\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
```

リンクターゲットドメインが現在の `window.location.hostname` と異なる場合、リンクに出口リンクというラベルが付けられます。

ダウンロードリンクまたは出口リンクと見なされないリンクは、「その他」とラベル付けされます。

### リンクトラッキングの値は、どのようにフィルタリングできますか？

自動リンクトラッキングで収集されたデータを調べ、フィルタリングするには、[onBeforeEventSend コールバック関数 ](../fundamentals/tracking-events.md#modifying-events-globally) を指定します。

リンクトラッキングデータのフィルタリングは、Analytics レポート用のデータを準備する際に役立ちます。 自動リンクトラッキングでは、リンク名とリンク URL の両方がキャプチャされます。 Analytics レポートでは、リンク名がリンク URL よりも優先されます。 リンク URL を報告する場合は、リンク名を削除する必要があります。 次の例は、ダウンロードリンクのリンク名を削除する `onBeforeEventSend` 関数を示しています。

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

