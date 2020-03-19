---
title: Adobe Experience Platform Web SDKのインストール
seo-title: SDKのインストールに関するAdobe Experience Platform Web SDK
description: エクスペリエンスプラットフォームWeb SDKのインストール方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKのインストール方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）SDKのインストール

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Web SDKを実装する最初の手順は、次の「ベースコード」をHTMLのタグ内でできる限り高い位置にコピーして貼り付 `<head>` けることです。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js" async></script>
```

ベースコードは、という名前のグローバル関数を作成しま `alloy`す。 この関数を使用してSDKを操作します。 グローバル関数に別の名前を付けたい場合は、次のように名前を変更 `alloy` できます。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname"]);
</script>
<script src="alloy.js" async></script>
```

この例では、グローバル関数の名前が、ではな `mycustomname`く変更されま `alloy`す。

>[!IMPORTANT]
>潜在的な問題を回避するには、1文字以上の文字を含む名前を使用します。この名前は、既に見つかったプロパティの名前と競合しません `window`。

このベースコードは、グローバル関数を作成するだけでなく、サーバ上でホストされる外部ファイル\(`alloy.js`\)に含まれる追加のコードもロードします。 デフォルトでは、このコードは非同期で読み込まれ、Webページのパフォーマンスを可能な限り高めます。 これは推奨される実装です。

## Internet Explorerのサポート

このSDKは、プロミスを利用します。これは、非同期タスクの完了を通信する方法です。 SDKが使 [用する](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) Promise実装は、Internet Explorerを除くすべてのターゲットブラウザーでネイティブにサポートされます。 Internet ExplorerでSDKを使用するには、多重入力が必要で `window.Promise` す [](https://remysharp.com/2010/10/08/what-is-a-polyfill)。

既に多重塗り潰しが行われているかどうかを確認するには、次の手順を実 `window.Promise` 行します。

1. Internet ExplorerでWebサイトを開きます。
1. ブラウザーのデバッグコンソールを開きます。
1. コンソール `window.Promise` に入力し、Enterキーを押します。

他のものが表示された場合 `undefined` は、既に多重塗り潰しが行われている可能性がありま `window.Promise`す。 上記のインストール手順を完了し `window.Promise` た後にWebサイトを読み込むことで、がポリフィルされているかどうかを判断する別の方法もあります。 SDKが約束に関する何かを言及するエラーをスローした場合、多くの場合、入力が行われていない可能性がありま `window.Promise`す。

ポリフィルが必要と判断した場合は、前述のベ `window.Promise`ースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

これにより、が有効なPromise実装であるこ `window.Promise` とを確認するスクリプトが読み込まれます。

## JavaScriptファイルの同期読み込み

前述のとおり、WebサイトのHTMLにコピー&amp;ペーストしたベースコードは、追加のコードを含む外部ファイルを読み込みます。 この追加のコードには、SDKのコア機能が含まれています。 このファイルの読み込み中に実行しようとしたコマンドは、キューに格納され、ファイルの読み込み後に処理されます。 これは、最もパフォーマンスの高いインストール方法です。

ただし、特定の状況では、ファイルを同期的に読み込むことが望ましい場合もあります（これらの状況に関する詳細は後で説明します）。 これを行うと、外部ファイルが読み込まれて実行されるまで、HTMLドキュメントの残りの部分がブラウザーで解析およびレンダリングされなくなります。 通常、プライマリコンテンツをユーザーに表示する前にこの遅延が発生するのはお勧めしませんが、状況に応じて意味があります。

非同期ではなく同期的にファイルを読み込むには、次に示すよ `async` うに、2番目のタグか `script` ら属性を削除します。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js"></script>
```
