---
title: Adobe Experience Platform Web SDK のインストール
description: Experience Platform Web SDK のインストール方法について説明します.
keywords: web sdk のインストール；web sdk のインストール；internet explorer;promise;npm パッケージ
exl-id: b1de7ca1-d0d2-4661-a273-a1acf29afcd5
source-git-commit: 8ded2aed32dffa4f0923fedac7baf798e68a9ec9
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 31%

---

# SDK のインストール {#installing-the-sdk}

Adobe Experience Platform Web SDK の使用方法は 3 つあります。

1. Adobe Experience Platform Web SDK を使用する推奨される方法は、データ収集 UI またはExperience PlatformUI を使用することです。
1. Adobe Experience Platform Web SDK は、コンテンツ配信ネットワーク (CDN) でも使用でき、
1. EcmaScript 5 モジュールと EcmaScript 2015(ES6) モジュールを書き出す NPM ライブラリを使用します。

## オプション 1:タグ拡張機能のインストール

タグ拡張機能に関するドキュメントについては、 [launch ドキュメント](../../tags/extensions/web/sdk/overview.md)

## オプション 2:事前にビルドされたスタンドアロンバージョンのインストール

事前ビルドバージョンは、CDN で使用できます。 CDN 上のライブラリをページで直接参照することも、独自のインフラストラクチャでダウンロードしてホストすることもできます。 縮小形式および縮小されていない形式で使用できます。 非縮小バージョンは、デバッグに役立ちます。

URL 構造：https://cdn1.adoberesources.net/alloy/[バージョン]/alloy.min.js または alloy.js （非縮小版）

例：


* 縮小済み： [https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js](https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js)
* 縮小解除済み： [https://cdn1.adoberesources.net/alloy/2.6.4/alloy.js](https://cdn1.adoberesources.net/alloy/2.6.4/alloy.js)


### コードの追加 {#adding-the-code}

事前にビルドされたスタンドアロンバージョンでは、ページに直接「ベースコード」を追加する必要があります。 次の「ベースコード」を、できる限り上位の `<head>` タグのHTML:

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js" async></script>
```

「ベースコード」は、 `alloy`. この関数を使用して SDK を操作します。グローバル関数に別の名前を付けたい場合は、 `alloy` 名前は次のようになります。

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

この SDK は、非同期タスクの完了を伝える方法として promise を使用します。 この [プロミス](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) SDK で使用される実装は、次を除くすべての target ブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]. で SDK を使用するには、以下を実行します。 [!DNL Internet Explorer]を選択し、 `window.Promise` [多充填の](https://remysharp.com/2010/10/08/what-is-a-polyfill).

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. で Web サイトを開きます。 [!DNL Internet Explorer].
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

既に決定している場合は、ポリフィルする必要があります `window.Promise`の上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

このタグは、 `window.Promise` は、有効な Promise 実装です。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、がをサポートしていることを確認してください。 `Promise.prototype.finally`.

### JavaScript ファイルの同期読み込み {#loading-javascript-synchronously}

「 」の節で説明されているように [コードの追加](#adding-the-code)の場合、Web サイトのHTMLにコピー&amp;ペーストしたベースコードは、外部ファイルを読み込みます。 外部ファイルには、SDK のコア機能が含まれています。 このファイルの読み込み中に実行しようとしたコマンドは、キューに追加され、ファイルの読み込み後に処理されます。 ファイルを非同期で読み込むことが、最もパフォーマンスの高いインストール方法です。

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

Adobe Experience Platform Web SDK は、NPM パッケージとしても使用できます。 [NPM](https://www.npmjs.com) は、JavaScript 用パッケージマネージャーです。 NPM パッケージをインストールすると、Adobe Experience Platform Web SDK JavaScript のビルドプロセスを制御できます。 NPM パッケージは、ブラウザーで実行する EcmaScript バージョン 5 モジュールまたは EcmaScript バージョン 2015(ES6) モジュールを公開します。

```bash
npm install @adobe/alloy
```

Adobe Experience Platform Web SDK の NPM パッケージは、 `createInstance` 関数に置き換えます。 この関数は、インスタンスの作成に使用されます。 関数に渡される name オプションは、ログに記録されるプレフィックスを制御します。 パッケージの使用例を以下に示します。

### パッケージを ECMAScript 2015(ES6) モジュールとして使用

```javascript
import { createInstance } from "@adobe/alloy";
const alloy = createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

>[!NOTE]
>
>NPM パッケージは、 CommonJS モジュールに依存します。そのため、bundler を使用する場合は、bundler が CommonJS モジュールをサポートしていることを確認してください。 一部のバンドル [ロールアップ](https://rollupjs.org)、が必要 [プラグイン](https://www.npmjs.com/package/@rollup/plugin-commonjs) を使用すれば、CommonJS のサポートを提供できます。

### ECMAScript 5 モジュールとしてのパッケージの使用

```javascript
var alloyLibrary = require("@adobe/alloy");
var alloy = alloyLibrary.createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

### Internet Explorer のサポート

Adobe Experience Platform SDK は、非同期タスクの完了を伝える方法として promise を使用します。 この [プロミス](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) SDK で使用される実装は、次を除くすべての target ブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]. で SDK を使用するには、以下を実行します。 [!DNL Internet Explorer]を選択し、 `window.Promise` [多充填の](https://remysharp.com/2010/10/08/what-is-a-polyfill).

promise のポリフィルに使用できるライブラリの 1 つに、promise-polyfill があります。 詳しくは、 [promise-polyfill ドキュメント](https://www.npmjs.com/package/promise-polyfill) を参照してください。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、がをサポートしていることを確認してください。 `Promise.prototype.finally`.
