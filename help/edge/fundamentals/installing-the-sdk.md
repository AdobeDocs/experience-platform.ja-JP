---
title: Adobe Experience Platform Web SDK のインストール
description: Experience Platform Web SDK のインストール方法について説明します.
keywords: web sdk のインストール；web sdk のインストール；internet explorer;promise;npm パッケージ
exl-id: b1de7ca1-d0d2-4661-a273-a1acf29afcd5
source-git-commit: f5d3c5911357d4b76e4d38564bf637e2549469d6
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 32%

---

# SDK のインストール {#installing-the-sdk}

Adobe Experience Platform Web SDK の使用方法は 3 つあります。

1. Adobe Experience Platform Web SDK を使用するには、データ収集 UI を使用することをお勧めします。
1. Adobe Experience Platform Web SDK は、コンテンツ配信ネットワーク (CDN) でも使用できます。
1. EcmaScript 5 および EcmaScript 2015(ES6) モジュールを書き出す NPM ライブラリを使用します。

## オプション 1:タグ拡張機能のインストール

タグ拡張のドキュメントについては、[launch のドキュメント ](../../tags/extensions/web/sdk/overview.md) を参照してください。

## オプション 2:事前にビルドされたスタンドアロンバージョンのインストール

事前にビルドされたバージョンは、CDN で使用できます。 CDN 上のライブラリを直接ページで参照することも、独自のインフラストラクチャでダウンロードしてホストすることもできます。 縮小および縮小されていない形式で使用できます。 非縮小バージョンはデバッグに役立ちます。

URL 構造：https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.min.js または alloy.js （非縮小版）

以下に例を示します。


* 縮小：[https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js](https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js)
* 縮小解除：[https://cdn1.adoberesources.net/alloy/2.6.4/alloy.js](https://cdn1.adoberesources.net/alloy/2.6.4/alloy.js)


### コードの追加 {#adding-the-code}

事前にビルドされたスタンドアロンバージョンには、ページに直接追加された「ベースコード」が必要です。 次の「ベースコード」を、HTMLの `<head>` タグのできる限り上位にコピーして貼り付けます。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js" async></script>
```

「ベースコード」は、`alloy` という名前のグローバル関数を作成します。 この関数を使用して SDK を操作します。グローバル関数に別の名前を付ける場合は、`alloy` の名前を次のように変更します。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js" async></script>
```

この例では、グローバル関数の名前が `alloy` から `mycustomname` に変更されています。

>[!IMPORTANT]
>
>潜在的な問題を回避するため、1 文字以上の文字を含み、この名前は、`window` で既に見つかったプロパティの名前と競合しない名前を使用してください。

このベースコードは、グローバル関数を作成するだけでなく、サーバ上でホストされる外部ファイル（`alloy.js`）に含まれる追加のコードも読み込みます。デフォルトでは、このコードは非同期で読み込まれ、Web ページのパフォーマンスを可能な限り高めます。これは推奨される実装です。

### Internet Explorer のサポート {#support-internet-explore}

この SDK は、非同期タスクの完了を伝える方法である promise を使用します。 SDK で使用される [Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) 実装は、[!DNL Internet Explorer] を除くすべてのターゲットブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer] で SDK を使用するには、`window.Promise` [ ポリフィル ](https://remysharp.com/2010/10/08/what-is-a-polyfill) が必要です。

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. [!DNL Internet Explorer] で Web サイトを開きます。
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

`window.Promise` のポリフィルが必要と判断した場合は、前述のベースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

このタグは、`window.Promise` が有効な Promise 実装であることを確認するスクリプトを読み込みます。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、`Promise.prototype.finally` をサポートしていることを確認してください。

### JavaScript ファイルの同期読み込み {#loading-javascript-synchronously}

[ コード ](#adding-the-code) の追加で説明したように、Web サイトのHTMLにコピー&amp;ペーストしたベースコードは、外部ファイルを読み込みます。 外部ファイルには、SDK のコア機能が含まれています。 このファイルの読み込み中に実行しようとしたコマンドは、キューに入れられ、ファイルの読み込み後に処理されます。 ファイルを非同期で読み込むのが、最もパフォーマンスの高いインストール方法です。

ただし、特定の状況では、ファイルを同期的に読み込むことが望ましい場合もあります（これらの状況に関する詳細は後で説明します）。これをおこなうと、外部ファイルが読み込まれて実行されるまで、HTML ドキュメントの残りの部分がブラウザーで解析およびレンダリングされなくなります。通常、プライマリコンテンツをユーザーに表示する前にこの遅延が発生するのはお勧めしませんが、状況によっては合理的な場合もあります。

非同期ではなく同期的にファイルを読み込むには、次に示すように、2 番目の　`script`　タグから `async` 属性を削除します。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js"></script>
```

## オプション 3:NPM パッケージの使用

Adobe Experience Platform Web SDK は、NPM パッケージとしても使用できます。 [](https://www.npmjs.com) NPM は、JavaScript 用パッケージマネージャーです。NPM パッケージをインストールすると、Adobe Experience Platform Web SDK JavaScript のビルドプロセスを制御できます。 NPM パッケージは、ブラウザーで実行する EcmaScript バージョン 5 モジュールまたは EcmaScript バージョン 2015(ES6) モジュールを公開します。

```bash
npm install @adobe/alloy
```

Adobe Experience Platform Web SDK の NPM パッケージは、`createInstance` 関数を公開します。 この関数は、インスタンスの作成に使用されます。 関数に渡される name オプションは、ログに使用されるプレフィックスを制御します。 パッケージの使用例を以下に示します。

### ECMAScript 2015 (ES6) モジュールとしてのパッケージの使用

```javascript
import { createInstance } from "@adobe/alloy";
const alloy = createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

>[!NOTE]
>
>NPM パッケージは、 CommonJS モジュールに依存しています。そのため、bundler を使用する場合は、bundler が CommonJS モジュールをサポートしていることを確認してください。 [Rollup](https://rollupjs.org) など、一部のバンドラーには、CommonJS をサポートする [ プラグイン ](https://www.npmjs.com/package/@rollup/plugin-commonjs) が必要です。

### ECMAScript 5 モジュールとしてのパッケージの使用

```javascript
var alloyLibrary = require("@adobe/alloy");
var alloy = alloyLibrary.createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

### Internet Explorer のサポート

Adobe Experience Platform SDK は、非同期タスクの完了を伝える方法として promise を使用します。 SDK で使用される [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 実装は、[!DNL Internet Explorer] を除くすべてのターゲットブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer] で SDK を使用するには、`window.Promise` [ ポリフィル ](https://remysharp.com/2010/10/08/what-is-a-polyfill) が必要です。

Promise のポリフィルに使用できるライブラリの 1 つに、promise-polyfill があります。 NPM でのインストール方法の詳細については、[promise-polyfill のドキュメント ](https://www.npmjs.com/package/promise-polyfill) を参照してください。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、`Promise.prototype.finally` をサポートしていることを確認してください。
