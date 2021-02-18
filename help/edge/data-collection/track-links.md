---
title: Adobe Experience PlatformWeb SDKを使用したリンクの追跡
description: Experience PlatformWeb SDKを使用してリンクデータをAdobe Analyticsに送信する方法を学びます
keywords: adobe analytics;analytics;sendEvent;s.t();s.tl();webPageDetails;pageViews;webInteraction;web Interaction;page表示;link tracking;links;track links;clickCollection;click collection;
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---


# リンクの追跡

リンクは手動で設定するか、[自動的に](#automaticLinkTracking)追跡することができます。 手動トラッキングは、スキーマの`web.webInteraction`部分に詳細を追加することで行います。 次の3つの必須変数があります。

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

## 自動リンクトラッキング{#automaticLinkTracking}

デフォルトでは、Web SDKは、該当するリンクタグのクリックをキャプチャ、ラベル付けおよび記録します。 クリック数は、ドキュメントに接続されている[キャプチャ](https://www.w3.org/TR/uievents/#capture-phase)クリックイベントリスナーを使用してキャプチャされます。

自動リンクトラッキングは、Web SDKを[設定](../fundamentals/configuring-the-sdk.md#clickCollectionEnabled)することで無効にできます。

```javascript
clickCollectionEnabled: false
```

### どのタグがリンクトラッキングに適合しますか？{#qualifyingLinks}

アンカー`A`タグと`AREA`タグに対して、自動リンクトラッキングが実行されます。 ただし、これらのタグに`onclick`ハンドラーがアタッチされている場合、リンクトラッキングは考慮されません。

### リンクのラベルは？{#labelingLinks}

アンカータグにdownload属性が含まれている場合、またはリンクが人気のあるファイル拡張子で終わる場合、リンクはダウンロードリンクとしてラベル付けされます。 ダウンロードリンク修飾子は、正規式を使用して[設定](../fundamentals/configuring-the-sdk.md)できます。

```javascript
downloadLinkQualifier: "\\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
```

リンクターゲットドメインが現在の`window.location.hostname`と異なる場合、リンクは離脱リンクとしてラベル付けされます。

ダウンロードリンクまたは離脱リンクと見なされないリンクは、「その他」とラベル付けされます。
