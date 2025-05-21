---
title: clickCollectionEnabled
description: リンククリックデータが自動的に収集されるかどうかを確認するように web SDKを設定する方法を説明します。
exl-id: e91b5bc6-8880-4884-87f9-60ec8787027e
source-git-commit: fdb809ea86e91a98b45877c99c3e64d7c49d1cd5
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# `clickCollectionEnabled`

`clickCollectionEnabled` プロパティは、Web SDKがリンクデータを自動的に収集するかどうかを指定するブール値です。 この変数を設定しない場合、デフォルト値は `true` になります。つまり、リンクトラッキングデータはデフォルトで自動的に収集されます。 このプロパティを `false` に設定すると、リンクデータを手動で追跡する場合に便利です。

`clickCollectionEnabled` が有効な場合、次の XDM 要素が自動的にデータを入力します。

* `xdm.web.webInteraction.name`
* `xdm.web.webInteraction.type`
* `xdm.web.webInteraction.URL`

このブール値を有効にすると、デフォルトで内部リンク、ダウンロードリンク、離脱リンクがすべて自動的に追跡されます。 自動リンクトラッキングをより詳細に制御する場合、Adobeでは [`clickCollection`](clickcollection.md) オブジェクトを使用することをお勧めします。

## オープンな [!DNL Shadow DOM] 要素のサポート

Web SDKは、**開いたシャドウ DOM** 要素内のリンクの自動クリックトラッキングをサポートしています。

最新の多くの Web サイトでは、[Web コンポーネント ](https://developer.mozilla.org/en-US/docs/Web/Web_Components) を使用して、再利用可能でカプセル化されたユーザーインターフェイス要素を構築しています。 これらのコンポーネントでは、多くの場合、[**シャドウ DOM**](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) と呼ばれるテクノロジーを使用して、内部構造とスタイルをページの他の部分から分離します。

シャドウ DOM には次の 2 種類があります。

* **シャドウ DOM を開く：** ページ上で動作しているJavaScriptから内部構造にアクセスできます。 つまり、他のスクリプトでコンポーネントのコンテンツを操作したり、検査したりできるということです。
* **クローズドシャドウ DOM:** 内部構造はコンポーネントの外側のJavaScriptに表示されず、トラッキングや操作のためにアクセスできません。

Web SDKは、メインドキュメント内のリンクの場合と同様に、**開いたシャドウ DOM** 内の `<a>` 要素と `<area>` 要素のクリック数を自動的に追跡します。 これにより、開いている [!DNL Shadow DOM] を使用して web コンポーネント内でクリックされたリンクが、分析データに確実に含まれます。 **閉じられたシャドウ DOM** 内のクリックは、その内部構造がコンポーネントの外部で動作するJavaScript コードに対して非表示になっているので、追跡されません。

## 自動リンクトラッキングロジック

Web SDKは、`<a>` および `<area>` HTML要素に対するすべてのクリックを追跡します（`onClick` 属性がない場合）。 クリックは、ドキュメントに添付された [capture](https://www.w3.org/TR/uievents/#capture-phase) クリックイベントリスナーでキャプチャされます。 有効なリンクをクリックすると、次のロジックが順に実行されます。

1. リンクが [`downloadLinkQualifier`](downloadlinkqualifier.md) 内の値に基づいて条件に一致する場合、またはリンクに `download` HTML属性が含まれている場合、`xdm.web.webInteraction.type` は `"download"` に設定されます（`clickCollection.downloadLinkEnabled` が有効になっている場合）。
1. リンク ターゲット ドメインが現在の `window.location.hostname` と異なる場合、`xdm.web.webInteraction.type` は `"exit"` に設定されます（`clickCollection.exitLinkEnabled` が有効な場合）。
1. リンクが `"download"` または `"exit"` の対象として認定されない場合、`xdm.web.webInteraction.type` は `"other"` に設定されます。

いずれの場合も、`xdm.web.webInteraction.name` はリンクテキストラベルに設定され、`xdm.web.webInteraction.URL` はリンク先の URL に設定されます。 URL へのリンク名を設定する場合は、`clickCollection` オブジェクトで `filterClickDetails` コールバックを使用して、この XDM フィールドを上書きできます。

## Web SDK タグ拡張機能を使用した自動リンクトラッキングの有効化 {#tag-extension}

この変数は、タグ拡張機能によって自動的に管理されるので、明示的に設定する必要はありません。 [ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に次のいずれかを選択すると、該当するリンクトラッキングデータが収集されます。

* [!UICONTROL &#x200B; 内部リンククリック数の収集 &#x200B;]
* [!UICONTROL &#x200B; 外部リンククリック数の収集 &#x200B;]
* [!UICONTROL &#x200B; ダウンロードリンクのクリック数を収集 &#x200B;]

詳細は、[`clickCollection`](clickcollection.md) を参照してください。

## Web SDK JavaScript ライブラリを使用して自動リンクトラッキングを有効にします {#library}

`configure` コマンドを実行するときは、`clickCollectionEnabled` のブール値を設定します。 Web SDKの設定時にこのプロパティを省略した場合、デフォルトは `true` になります。 `xdm.web.webInteraction.type` と `xdm.web.webInteraction.value` を手動で設定する場合は、この値を `false` に設定します。

```js
alloy(configure, {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: false
});
```
