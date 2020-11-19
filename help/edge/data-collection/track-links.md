---
title: Adobe Analyticsとのリンクトラッキング
seo-title: Adobe Experience PlatformWeb SDKを使用したAdobe Analyticsへのリンクトラッキング
description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
seo-description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web Interaction;page views;link tracking;links;track links;clickCollection;click collection;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---


# リンクの追跡

リンクは手動で設定することも、 [自動的に追跡することもできます](#automaticLinkTracking)。 手動トラッキングは、スキーマの `web.webInteraction` 部分に詳細を追加することで行います。 次の3つの必須変数があります。

* `web.webInteraction.name`
* `web.webInteraction.type`
* `web.webInteraction.linkClicks.value`

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
  }
});
```

リンクタイプは、次の3つの値のいずれかになります。

* **`other`:** カスタムリンク
* **`download`:** ダウンロードリンク
* **`exit`:** 離脱リンク

## 自動リンクトラッキング {#automaticLinkTracking}

デフォルトでは、Web SDKは、該当するリンクタグのクリックをキャプチャ、ラベル付けおよび記録します。 クリック数は、ドキュメントに接続された [キャプチャ](https://www.w3.org/TR/uievents/#capture-phase) ・クリックイベント・リスナーを使用してキャプチャされます。

自動リンクトラッキングは、Web SDKを [設定することで無効にできます](../fundamentals/configuring-the-sdk.md#clickCollectionEnabled) 。

```javascript
clickCollectionEnabled: false
```

### どのタグがリンクトラッキングに適しているか。{#qualifyingLinks}

自動リンクトラッキングは、アンカー `A` と `AREA` タグに対して行われます。 ただし、これらのタグにハン `onclick` ドラーがアタッチされている場合、リンクトラッキングでは考慮されません。

### リンクのラベルはどのように付けられますか。{#labelingLinks}

アンカータグにdownload属性が含まれている場合、またはリンクが人気のあるファイル拡張子で終わる場合、リンクはダウンロードリンクとしてラベル付けされます。 ダウンロードリンク修飾子は、次の正規式を使用して [設定できます](../fundamentals/configuring-the-sdk.md) 。

```javascript
downloadLinkQualifier: "\\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
```

リンクターゲットドメインが現在のドメインと異なる場合、リンクは離脱リンクとしてラベル付けされ `window.location.hostname`ます。

ダウンロードリンクまたは離脱リンクと見なされないリンクは、「その他」とラベル付けされます。
