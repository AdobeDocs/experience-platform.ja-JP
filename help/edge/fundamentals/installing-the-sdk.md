---
title: Adobe Experience PlatformWeb SDKのインストール
description: Experience Platform Web SDK のインストール方法について説明します.
keywords: web sdkのインストール；web sdkのインストール；internet explorer;promise;
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 62%

---


# SDKのインストール{#installing-the-sdk}

Adobe Experience PlatformWeb SDKの使い方は、[Adobe Experience Platform Launch](http://launch.adobe.com/jp)を経由することをお勧めします。 拡張機能カタログで`AEP Web SDK`を検索し、インストールして、拡張機能を設定します。

Adobe Experience PlatformWeb SDKは、CDNでも使用できます。 このファイルを参照するか、ダウンロードして、独自のインフラストラクチャ上でホストできます。 縮小版および縮小版以外のバージョンで利用できます。 縮小されていないバージョンは、デバッグ目的で役立ちます。

URL構造：https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.min.jsまたはalloy.js（縮小されていないバージョン用）

次に例を示します。

* 縮小：[https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js](https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js)
* 縮小解除：[https://cdn1.adoberesources.net/alloy/2.3.0/alloy.js](https://cdn1.adoberesources.net/alloy/2.3.0/alloy.js)

## コードの追加{#adding-the-code}

Adobe Experience Platform[!DNL Web SDK]を実装する最初の手順は、次の「ベースコード」をHTMLの`<head>`タグ内でできる限り高くコピーして貼り付けることです。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js" async></script>
```

ベースコードは、`alloy` という名前のグローバル関数を作成します。この関数を使用して SDK を操作します。グローバル関数に別の名前を付けたい場合は、`alloy` の名前を次のように変更できます。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js" async></script>
```

この例では、グローバル関数の名前が `alloy` から `mycustomname` に変更されています。

>[!IMPORTANT]
>
>潜在的な問題を回避するため、1 文字以上の文字を含み、この名前は、`window` で既に見つかったプロパティの名前と競合しない名前を使用してください。

このベースコードは、グローバル関数を作成するだけでなく、サーバ上でホストされる外部ファイル（`alloy.js`）に含まれる追加のコードも読み込みます。デフォルトでは、このコードは非同期で読み込まれ、Web ページのパフォーマンスを可能な限り高めます。これは推奨される実装です。

## Internet Explorer のサポート {#support-internet-explore}

この SDK は、非同期タスクの完了を伝える方法として promise を使用します。SDKが使用する[Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise)実装は、[!DNL Internet Explorer]を除くすべてのターゲットブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]でSDKを使用するには、`window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill)が必要です。

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. [!DNL Internet Explorer]でWebサイトを開きます。
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

`window.Promise` のポリフィルが必要と判断した場合は、前述のベースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

これにより、`window.Promise` が有効な promise 実装であることを確認するスクリプトが読み込まれます。

>[!NOTE]
>
>別のPromise実装を読み込む場合は、`Promise.prototype.finally`をサポートしていることを確認してください。

## JavaScript ファイルの同期読み込み {#loading-javascript-synchronously}

「[コード](#adding-the-code)の追加」で説明したように、コピーしてWebサイトのHTMLに貼り付けたベースコードは、追加のコードを含む外部ファイルを読み込みます。 この追加のコードには、SDK のコア機能が含まれています。このファイルの読み込み中に実行しようとしたコマンドは、キューに追加され、ファイルの読み込み後に処理されます。これは、最もパフォーマンスの高いインストール方法です。

ただし、特定の状況では、ファイルを同期的に読み込むことが望ましい場合もあります（これらの状況に関する詳細は後で説明します）。これをおこなうと、外部ファイルが読み込まれて実行されるまで、HTML ドキュメントの残りの部分がブラウザーで解析およびレンダリングされなくなります。通常、プライマリコンテンツをユーザーに表示する前にこの遅延が発生するのはお勧めしませんが、状況によっては合理的な場合もあります。

非同期ではなく同期的にファイルを読み込むには、次に示すように、2 番目の　`script`　タグから `async` 属性を削除します。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js"></script>
```
