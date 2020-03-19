---
title: パーソナライズされたコンテンツのレンダリング
seo-title: Adobe Experience Platform Web SDKのパーソナライズされたコンテンツのレンダリング
description: エクスペリエンスプラットフォームWeb SDKを使用してパーソナライズされたコンテンツをレンダリングする方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKを使用してパーソナライズされたコンテンツをレンダリングする方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）パーソナライズされたコンテンツのレンダリング

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

SDKは、イベントをサーバーに送信し、イベントのオプションとしてに設定すると、パーソナライズされたコン `viewStart` テンツを `true` 自動的にレンダリングします。

```javascript
alloy("event", {
  "viewStart": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

パーソナライズされたコンテンツのレンダリングは非同期的なので、特定のコンテンツがページの一部である場合を想定しないでください。

## ちらつきの管理

パーソナライゼーションコンテンツをレンダリングする場合、SDKはちらつきがないことを確認する必要があります。 ちらつき(FOOC(Flash of Original Content)とも呼ばれます)は、テストやパーソナライゼーションの際に代替コンテンツが表示される前に、元のコンテンツが短く表示される場合です。 SDKは、パーソナライゼーションコンテンツが正常にレンダリングされるまで、これらの要素が非表示になるように、ページの要素にCSSスタイルを適用しようとします。

ちらつき管理機能には、いくつかの段階があります。

1. プリヒード
1. 前処理中
1. レンダリング

### プリヒード

前の非表示段階では、SDKは設定オプションを使用し `prehidingStyle` てHTMLスタイルタグを作成し、DOMに追加して、ページの大きな部分が非表示になるようにします。 ページのどの部分がパーソナライズされるかが不明な場合は、をに設定することをお勧め `prehidingStyle` しま `body { opacity: 0 !important }`す。 これにより、ページ全体が非表示になります。 ただし、これは、LighthouseやWebページテストなどのツールから報告されるページレンダリングパフォーマンスの低下の原因となります。 最高のページレンダリングパフォーマンスを得るには、パーソナライズするペ `prehidingStyle` ージの部分を含むコンテナ要素のリストを設定することをお勧めします。

次のようなHTMLページがあり、コンテナ要素とコンテナ要素のみがパーソナラ `bar` イズされ `bazz` ることがわかっている場合：

```html
<html>
  <head>
  </head>
  <body>
    <div id="foo">
      Foo foo foo
    </div>

    <div id="bar">
      Bar bar bar
    </div>

    <div id="bazz">
      Bazz bazz bazz
    </div>
  </body>
</html>
```

次に、 `prehidingStyle` をのような値に設定します `#bar, #bazz { opacity: 0 !important }`。

### 前処理中

前処理段階は、SDKがパーソナライズされたコンテンツをサーバーから受け取ると開始されます。 この段階では応答が事前に処理され、パーソナライズされたコンテンツを含む必要のある要素が非表示になっていることを確認します。 これらの要素を非表示にすると、設定オプションに基づいて作成されたHTMLスタイルタグが削除され、HTMLの本文または非表示のコンテナ要素が `prehidingStyle` 表示されます。

### レンダリング

すべてのパーソナライゼーションコンテンツが正常にレンダリングされた後、またはエラーが発生した場合は、すべての非表示のエレメントが表示され、SDKによって非表示にされた非表示のエレメントがページ上にないことを確認します。

## SDKが非同期で読み込まれる場合のちらつきの管理

最高のページレンダリングパフォーマンスを得るために、常にSDKを非同期で読み込むことをお勧めします。 ただし、これは、パーソナライズされたコンテンツのレンダリングに何らかの影響を与えます。 SDKが非同期で読み込まれる場合は、プリヒードスニペットを使用する必要があります。 HTMLページのSDKの前に、事前非表示のスニペットを追加する必要があります。 ボディ全体を非表示にするスニペットの例を次に示します。

```html
<script>
  !function(e,a,n,t){
    if (a) return;
    var i=e.head;if(i){
    var o=e.createElement("style");
    o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
    setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
    (document, document.location.href.indexOf("mboxEdit") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

HTMLの本文またはコンテナ要素が長期間非表示にならないようにするために、プリヒードスニペットではタイマーを使用し、ミリ秒後にデフォルトでスニペットを削除 `3000` します。 ミリ `3000` 秒は最大待機時間です。 サーバーからの応答が受信され、処理が早い場合は、可能な限り早く前のHTMLスタイルタグが削除されます。
