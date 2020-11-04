---
title: Adobe Experience Platform Web SDK のインストール
seo-title: Adobe Experience Platform Web SDK：SDK のインストール
description: Experience Platform Web SDK のインストール方法について説明します
seo-description: Experience Platform Web SDK のインストール方法について説明します
keywords: web sdk installation;installing web sdk;internet explorer;promise;
translation-type: tm+mt
source-git-commit: d23568f7ce63df5aa98dc237a6671eeadde0c9b2
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 66%

---


# SDK のインストール {#installing-the-sdk}

Adobe Experience PlatformWeb SDKの使用方法は、 [Adobe Experience Platform Launchを使用することをお勧めします](http://launch.adobe.com/jp)。 拡張機能カタログ `AEP Web SDK` 内でを検索し、インストールしてから、拡張機能を設定します。

AEP Web SDKは、CDN上でも使用できます。 このファイルを参照するか、ダウンロードして、独自のインフラストラクチャ上でホストできます。 縮小版および縮小版以外のバージョンで利用できます。 縮小されていないバージョンは、デバッグ目的で役立ちます。

URL構造：https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.min.jsまたはalloy.js（縮小されていないバージョン用）

以下に例を示します。

* 縮小： [https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js](https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js)
* 縮小解除： [https://cdn1.adoberesources.net/alloy/2.3.0/alloy.js](https://cdn1.adoberesources.net/alloy/2.3.0/alloy.js)

## コードの追加 {#adding-the-code}

The first step in implementing the Adobe Experience Platform [!DNL Web SDK] is to copy and paste the following &quot;base code&quot; as high as possible in the `<head>` tag of your HTML:

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

この SDK は、非同期タスクの完了を伝える方法として promise を使用します。The [Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) implementation used by the SDK is natively supported by all target browsers except [!DNL Internet Explorer]. To use the SDK on [!DNL Internet Explorer], you need to have `window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill).

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. Open your website in [!DNL Internet Explorer].
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
>別のPromise実装を読み込む場合は、それがサポートしていることを確認してくだ `Promise.prototype.finally`さい。

## JavaScript ファイルの同期読み込み {#loading-javascript-synchronously}

As explained in the section [Adding the code](#adding-the-code), the base code you have copied and pasted into your website&#39;s HTML loads an external file with additional code. この追加のコードには、SDK のコア機能が含まれています。このファイルの読み込み中に実行しようとしたコマンドは、キューに追加され、ファイルの読み込み後に処理されます。これは、最もパフォーマンスの高いインストール方法です。

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
