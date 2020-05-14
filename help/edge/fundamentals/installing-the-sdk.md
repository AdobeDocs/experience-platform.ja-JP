---
title: Adobe Experience Platform Web SDK のインストール
seo-title: Adobe Experience Platform Web SDK：SDK のインストール
description: Experience Platform Web SDK のインストール方法について説明します
seo-description: Experience Platform Web SDK のインストール方法について説明します
translation-type: tm+mt
source-git-commit: e9fb726ddb84d7a08afb8c0f083a643025b0f903
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 100%

---


# SDK のインストール

Adobe Experience Platform Web SDK を実装する最初の手順として、次の「ベースコード」を、HTML の `<head>` のタグ内のできるだけ上位にコピーして貼り付けます。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js" async></script>
```

ベースコードは、`alloy` という名前のグローバル関数を作成します。この関数を使用して SDK を操作します。グローバル関数に別の名前を付けたい場合は、`alloy` の名前を次のように変更できます。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname"]);
</script>
<script src="alloy.js" async></script>
```

この例では、グローバル関数の名前が `alloy` から `mycustomname` に変更されています。

>[!IMPORTANT]
>潜在的な問題を回避するため、1 文字以上の文字を含み、この名前は、`window` で既に見つかったプロパティの名前と競合しない名前を使用してください。

このベースコードは、グローバル関数を作成するだけでなく、サーバ上でホストされる外部ファイル（`alloy.js`）に含まれる追加のコードも読み込みます。デフォルトでは、このコードは非同期で読み込まれ、Web ページのパフォーマンスを可能な限り高めます。これは推奨される実装です。

## Internet Explorer のサポート

この SDK は、非同期タスクの完了を伝える方法として promise を使用します。SDK が使用する [promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) 実装は、Internet Explorer を除くすべてのターゲットブラウザーでネイティブにサポートされます。Internet Explorer で SDKを使用するには、`window.Promise` の[ポリフィル](https://remysharp.com/2010/10/08/what-is-a-polyfill)をおこなう必要があります。

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. Internet Explorer で Web サイトを開きます。
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

`window.Promise` のポリフィルが必要と判断した場合は、前述のベースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

これにより、`window.Promise` が有効な promise 実装であることを確認するスクリプトが読み込まれます。

## JavaScript ファイルの同期読み込み

前述のとおり、Web サイトの HTML にコピー＆ペーストしたベースコードは、追加コードが入った外部ファイルを読み込みます。この追加のコードには、SDK のコア機能が含まれています。このファイルの読み込み中に実行しようとしたコマンドは、キューに追加され、ファイルの読み込み後に処理されます。これは、最もパフォーマンスの高いインストール方法です。

ただし、特定の状況では、ファイルを同期的に読み込むことが望ましい場合もあります（これらの状況に関する詳細は後で説明します）。これをおこなうと、外部ファイルが読み込まれて実行されるまで、HTML ドキュメントの残りの部分がブラウザーで解析およびレンダリングされなくなります。通常、プライマリコンテンツをユーザーに表示する前にこの遅延が発生するのはお勧めしませんが、状況によっては合理的な場合もあります。

非同期ではなく同期的にファイルを読み込むには、次に示すように、2 番目の　`script`　タグから `async` 属性を削除します。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js"></script>
```
