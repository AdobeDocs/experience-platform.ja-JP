---
title: clickCollectionEnabled
description: リンククリックデータが自動的に収集されるかどうかを確認するように web SDKを設定する方法を説明します。
exl-id: e91b5bc6-8880-4884-87f9-60ec8787027e
source-git-commit: 364b9adc406f732ea5ba450730397c4ce1bf03cf
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# `clickCollectionEnabled`

`clickCollectionEnabled` プロパティは、Web SDKがリンクデータを自動的に収集するかどうかを指定するブール値です。 この変数を設定しない場合、デフォルト値は `true` になります。つまり、リンクトラッキングデータはデフォルトで自動的に収集されます。 このプロパティを `false` に設定すると、リンクデータを手動で追跡する場合に便利です。

`clickCollectionEnabled` が有効な場合、次の XDM 要素が自動的にデータを入力します。

* `xdm.web.webInteraction.name`
* `xdm.web.webInteraction.type`
* `xdm.web.webInteraction.URL`

このブール値を有効にすると、デフォルトで内部リンク、ダウンロードリンク、離脱リンクがすべて自動的に追跡されます。 自動リンクトラッキングをより詳細に制御する場合、Adobeでは [`clickCollection`](clickcollection.md) オブジェクトを使用することをお勧めします。

## 自動リンクトラッキングロジック

Web SDKは、`<a>` および `<area>` HTML要素に対するすべてのクリックを追跡します（`onClick` 属性がない場合）。 クリックは、ドキュメントに添付された [capture](https://www.w3.org/TR/uievents/#capture-phase) クリックイベントリスナーでキャプチャされます。 有効なリンクをクリックすると、次のロジックが順に実行されます。

1. リンクが [`downloadLinkQualifier`](downloadlinkqualifier.md) 内の値に基づいて条件に一致する場合、またはリンクに `download` HTML属性が含まれている場合、`xdm.web.webInteraction.type` は `"download"` に設定されます（`clickCollection.downloadLinkEnabled` が有効になっている場合）。
1. リンク ターゲット ドメインが現在の `window.location.hostname` と異なる場合、`xdm.web.webInteraction.type` は `"exit"` に設定されます（`clickCollection.exitLinkEnabled` が有効な場合）。
1. リンクが `"download"` または `"exit"` の対象として認定されない場合、`xdm.web.webInteraction.type` は `"other"` に設定されます。

いずれの場合も、`xdm.web.webInteraction.name` はリンクテキストラベルに設定され、`xdm.web.webInteraction.URL` はリンク先の URL に設定されます。 URL へのリンク名を設定する場合は、`filterClickDetails` オブジェクトで `clickCollection` コールバックを使用して、この XDM フィールドを上書きできます。

`clickCollectionEnabled` コマンドを実行するときは、`configure` のブール値を設定します。 Web SDKの設定時にこのプロパティを省略した場合、デフォルトは `true` になります。 `false` と `xdm.web.webInteraction.type` を手動で設定する場合は、この値を `xdm.web.webInteraction.value` に設定します。

```js
alloy(configure, {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: false
});
```

## オープンな [!DNL Shadow DOM] 要素のサポート

Web SDKは、**開いたシャドウ DOM** 要素内のリンクの自動クリックトラッキングをサポートしています。

最新の多くの Web サイトでは、[Web コンポーネント &#x200B;](https://developer.mozilla.org/en-US/docs/Web/Web_Components) を使用して、再利用可能でカプセル化されたユーザーインターフェイス要素を構築しています。 これらのコンポーネントでは、多くの場合、[**シャドウ DOM**](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) と呼ばれるテクノロジーを使用して、内部構造とスタイルをページの他の部分から分離します。

シャドウ DOM には次の 2 種類があります。

* **シャドウ DOM を開く：** ページ上で動作しているJavaScriptから内部構造にアクセスできます。 他のスクリプトで、コンポーネントのコンテンツを操作したり、検査したりすることができます。
* **クローズドシャドウ DOM:** 内部構造はコンポーネントの外側のJavaScriptに表示されず、トラッキングや操作のためにアクセスできません。

Web SDKは、メインドキュメント内のリンクの場合と同様に、`<a>` 開いたシャドウ DOM`<area>` 内の **要素と** 要素のクリック数を自動的に追跡します。 このトラッキングにより、開いた [!DNL Shadow DOM] を使用して web コンポーネント内のリンククリックが、分析データに確実に含まれます。 **閉じられたシャドウ DOM** 内のクリックは、その内部構造がコンポーネントの外部で動作するJavaScript コードに対して非表示になっているので、追跡されません。

## Web SDK タグ拡張機能のクリックコレクションを有効または無効にします

タグを使用してこれらのアクションを実行する方法については、Web SDK拡張機能のドキュメントの [&#x200B; データ収集設定 &#x200B;](/help/tags/extensions/client/web-sdk/configure/data-collection.md) を参照してください。
