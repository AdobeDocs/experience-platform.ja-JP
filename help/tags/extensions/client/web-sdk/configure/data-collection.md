---
title: データ収集の設定
description: Web SDK タグ拡張機能でデータ収集を設定します。
source-git-commit: 46c8748e9ab972705b8283c174c285e571acb2ed
workflow-type: tm+mt
source-wordcount: '692'
ht-degree: 1%

---

# データ収集の設定

この設定セクションでは、拡張機能全体でのデータの収集方法を指定できます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Data collection]** セクションまで下にスクロールします。

![&#x200B; タグ UI の web SDK タグ拡張機能のデータ収集設定を示す画像。](../assets/web-sdk-ext-collection.png)

次のオプションがあります。

## [!UICONTROL On before event send callback]

Adobeに送信されたペイロードを評価して変更するコールバック関数。 コードエディター内で、次の変数にアクセスできます。

* **`content.xdm`**：イベントの XDM ペイロード。
* **`content.data`**：イベントのデータオブジェクトペイロード。
* **`return true`**：直ちにコールバックを終了し、`content` オブジェクト内の現在の値を使用してデータをAdobeに送信します。
* **`return false`**：直ちにコールバックを終了し、Adobeへのデータの送信を中止します。

`content` 外で定義された変数は使用できますが、Adobeに送信されるペイロードには含まれません。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止します。 **データはAdobeに送信されません。**

```js
// Use nullish coalescing assignments to add objects if they don't yet exist
content.xdm.commerce ??= {};
content.xdm.commerce.order ??= {};

// Then add the purchase ID
content.xdm.commerce.order.purchaseID = "12345";

// Use optional chaining to prevent undefined errors when setting tracking code to lower case
if(content.xdm.marketing?.trackingCode) content.xdm.marketing.trackingCode = content.xdm.marketing.trackingCode.toLowerCase();

// Delete operating system version
if(content.xdm.environment) delete content.xdm.environment.operatingSystemVersion;

// Immediately end onBeforeEventSend logic and send the data to Adobe for this event type
if (content.xdm.eventType === "web.webInteraction.linkClicks") {
  return true;
}

// Cancel sending data if it is a known bot
if (myBotDetector.isABot()) {
  return false;
}
```

>[!TIP]
>ページの最初のイベントで `false` を返さないようにします。 最初のイベントで `false` を返すと、パーソナライゼーションに悪影響を与える可能性があります。

このコールバックは、JavaScript ライブラリの [`onBeforeEventSend`](/help/collection/js/commands/configure/onbeforeeventsend.md) と同等のタグです。

## [!UICONTROL Collect internal link clicks]

サイトまたはプロパティ内部のリンクトラッキングデータの収集を有効にするチェックボックス。 このチェックボックスは、JavaScript ライブラリの [`clickCollection.internalLinkEnabled`](/help/collection/js/commands/configure/clickcollection.md) と同等のタグです。 このチェックボックスを有効にすると、イベントのグループ化オプションが表示されます。

* **[!UICONTROL No event grouping]**：リンクトラッキングデータは、別のイベントでAdobeに送信されます。 別々のイベントで送信されるリンククリック数は、Adobe Experience Platformに送信されるデータの契約上の使用を増やす可能性があります。
* **[!UICONTROL Event grouping using session storage]**：次の「ページビュー」イベントまで、リンクトラッキングデータをセッションストレージに保存します。 「ページビュー」と見なされる次のイベントで、保存されたリンクトラッキングデータが「ページビュー」イベントペイロードと結合されます。 Adobeでは、内部リンクをトラッキングする場合にこの設定を有効にすることをお勧めします。
* **[!UICONTROL Event grouping using local object]**：次の「ページビュー」イベントまで、リンクトラッキングデータをローカルオブジェクトに保存します。 訪問者が新しいブラウザーページに移動すると、リンクトラッキングデータが失われます。 この設定は、単一ページアプリケーションのコンテキストで最も役立ちます。

次の要素がペイロードに含まれる場合、タグライブラリは特定のイベントを「ページビュー」と見なします。

* `xdm.web.webPageDetails.name` に文字列値が含まれています
* `xdm.web.webPageDetails.pageViews.value` が `0` より大きい

## [!UICONTROL Collect external link clicks]

外部リンクの収集を有効にするチェックボックス。 このチェックボックスは、JavaScript ライブラリの [`clickCollection.externalLinkEnabled`](/help/collection/js/commands/configure/clickcollection.md) と同等のタグです。

## [!UICONTROL Collect download link clicks]

ダウンロードリンクの収集を有効にするチェックボックス。 このチェックボックスは、JavaScript ライブラリの [`clickCollection.downloadLinkEnabled`](/help/collection/js/commands/configure/clickcollection.md) と同等のタグです。

## [!UICONTROL Download link qualifier]

リンク URL をダウンロードリンクとして認定する正規表現。 この文字列は、JavaScript ライブラリの [`downloadLinkQualifier`](/help/collection/js/commands/configure/downloadlinkqualifier.md) と同等のタグです。

## [!UICONTROL Filter click properties]

コレクションの前にクリック関連のプロパティを評価および変更するコールバック関数。 この関数は、[!UICONTROL On before event send callback] の前に実行され、JavaScript ライブラリの [`clickCollection.filterClickDetails`](/help/collection/js/commands/configure/clickcollection.md) と同等のタグです。 コードエディター内で、次の変数にアクセスできます。

* **`content.clickedElement`**：クリックされた DOM 要素。
* **`content.pageName`**：クリックが発生した際のページ名。
* **`content.linkName`**: クリックされたリンクの名前。
* **`content.linkRegion`**：クリックされたリンクの領域。
* **`content.linkType`**: リンクのタイプ（離脱、ダウンロードまたはその他）。
* **`content.linkURL`**：クリックされたリンクの宛先 URL。
* **`return true`**：現在の変数値でコールバックを直ちに終了します。
* **`return false`**：直ちにコールバックを終了し、データの収集を中止します。
* `content` 外で定義された変数は使用できますが、Adobeに送信されるペイロードには含まれません。

>[!TIP]
>
>**[!UICONTROL On before link click send]** フィールドは非推奨（廃止予定）のコールバックで、既に設定されているプロパティにのみ表示されます。 これは、JavaScript ライブラリの [`onBeforeLinkClickSend`](/help/collection/js/commands/configure/onbeforelinkclicksend.md) と同等のタグです。 **[!UICONTROL Filter click properties]** コールバックを使用してクリックデータをフィルタリングまたは調整するか、**[!UICONTROL On before event send callback]** を使用してAdobeに送信されるペイロード全体をフィルタリングまたは調整します。 **[!UICONTROL Filter click properties]** コールバックと **[!UICONTROL On before link click send]** コールバックの両方が設定されている場合は、**[!UICONTROL Filter click properties]** コールバックのみが実行されます。

## コンテキスト設定

特定の XDM フィールドに値を入力する訪問者情報を自動的に収集します。 **[!UICONTROL All default context information]** または **[!UICONTROL Specific context information]** を選択できます。 これは、JavaScript ライブラリの [`context`](/help/collection/js/commands/configure/context.md) と同等のタグです。

* **[!UICONTROL Web]**：現在のページに関する情報を収集します。
* **[!UICONTROL Device]**: ユーザーのデバイスに関する情報を収集します。
* **[!UICONTROL Environment]**: ユーザーのブラウザーに関する情報を収集します。
* **[!UICONTROL Place context]**: ユーザーの場所に関する情報を収集します。
* **[!UICONTROL High entropy user-agent hints]**: ユーザーのデバイスに関する詳細情報を収集します。
