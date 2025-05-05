---
title: clickCollection
description: クリックコレクション設定を微調整します。
exl-id: 5a128b4a-4727-4415-87b4-4ae87a7e1750
source-git-commit: d3be2a9e75514023a7732a1c3460f8695ef02e68
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# `clickCollection`

`clickCollection` オブジェクトには、自動的に収集されたリンクデータの制御に役立つ変数がいくつか含まれています。 これらの変数は、データ収集にリンクタイプを含める場合や除外する場合に使用します。

[`clickCollectionEnabled`](clickcollectionenabled.md) を有効にする必要があります。

Web SDK 2.25.0 以降でサポートされています。

`clickCollection` オブジェクトでは、次の変数を使用できます。

* **`clickCollection.internalLinkEnabled`**：現在のドメイン内のリンクを自動的に追跡するかどうかを決定するブール値です。 例えば、`https://example.com/index.html` を `https://example.com/product.html` に設定します。
* **`clickCollection.downloadLinkEnabled`**: [`downloadLinkQualifier`](downloadlinkqualifier.md) プロパティに基づいてダウンロードとして認定されるリンクをライブラリが追跡するかどうかを決定するブール値。
* **`clickCollection.externalLinkEnabled`**：外部ドメインへのリンクを自動的に追跡するかどうかを決定するブール値です。 例えば、`https://example.com` を `https://example.net` に設定します。
* **`clickCollection.eventGroupingEnabled`**: リンクトラッキングデータを送信するために、次のページまでライブラリが待機するかどうかを決定するブール値です。 次のページが読み込まれたら、リンクトラッキングデータをページ読み込みイベントと組み合わせます。 このオプションを有効にすると、Adobeに送信されるイベントの数が減ります。 `internalLinkEnabled` が無効になっている場合、この変数は何も行いません。
* **`clickCollection.sessionStorageEnabled`**: リンクトラッキングデータをローカル変数ではなくセッションストレージに保存するかどうかを決定するブール値。 `internalLinkEnabled` または `eventGroupingEnabled` が無効になっている場合、この変数は何も行いません。

  Adobeでは、シングルページアプリケーション以外で `eventGroupingEnabled` を使用する場合に、この変数を有効にすることを強くお勧めします。 `sessionStorageEnabled` が無効な状態で `eventGroupingEnabled` が有効になっている場合、新しいページをクリックすると、セッションストレージに保持されないので、リンクトラッキングデータが失われます。 シングルページアプリケーションは通常、新しいページに移動しないので、SPA ページにセッションストレージは必要ありません。
* **`filterClickDetails`**：収集するリンクトラッキングデータを完全に制御できるコールバック関数。 このコールバック関数を使用して、リンクトラッキングデータの送信を変更、難読化または中止できます。 このコールバックは、リンク内の個人を特定できる情報など、特定の情報を省略する場合に役立ちます。

## Web SDK タグ拡張機能を使用したコレクション設定のクリック

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) の際には、次のいずれかのオプションを選択します。

* [!UICONTROL &#x200B; 内部リンクの収集 &#x200B;]
   * [!UICONTROL &#x200B; イベントのグループ化オプション &#x200B;]:
      * [!UICONTROL &#x200B; イベントのグループ化なし &#x200B;]
      * [!UICONTROL &#x200B; セッションストレージを使用したイベントのグループ化 &#x200B;]
      * [!UICONTROL &#x200B; ローカルオブジェクトを使用したイベントのグループ化 &#x200B;]
* [!UICONTROL &#x200B; 外部リンクの収集 &#x200B;]
* [!UICONTROL &#x200B; ダウンロードリンクの収集 &#x200B;]
* [!UICONTROL &#x200B; フィルタークリックのプロパティ &#x200B;]

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL &#x200B; データ収集 &#x200B;]」セクションまでスクロールし、目的のクリックコレクション設定を選択します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

[!UICONTROL &#x200B; フィルタークリックのプロパティ &#x200B;] コールバックにより、目的のコードを挿入できるカスタムコードエディターが開きます。 コードエディター内で、次の変数にアクセスできます。

* **`content.clickedElement`**：クリックされた DOM 要素。
* **`content.pageName`**：クリックが発生した際のページ名。
* **`content.linkName`**: クリックされたリンクの名前。
* **`content.linkRegion`**：クリックされたリンクの領域。
* **`content.linkType`**: リンクのタイプ（離脱、ダウンロードまたはその他）。
* **`content.linkURL`**：クリックされたリンクの宛先 URL。
* **`return true`**：現在の変数値でコールバックを直ちに終了します。
* **`return false`**：直ちにコールバックを終了し、データの収集を中止します。

`content` 外で定義された変数は使用できますが、Adobeに送信されるペイロードには含まれません。

## Web SDK JavaScript ライブラリを使用したコレクション設定のクリック

[`configure`](overview.md) コマンドを実行するときに、`clickCollection` オブジェクト内に必要な変数を設定します。 設定されていない場合、このオブジェクトのデフォルト設定は [`clickCollectionEnabled`](clickcollectionenabled.md) の値によって異なります。

* `internalLinkEnabled`：次に一致 `clickCollectionEnabled` ます：
* `downloadLinkEnabled`：次に一致 `clickCollectionEnabled` ます：
* `externalLinkEnabled`：次に一致 `clickCollectionEnabled` ます：
* `eventGroupingEnabled`：デフォルトは `false` です。明示的に有効にする必要があります
* `sessionStorageEnabled`：デフォルトは `false` です。明示的に有効にする必要があります
* `filterClickDetails`：関数が含まれていません。明示的に登録する必要があります。

>[!TIP]
>Adobeでは、`internalLinkEnabled` が有効な場合に `eventGroupingEnabled` を有効にすることをお勧めします。これにより、契約上の使用にカウントされるイベントの数が減るからです。

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
