---
title: Adobe Experience Platform Web SDK を使用して、パーソナライズされたエクスペリエンスのちらつきを管理する
description: Adobe Experience Platform Web SDK を使用して、ユーザーエクスペリエンスのちらつきを管理する方法について説明します。
keywords: target;flicker;prehidingStyle；非同期；非同期；
exl-id: f4b59109-df7c-471b-9bd6-7082e00c293b
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 53%

---

# フリッカーの管理

パーソナライゼーションコンテンツをレンダリングする場合、SDK はちらつきがないことを確認する必要があります。ちらつき (FOOC( 元のコンテンツのFlash) とも呼ばれます ) は、テスト/パーソナライゼーション中に代替コンテンツが表示される前に、元のコンテンツが短く表示される場合です。 SDK は、パーソナライゼーションコンテンツが正常にレンダリングされるまでこれらの要素が非表示になるよう、ページの要素に CSS スタイルを適用しよう試みます。

ちらつきの制御方法は、Web SDK を同期的にデプロイするか非同期的にデプロイするかによって異なります。 次を確認します。 `<head>` タグを使用して、 `alloy.js` またはタグローダー。 この `async` 属性 `<script>` タグは、Web SDK が非同期で読み込まれるかどうかを決定します。

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

## 同期デプロイメントのちらつき制御

同期ちらつき管理は、次の 3 つのフェーズに分かれます。

1. 事前非表示
1. 前処理
1. レンダリング

期間 **事前非表示段階**&#x200B;に設定されていない場合、SDK は [`prehidingStyle`](../commands/configure/prehidingstyle.md) 設定プロパティを使用してHTMLスタイルタグを作成し、DOM に追加して、ページ内の目的のセクションが非表示になっていることを確認します。 ページのどの部分がパーソナライズされるかが不明な場合は、`prehidingStyle` を `body { opacity: 0 !important }` に設定することをお勧めします。これにより、ページ全体が非表示になります。ただし、これは、Lighthouse や Web ページテストなどのツールで報告されるページレンダリングパフォーマンスの低下につながる悪影響を及ぼします。 最高のページレンダリングパフォーマンスを得るには、`prehidingStyle` をコンテナ要素（パーソナライズするページの部分を含む）のリストに設定することをお勧めします。

次のようなHTMLページがあり、 `bar` および `bazz` コンテナ要素はパーソナライズされます。

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

SDK がサーバーからパーソナライズされたコンテンツを受け取ると、 **前処理相** 開始します。 このフェーズでは応答が前処理され、パーソナライズされたコンテンツを含める必要がある要素が非表示になっていることを確認します。これらの要素を非表示にすると、`prehidingStyle` 設定オプションに基づいて作成された HTML スタイルタグが削除され、HTML の本文または非表示のコンテナ要素が表示されます。

すべてのパーソナライゼーションコンテンツが正常にレンダリングされた後、またはエラーが発生した場合は、 **レンダリング段階** 開始します。 SDK で非表示にされた非表示の要素がページにないことを確認するために、以前非表示にされたすべての要素が表示されます。

## 非同期デプロイメントでちらつきを管理

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
