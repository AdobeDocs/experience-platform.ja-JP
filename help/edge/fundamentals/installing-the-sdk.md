---
title: Adobe Experience PlatformWeb SDKのインストール
description: Experience Platform Web SDK のインストール方法について説明します.
keywords: Web sdkのインストール；Web sdkのインストール；internet explorer;promise;npmパッケージ
translation-type: tm+mt
source-git-commit: 29272856d766e5adeb4b00ea62b28ea77abe338e
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 33%

---


# SDKのインストール{#installing-the-sdk}

Adobe Experience PlatformWeb SDKの使用には、次の3つのサポートされている方法があります。

1. Adobe Experience PlatformWeb SDKの使い方は、[Adobe Experience Platform Launch](https://launch.adobe.com/)を経由することをお勧めします。
1. Adobe Experience PlatformWeb SDKは、コンテンツ配信ネットワーク(CDN)でも使用できます。
1. EcmaScript 5およびEcmaScript 2015(ES6)モジュールを書き出すNPMライブラリを使用します。

## オプション1:Adobe Experience Platform Launch拡張機能のインストール

Adobe Experience Platform Launchの拡張機能に関するドキュメントについては、[起動ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)を参照してください

## オプション2:プレビルトスタンドアロンバージョンのインストール

事前にビルドされたバージョンは、CDNで利用できます。 CDN上のライブラリを直接ページで参照することも、独自のインフラストラクチャでダウンロードしてホストすることもできます。 縮小および縮小されていない形式で使用できます。 縮小されていないバージョンは、デバッグの目的に役立ちます。

URL構造：https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.min.jsまたはalloy.js（縮小されていないバージョン用）

次に例を示します。

* 縮小：[https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js](https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js)
* 縮小されません：[https://cdn1.adoberesources.net/alloy/2.3.0/alloy.js](https://cdn1.adoberesources.net/alloy/2.3.0/alloy.js)

### コードの追加{#adding-the-code}

プレビルトスタンドアロンバージョンでは、ページに直接「ベースコード」を追加する必要があります。 次の「ベースコード」を、HTMLの`<head>`タグ内でできる限り高い位置にコピー&amp;ペーストします。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.3.0/alloy.min.js" async></script>
```

「ベースコード」は、`alloy`という名前のグローバル関数を作成します。 この関数を使用して SDK を操作します。グローバル関数に別の名前を付けたい場合は、`alloy`名前を次のように変更します。

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

### Internet Explorer のサポート {#support-internet-explore}

このSDKは、プロミスを使用します。プロミスは、非同期タスクの完了を通信する方法です。 SDKが使用する[Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise)実装は、[!DNL Internet Explorer]を除くすべてのターゲットブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]でSDKを使用するには、`window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill)が必要です。

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. [!DNL Internet Explorer]でWebサイトを開きます。
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

`window.Promise`をポリフィルする必要があると判断した場合は、前に提供したベースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

このタグは、`window.Promise`が有効なPromise実装であることを確認するスクリプトを読み込みます。

>[!NOTE]
>
>別のPromise実装を読み込む場合は、`Promise.prototype.finally`をサポートしていることを確認してください。

### JavaScript ファイルの同期読み込み {#loading-javascript-synchronously}

「[コード](#adding-the-code)を追加」で説明したように、コピーしてWebサイトのHTMLに貼り付けたベースコードは、外部ファイルを読み込みます。 外部ファイルには、SDKのコア機能が含まれています。 このファイルの読み込み中に実行を試みたコマンドは、キューに登録され、ファイルの読み込み後に処理されます。 ファイルの非同期読み込みは、インストールで最もパフォーマンスの高い方法です。

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

## オプション3:NPMパッケージの使用

Adobe Experience PlatformWeb SDKは、NPMパッケージとしても利用できます。 [](https://www.npmjs.com) NPMは、JavaScriptのパッケージマネージャーです。NPMパッケージをインストールすると、Adobe Experience PlatformWeb SDK JavaScriptのビルドプロセスを制御できます。 NPMパッケージは、ブラウザーで実行するEcmaScriptバージョン5モジュールまたはEcmaScriptバージョン2015(ES6)モジュールを公開します。

```bash
npm install @adobe/alloy
```

Adobe Experience PlatformWeb SDKのNPMパッケージは、`createInstance`関数を公開します。 この関数は、インスタンスの作成に使用されます。 関数に渡されるnameオプションは、ログで使用されるプレフィックスを制御します。 パッケージの使用例を以下に示します。

### ECMAScript 2015(ES6)モジュールとしてのパッケージの使用

```javascript
import { createInstance } from "@adobe/alloy";
const alloy = createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

### ECMAScript 5モジュールとしてのパッケージの使用

```javascript
var alloyLibrary = require("@adobe/alloy");
var alloy = alloyLibrary.createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

### Internet Explorer のサポート

Adobe Experience PlatformSDKは、プロミスを使用します。これは、非同期タスクの完了を伝える手段です。 SDKが使用する[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)実装は、[!DNL Internet Explorer]を除くすべてのターゲットブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]でSDKを使用するには、`window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill)が必要です。

PolyFill約束のために使用できるライブラリの1つは、Promise-Polyfillです。 NPMでのインストール方法の詳細については、[promise-polyfillのドキュメント](https://www.npmjs.com/package/promise-polyfill)を参照してください。

>[!NOTE]
>
>別のPromise実装を読み込む場合は、`Promise.prototype.finally`をサポートしていることを確認してください。