---
title: Adobe Experience Platform Web SDKを使用したパーソナライズされたエクスペリエンスのちらつきの管理
description: Adobe Experience Platform Web SDKを使用して、ユーザーエクスペリエンス上のちらつきを管理する方法を説明します。
exl-id: f4b59109-df7c-471b-9bd6-7082e00c293b
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 54%

---

# フリッカーの管理

パーソナライゼーションコンテンツをレンダリングする場合、SDK はちらつきがないことを確認する必要があります。ちらつき（FOOC （元のコンテンツのフラッシュ）とも呼ばれます）は、元のコンテンツが短時間表示された後で、テスト/パーソナライゼーション中に代替コンテンツが表示される場合です。 SDK は、パーソナライゼーションコンテンツが正常にレンダリングされるまでこれらの要素が非表示になるよう、ページの要素に CSS スタイルを適用しよう試みます。

ちらつきの制御方法は、Web SDKを同期または非同期のどちらでデプロイするかによって異なります。 デプロイ先の `<head>` タグまたはタグローダー `alloy.js` 確認します。 `async` タグに `<script>` 属性が存在する場合は、web SDKが非同期で読み込まれるかどうかを判断します。

```html
<!-- This tag loads synchronously -->
<script src="https://assets.adobedtm.com/[...]/launch-example.min.js"></script>

<!-- This tag loads asynchronously -->
<script src="https://assets.adobedtm.com/[...]/launch-example.min.js" async></script>

<!-- This library loads synchronously -->
<script src="https://cdn1.adoberesources.net/alloy/2.14.0/alloy.min.js"></script>

<!-- This library loads asynchronously -->
<script src="https://cdn1.adoberesources.net/alloy/2.14.0/alloy.min.js" async></script>
```

## 同期展開のフリッカーの管理

同期フリッカー管理は、次の 3 つの段階に分かれます。

1. 事前非表示
1. 前処理
1. レンダリング

**事前非表示フェーズ** では、SDKは [`prehidingStyle`](../../js/commands/configure/prehidingstyle.md) 設定プロパティを使用してHTML スタイルタグを作成し、それを DOM に追加して、ページの目的のセクションが非表示になっていることを確認します。 ページのどの部分がパーソナライズされるかが不明な場合は、`prehidingStyle` を `body { opacity: 0 !important }` に設定することをお勧めします。これにより、ページ全体が非表示になります。ただし、これには、Lighthouse、web ページテストなどのツールで報告されるページレンダリングパフォーマンスが低下する欠点があります。 最高のページレンダリングパフォーマンスを得るには、`prehidingStyle` をコンテナ要素（パーソナライズするページの部分を含む）のリストに設定することをお勧めします。

次のようなHTML ページがあり、`bar` と `bazz` のコンテナ要素のみがパーソナライズされることをご存知だとします。

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

SDKがサーバーからパーソナライズされたコンテンツを受信したら、**前処理フェーズ** が開始されます。 このフェーズでは応答が前処理され、パーソナライズされたコンテンツを含める必要がある要素が非表示になっていることを確認します。これらの要素を非表示にすると、`prehidingStyle` 設定オプションに基づいて作成された HTML スタイルタグが削除され、HTML の本文または非表示のコンテナ要素が表示されます。

すべてのパーソナライゼーションコンテンツが正常にレンダリングされたら、またはエラーが発生した場合は、**レンダリングフェーズ** が開始されます。 既に非表示になっている要素はすべて表示され、SDKによって非表示にされている要素がページに存在しないことが確認されます。

## 非同期展開のちらつきの管理

最高のページレンダリングパフォーマンスを得るためのレコメンデーションは、常に SDK を非同期で読み込むことです。ただし、これにより、パーソナライズされたコンテンツのレンダリングに何らかの影響が出ます。SDK が非同期で読み込まれる場合は、非表示のスニペットを使用する必要があります。HTML ページで SDK の前に、事前非表示のスニペットを追加する必要があります。本文全体を非表示にするスニペットの例を次に示します。

```html
<script>
  !function(e,a,n,t){
    if (a) return;
    var i=e.head;if(i){
    var o=e.createElement("style");
    o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
    setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
    (document, document.location.href.indexOf("adobe_authoring_enabled") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

HTML の本文またはコンテナ要素が長期間非表示にならないようにするために、事前非表示スニペットでは、`3000` ミリ秒後にデフォルトでスニペットを削除するタイマーを使用します。`3000` ミリ秒は最大待機時間です。サーバーからの応答が受信され、短時間で処理された場合は、可能な限り早く事前非表示 HTML スタイルタグが削除されます。
