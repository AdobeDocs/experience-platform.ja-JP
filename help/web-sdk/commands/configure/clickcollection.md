---
title: clickCollection
description: クリックコレクション設定を微調整します。
source-git-commit: 660d4e72bd93ca65001092520539a249eae23bfc
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# `clickCollection`

この `clickCollection` オブジェクトには、自動的に収集されたリンクデータの制御に役立つ変数がいくつか含まれています。 これらの変数は、データ収集にリンクタイプを含める場合や除外する場合に使用します。

それには次が必要です [`clickCollectionEnabled`](clickcollectionenabled.md) を有効にします。

Web SDK 2.25.0 以降でサポートされています。

では次の変数を使用できます。 `clickCollection` オブジェクト :

* **`clickCollection.internalLinkEnabled`**：現在のドメイン内のリンクを自動的に追跡するかどうかを決定するブール値です。 例： `https://example.com/index.html` 対象： `https://example.com/product.html`.
* **`clickCollection.downloadLinkEnabled`**：に基づいてダウンロードと認定されるリンクをライブラリが追跡するかどうかを決定するブール値 [`downloadLinkQualifier`](downloadlinkqualifier.md) プロパティ。
* **`clickCollection.externalLinkEnabled`**：外部ドメインへのリンクを自動的に追跡するかどうかを決定するブール値です。 例： `https://example.com` 対象： `https://example.net`.
* **`clickCollection.eventGroupingEnabled`**：リンクトラッキングデータを送信するために、次のページまでライブラリが待機するかどうかを決定するブール値です。 次のページが読み込まれたら、リンクトラッキングデータをページ読み込みイベントと組み合わせます。 このオプションを有効にすると、Adobeに送信されるイベントの数が減ります。 次の場合 `internalLinkEnabled` が無効になっている場合は、この変数は何も行いません。
* **`clickCollection.sessionStorageEnabled`**：リンクトラッキングデータをローカル変数ではなくセッションストレージに保存するかどうかを決定するブール値。 次の場合 `internalLinkEnabled` または `eventGroupingEnabled` が無効になっている場合は、この変数は何も行いません。

  Adobeでは、を使用する場合にこの変数を有効にすることを強くお勧めします `eventGroupingEnabled`. 次の場合 `eventGroupingEnabled` は次の場合に有効になります `sessionStorageEnabled` が無効になっている場合、セッションストレージに保持されないので、新しいページをクリックすると、リンクトラッキングデータが失われます。 無効にすることは可能ですが `sessionStorageEnabled` 単一ページアプリケーションでは、非SPA ページに対しては理想的ではありません。
* **`filterClickDetails`**：収集するリンクトラッキングデータを完全に制御できるコールバック関数。 このコールバック関数を使用して、リンクトラッキングデータの送信を変更、難読化または中止できます。 このコールバックは、リンク内の個人を特定できる情報など、特定の情報を省略する場合に役立ちます。

## Web SDK タグ拡張機能を使用したコレクション設定のクリック

「」を選択します **[!UICONTROL クリックデータ収集の有効化]** 次の場合にチェックボックス [タグ拡張機能の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). このチェックボックスをオンにすると、クリックコレクションに関する次のオプションが表示されます。

* [!UICONTROL 内部リンク]
   * [!UICONTROL イベント グループ化を有効にする]
   * [!UICONTROL セッションストレージの有効化]
* [!UICONTROL 外部リンク]
* [!UICONTROL ダウンロードリンク]
* [!UICONTROL フィルタークリックのプロパティ]

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL 設定]** 日 [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. にスクロール ダウンします。 [!UICONTROL データ収集] 「」セクションで、「」チェックボックスを選択します **[!UICONTROL クリックデータ収集の有効化]**.
1. 目的のクリックコレクション設定を選択します。
1. クリック **[!UICONTROL 保存]**&#x200B;を作成してから、変更を公開します。

この [!UICONTROL フィルタークリックのプロパティ] コールバックをクリックすると、目的のコードを挿入できるカスタムコードエディターが開きます。 コードエディター内で、次の変数にアクセスできます。

* **`content.clickedElement`**：クリックされた DOM 要素。
* **`content.pageName`**：クリック時のページ名。
* **`content.linkName`**：クリックされたリンクの名前。
* **`content.linkRegion`**：クリックされたリンクの領域。
* **`content.linkType`**：リンクのタイプ（離脱、ダウンロードまたはその他）。
* **`content.linkURL`**：クリックされたリンクの宛先 URL。
* **`return true`**：現在の変数値でコールバックを直ちに終了します。
* **`return false`**：直ちにコールバックを終了し、データの収集を中止します。

の外部で定義された変数 `content` を使用できますが、Adobeに送信されるペイロードには含まれません。

## Web SDK JavaScript ライブラリを使用したコレクション設定のクリック

内で必要な変数を設定します。 `clickCollection` を実行しているときのオブジェクト [`configure`](overview.md) コマンド。 設定されていない場合、このオブジェクトのデフォルト設定は、 [`clickCollectionEnabled`](clickcollectionenabled.md).

* `internalLinkEnabled`：次に一致 `clickCollectionEnabled`
* `downloadLinkEnabled`：次に一致 `clickCollectionEnabled`
* `externalLinkEnabled`：次に一致 `clickCollectionEnabled`
* `eventGroupingEnabled`：デフォルトは `false`。明示的に有効にする必要があります
* `sessionStorageEnabled`：デフォルトは `false`。明示的に有効にする必要があります
* `filterClickDetails`：関数が含まれていません。明示的に登録する必要があります

>[!TIP]
>Adobeでは、を有効にすることをお勧めします `eventGroupingEnabled`を使用できます。これは、契約上の使用状況にカウントされるイベントの数を減らすのに役立ちます。

```js
alloy("configure", {
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
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
