---
title: clickCollection
description: クリックコレクション設定を微調整します。
exl-id: 5a128b4a-4727-4415-87b4-4ae87a7e1750
source-git-commit: 46c8748e9ab972705b8283c174c285e571acb2ed
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 0%

---

# `clickCollection`

`clickCollection` オブジェクトには、自動的に収集されたリンクデータの制御に役立つ変数がいくつか含まれています。 これらの変数は、データ収集にリンクタイプを含める場合や除外する場合に使用します。 Web SDK バージョン 2.25.0 以降でサポートされます。

この変数には、次のすべてが必要です。

* [`clickCollectionEnabled`](clickcollectionenabled.md) を有効にする必要があります。
* `clickCollection.filterClickDetails` を使用する場合、非推奨のメソッド [`onBeforeLinkClickSend`](onbeforelinkclicksend.md) は空である必要があります。
* イベントペイロードには、訪問者の訪問のある時点での `xdm.web.webPageDetails.name` の値を含める必要があります。

実装が上記の要件をすべて満たしていない場合、このオブジェクトはなにもしません。

`clickCollection` オブジェクトでは、次のプロパティを使用できます。

| プロパティ | タイプ | 説明 |
| --- | --- | --- |
| **`internalLinkEnabled`** | `boolean` | 現在のドメイン内のリンクを自動的に追跡するかどうかを決定します。 例えば、`https://example.com/index.html` から `https://example.com/product.html` は内部リンクと見なされます。 |
| **`downloadLinkEnabled`** | `boolean` | ライブラリが [`downloadLinkQualifier`](downloadlinkqualifier.md) プロパティに基づいてダウンロードと判断されるリンクを追跡するかどうかを決定します。 |
| **`externalLinkEnabled`** | `boolean` | 外部ドメインへのリンクを自動的に追跡するかどうかを決定します。 例えば、`https://example.com` から `https://example.net` は外部リンクと見なされます。 |
| **`eventGroupingEnabled`** | `boolean` | ライブラリが次の「ページビュー」イベントを待機してリンクトラッキングデータを送信するかどうかを決定します。 ライブラリは、次の要素がペイロードに含まれる場合、イベントを「ページビュー」と見なします。<ul><li>`xdm.web.webPageDetails.name` に文字列値が含まれています</li><li>`xdm.web.webPageDetails.pageViews.value` が `0` より大きい</li></ul>「ページビュー」イベントが読み込まれると、ライブラリは保存されたリンクトラッキングデータを、そのイベント内の残りのデータと組み合わせます。 このオプションを有効にすると、Adobeに送信されるイベントの合計数が減ります。 `internalLinkEnabled` が無効になっている場合、この変数は何も行いません。 |
| **`sessionStorageEnabled`** | `boolean` | リンクトラッキングデータをローカル変数ではなく、セッションストレージに保存するかどうかを決定します。 `internalLinkEnabled` または `eventGroupingEnabled` が無効になっている場合、この変数は何も行いません。<br>Adobeでは、シングルページアプリケーション以外で `eventGroupingEnabled` を使用する場合に、この変数を有効にすることを強くお勧めします。 `eventGroupingEnabled` が無効な状態で `sessionStorageEnabled` が有効になっている場合、新しいページをクリックすると、セッションストレージに保持されないので、リンクトラッキングデータが失われます。 単一ページアプリケーションは通常、新しいページに移動しないので、SPA ページにはセッションストレージは必要ありません。 |
| **`filterClickDetails`** | `function` | 収集するリンクトラッキングデータを完全に制御できるコールバック関数。 このコールバック関数を使用して、リンクトラッキングデータの送信を変更、難読化または中止できます。 このコールバックは、リンク内の個人を特定できる情報など、特定の情報を省略する場合に役立ちます。 |

[`configure`](overview.md) コマンドでこのオブジェクトを設定しない場合、このオブジェクトの既定の設定は [`clickCollectionEnabled`](clickcollectionenabled.md) の値によって異なります。

* `internalLinkEnabled`：次に一致 `clickCollectionEnabled` ます：
* `downloadLinkEnabled`：次に一致 `clickCollectionEnabled` ます：
* `externalLinkEnabled`：次に一致 `clickCollectionEnabled` ます：
* `eventGroupingEnabled`：デフォルトは `false` です。明示的に有効にする必要があります
* `sessionStorageEnabled`：デフォルトは `false` です。明示的に有効にする必要があります
* `filterClickDetails`：関数が含まれていません。明示的に登録する必要があります。

>[!TIP]
>
>Adobeでは、`eventGroupingEnabled` を有効にすると契約上の使用にカウントされるイベントの数が減るので、`internalLinkEnabled` を有効にすることをお勧めします。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: true,
  clickCollection: {
    internalLinkEnabled: true,
    downloadLinkEnabled: true,
    externalLinkEnabled: true,
    eventGroupingEnabled: true,
    sessionStorageEnabled: true,
    filterClickDetails: function(content) {
      // If the link is a clickable telephone number, anonymize it
      if(content.linkUrl?.includes("tel:")) {
        content.linkName = content.linkUrl = "Phone number";
      }
      // If the link is an email address, anonymize it
      if(content.linkUrl?.includes("mailto:")) {
        content.linkName = content.linkUrl = "Email address";
      }
    }
  }
});
```

## Web SDK タグ拡張機能用にクリックコレクションを設定する

これらの設定は、web SDK タグ拡張機能で [Data collection configuration settings](/help/tags/extensions/client/web-sdk/configure/data-collection.md) を使用して設定できます。
