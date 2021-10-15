---
title: Adobe Experience Platform Web SDK を使用した、パーソナライズされたエクスペリエンスのちらつき制御
description: Adobe Experience Platform Web SDK を使用して、ユーザーエクスペリエンスのちらつきを管理する方法について説明します。
keywords: target;flicker;prehidingStyle；非同期；非同期；
exl-id: f4b59109-df7c-471b-9bd6-7082e00c293b
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 76%

---

# フリッカーの管理

パーソナライゼーションコンテンツをレンダリングする場合、SDK はちらつきがないことを確認する必要があります。ちらつきは FOOC( オリジナルコンテンツのFlash) とも呼ばれ、テストやパーソナライゼーションの際に代替コンテンツが表示される前に、元のコンテンツが短く表示されます。 SDK は、パーソナライゼーションコンテンツが正常にレンダリングされるまでこれらの要素が非表示になるよう、ページの要素に CSS スタイルを適用しよう試みます。

ちらつき管理機能には、いくつかのフェーズがあります。

1. 事前非表示
1. 前処理
1. レンダリング

## 事前非表示

事前非表示フェーズでは、SDK は `prehidingStyle` 設定オプションを使用して HTML スタイルタグを作成し、DOM に追加して、ページの大きな部分が非表示になるようにします。ページのどの部分がパーソナライズされるかが不明な場合は、`prehidingStyle` を `body { opacity: 0 !important }` に設定することをお勧めします。これにより、ページ全体が非表示になります。ただし、これは、Lighthouse や Web ページテストなどのツールで報告されるページレンダリングパフォーマンスの低下につながるという欠点があります。 最高のページレンダリングパフォーマンスを得るには、`prehidingStyle` をコンテナ要素（パーソナライズするページの部分を含む）のリストに設定することをお勧めします。

次のようなHTMLページがあり、`bar` と `bazz` のコンテナ要素のみがパーソナライズされることがわかっている場合：

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

`prehidingStyle` を `#bar, #bazz { opacity: 0 !important }` などの値に設定します。

## 前処理

前処理フェーズは、SDK がパーソナライズされたコンテンツをサーバーから受け取ると開始されます。 このフェーズでは応答が前処理され、パーソナライズされたコンテンツを含める必要がある要素が非表示になっていることを確認します。これらの要素を非表示にすると、`prehidingStyle` 設定オプションに基づいて作成された HTML スタイルタグが削除され、HTML の本文または非表示のコンテナ要素が表示されます。

## レンダリング

すべてのパーソナライゼーションコンテンツが正常にレンダリングされた後、またはエラーが発生した場合は、すべての非表示の要素が表示され、SDK によって非表示にされた非表示の要素がページ上にないことを確認します。

## SDK が非同期で読み込まれる場合のちらつきの管理

最高のページレンダリングパフォーマンスを得るため、常に SDK を非同期で読み込むことをお勧めします。ただし、これにより、パーソナライズされたコンテンツのレンダリングに何らかの影響が出ます。SDK が非同期で読み込まれる場合は、非表示のスニペットを使用する必要があります。HTML ページで SDK の前に、事前非表示のスニペットを追加する必要があります。本文全体を非表示にするスニペットの例を次に示します。

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

HTML の本文またはコンテナ要素が長期間非表示にならないようにするために、事前非表示スニペットでは、`3000` ミリ秒後にデフォルトでスニペットを削除するタイマーを使用します。`3000` ミリ秒は最大待機時間です。サーバーからの応答が受信され、短時間で処理された場合は、可能な限り早く事前非表示 HTML スタイルタグが削除されます。
