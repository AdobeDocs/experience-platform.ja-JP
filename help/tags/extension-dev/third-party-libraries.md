---
title: サードパーティ製ライブラリの実装
description: Adobe Experience Platform タグ拡張機能内でサードパーティ製ライブラリをホストする様々な方法について説明します。
exl-id: d8eaf814-cce8-499d-9f02-b2ed3c5ee4d0
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 100%

---

# サードパーティ製ライブラリの実装

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../term-updates.md)を参照してください。

Adobe Experience Platform のタグ拡張機能の主な目的の 1 つに、既存のマーケティング技術（ライブラリ）を容易に Web サイトに実装できるようにするという点があります。拡張機能を使用すると、web サイトの HTML を手動で編集することなく、サードパーティのコンテンツ配信ネットワーク（CDN）が提供するライブラリを実装できます。

拡張機能内でサードパーティ（ベンダー）ライブラリをホストするには、いくつかの方法があります。このドキュメントでは、これらの実装方法の概要を示し、それぞれの長所と短所について説明します。

## 前提条件

このドキュメントでは、タグの機能や構成方法など、タグ内の拡張機能に関する十分な知識が必要です。詳しくは、 [拡張機能開発の概要](./overview.md) を参照してください。

## ベースコードの読み込みプロセス

タグのコンテキスト以外では、Web サイト上でマーケティングテクノロジーが通常どのように読み込まれるかを理解することが重要です。サードパーティのライブラリベンダーは、コードのスニペット（ベースコードと呼ばれます）を提供しています。ライブラリの機能を読み込むには、このスニペットを web サイトの HTML に埋め込む必要があります。

一般に、マーケティング技術のベースコードは、サイトに読み込む際に次のプロセスの一部のバリアントを実行します。

1. ベンダーライブラリとのやり取りに使用できるグローバル関数を設定する。
1. ベンダーライブラリを読み込む。
1. 設定および追跡のために、グローバル関数に対する、キューに格納された一連の初期呼び出しを実行する。

グローバル関数を初めて設定したときも、ライブラリの読み込みが完了する前に、関数を呼び出すことができます。おこなった呼び出しは、ベースコードのキューメカニズムに追加され、ライブラリが読み込まれると順番に実行されます。

ライブラリの読み込みが完了すると、グローバル関数は新しい関数に置き換えられます。この関数はキューをバイパスし、その関数に対する将来の呼び出しを直ちに処理します。

### ベースコードの例

次の JavaScript は、[Pinterest コンバージョンタグ](https://developers.pinterest.com/docs/ad-tools/conversion-tag/?)の非縮小化ベースコードの例です。このコードについては、このドキュメントで後述し、タグを使用して様々な実装戦略にベースコードがどのように適応するかを示します。

```js
!function(scriptUrl) {
  if (!window.pintrk) {
    window.pintrk = function() {
      window.pintrk.queue.push(
        Array.prototype.slice.call(arguments)
      );
    };
    window.pintrk.queue = []; 
    window.pintrk.version = '3.0';
    var scriptElement = document.createElement('script');
    scriptElement.async = true;
    scriptElement.src = scriptUrl;
    var firstScriptElement = 
      document.getElementsByTagName('script')[0];
    firstScriptElement.parentNode.insertBefore(
      scriptElement, firstScriptElement
    );
  }
}('https://s.pinimg.com/ct/core.js');
pintrk('load', 'YOUR_TAG_ID');
pintrk('page');
```

要約すると、上記のベースコードは、ライブラリ（`window.pintrk`）とやり取りするグローバル関数を作成する[即時実行関数式（IIFE）](https://developer.mozilla.org/ja-JP/docs/Glossary/IIFE)を提供します。また、`scriptURL` 変数に `https://s.pinimg.com/ct/core.js` の値（ライブラリの場所）を割り当てます。前述したように、ライブラリの読み込み前に呼び出された関数はキュー（`window.pintrk.queue`）にプッシュされ、ライブラリが使用可能になると順番に実行されます。

ベースコードの次の部分は、サイトでのライブラリの読み込み方法を理解するうえで最も重要です。

```js
var scriptElement = document.createElement("script");
scriptElement.async = true;
scriptElement.src = scriptUrl;
var firstScriptElement = 
  document.getElementsByTagName("script")[0];
firstScriptElement.parentNode.insertBefore(
  scriptElement, firstScriptElement
);
```

ベースコードはスクリプト要素を作成し、その要素が非同期で読み込まれるように設定し、`src` URL を `https://s.pinimg.com/ct/core.js` に設定します。次に、ドキュメント内に既に存在する最初のスクリプト要素の前に挿入し、スクリプト要素をドキュメントに追加します。

## タグ実装オプション

以下の節では、前述の例で示した Pinterest ベースコードを使用して、拡張機能にベンダーライブラリを読み込む様々な方法について説明します。各例では、Web サイト上のライブラリを読み込む [Web 拡張機能のアクションタイプ](./web/action-types.md) の作成方法について説明しています。

>[!NOTE]
>
>以下の例では、デモ目的でアクションタイプを使用しますが、サイト上でタグライブラリを読み込む関数にも同じ原則を適用できます。


以下の実行方法について説明します。

- [サードパーティ製ライブラリの実装](#implementing-third-party-libraries)
   - [前提条件](#prerequisites)
   - [ベースコードの読み込みプロセス](#base-code-loading-process)
      - [ベースコードの例](#base-code-example)
   - [タグ実装オプション](#tags-implementation-options)
      - [ベンダーホストから実行時に読み込む {#vendor-host}](#load-at-runtime-from-the-vendor-host-vendor-host)
      - [タグライブラリホストから実行時に読み込む](#load-at-runtime-from-the-tag-library-host)
      - [ライブラリを直接埋め込む](#embed-the-library-directly)
   - [次の手順](#next-steps)

### ベンダーホストから実行時に読み込む {#vendor-host}

ベンダーライブラリをホストする際の最も一般的な方法は、ベンダーの CDN を使用することです。ほとんどのベンダーライブラリのベースコードは、ベンダーの CDN からライブラリを読み込むように設定されています。そのため、同じ場所からライブラリを読み込むように拡張機能を設定できます。

CDN 上のファイルに対する更新は拡張機能によって自動的に読み込まれるので、通常、この方法は管理が最も簡単です。

この方法を使用する場合は、次のようなアクションタイプにベースコード全体を直接貼り付けるだけで済みます。

```js
module.exports = function() {
  !function(scriptUrl) {
    if (!window.pintrk) {
      window.pintrk = function() {
        window.pintrk.queue.push(
          Array.prototype.slice.call(arguments)
        );
      };
      window.pintrk.queue = []; 
      window.pintrk.version = "3.0";
      var scriptElement = document.createElement("script");
      scriptElement.async = true;
      scriptElement.src = scriptUrl;
      var firstScriptElement = 
        document.getElementsByTagName("script")[0];
      firstScriptElement.parentNode.insertBefore(
        scriptElement, firstScriptElement
      );
    }
  }("https://s.pinimg.com/ct/core.js");
  pintrk('load', 'YOUR_TAG_ID');
  pintrk('page');
};
```

また、この実装をリファクタリングするための追加の手順を実行できます。変数 `scriptElement` と `firstScriptElement` はエクスポートされた関数にまで拡張されたので、IIFE を削除できます。これは、これらの変数がグローバルになるリスクを回避できるためです。

さらに、タグは複数の[コアモジュール](./web/core.md)を提供します。これらは、あらゆる拡張機能が使用できるユーティリティです。特に、`@adobe/reactor-load-script` モジュールは、スクリプト要素を作成してドキュメントに追加することで、リモートの場所からスクリプトを読み込みます。スクリプトの読み込みプロセスでこのモジュールを使用すると、アクションコードをさらにリファクタリングできます。

```js
var loadScript = require('@adobe/reactor-load-script');
var scriptUrl = 'https://s.pinimg.com/ct/core.js';
module.exports = function() {
  if (!window.pintrk) {
    window.pintrk = function() {
      window.pintrk.queue.push(
        Array.prototype.slice.call(arguments)
      );
    };
    window.pintrk.queue = []; 
    window.pintrk.version = "3.0";
    loadScript(scriptUrl);   
  }
  pintrk('load', 'YOUR_TAG_ID');
  pintrk('page');
};
```

### タグライブラリホストから実行時に読み込む

ライブラリのホスティングにベンダー CDN を使用すると、CDN に障害が発生したり、ファイルが日時を問わず致命的なバグで更新されたり、ファイルに悪意を持って不正にアクセスされたりするなどのリスクが生じます。

これらの問題に対処するため、ベンダーライブラリを別のファイルとして拡張機能に含めることもできます。その後、メインのタグライブラリと一緒にファイルがホストされるように拡張機能を設定できます。実行時に、この拡張機能では、メインライブラリを web サイトに配信したのと同じサーバーからベンダーライブラリが読み込まれます。

>[!IMPORTANT]
>
>Pinterest ベンダーライブラリの場合と同様に、ベンダーライブラリがサードパーティサーバーから追加のコードを読み込むことがあります。この場合、ベンダーライブラリを拡張機能と一緒にバンドルすると、すべてのリスクに対する不安が完全に解消されない可能性があります。

これを実装するには、まずお使いのコンピューターにベンダーライブラリをダウンロードする必要があります。Pinterest の場合、ベンダーライブラリは [https://s.pinimg.com/ct/core.js](https://s.pinimg.com/ct/core.js) にあります。 ファイルをダウンロードしたら、拡張機能プロジェクト内に配置する必要があります。次の例では、ファイルの名前は `pinterest.js` で、プロジェクトディレクトリ内の `vendor` フォルダー内にあります。

ライブラリファイルがプロジェクトに含まれたら、[拡張機能マニフェスト](./manifest.md)（`extension.json`）を更新して、ベンダーライブラリをメインタグライブラリと一緒に配信する必要があることを示す必要があります。それには、`hostedLibFiles` 配列内にライブラリファイルのパスを追加します。

```json
{
  "hostedLibFiles": ["vendor/pinterest.js"]
}
```

最後に、メインライブラリをホストするサーバーと同じサーバーからベンダーライブラリを読み込むようにアクションコードを設定する必要があります。以下の例は、[前のセクション](#vendor-host)で作成したアクションコードを基にしています。次のように、[turbine オブジェクト](./turbine.md)を使用して、ベンダーファイルのファイル名（パスなし）を渡す必要があります。

```js
var loadScript = require('@adobe/reactor-load-script');
var scriptUrl = turbine.getHostedLibFileUrl('pinterest.js');
module.exports = function() {
  if (!window.pintrk) {
    window.pintrk = function() {
      window.pintrk.queue.push(
        Array.prototype.slice.call(arguments)
      );
    };
    window.pintrk.queue = []; 
    window.pintrk.version = "3.0";
    loadScript(scriptUrl);   
  }
  pintrk('load', 'YOUR_TAG_ID');
  pintrk('page');
};
```

この方法を使用する場合、ライブラリが CDN 上で更新されるたびに、ダウンロードしたベンダーファイルを手動で更新し、変更を新しいバージョンの拡張機能にリリースする必要があることに注意してください。

### ライブラリを直接埋め込む

ライブラリコードをアクションコード自体に直接埋め込むことで、ベンダーライブラリを完全に読み込む手間を省くことができます。これにより、ライブラリがメインタグライブラリの一部になります。この方法を使用すると、メインライブラリのサイズが大きくなりますが、追加の HTTP リクエストを実行して別のファイルを取得する必要がなくなります。

[前のセクション](#vendor-host)で作成したアクションコードを基に、スクリプトが読み込まれる行をスクリプト自体のコンテンツで置き換えることができます。

```js
module.exports = function() {
  if (!window.pintrk) {
    window.pintrk = function() {
      window.pintrk.queue.push(
        Array.prototype.slice.call(arguments)
      );
    };
    window.pintrk.queue = []; 
    window.pintrk.version = "3.0";
    // Paste the full vendor library code here.
  }
  pintrk('load', 'YOUR_TAG_ID');
  pintrk('page');
};
```

## 次の手順

このドキュメントでは、タグ拡張機能でサードパーティライブラリをホストする様々な方法の概要を示しました。ここで示した例ではライブラリに焦点を当てていますが、これらの手法は拡張機能で使用できるすべてのコードに適用されます。

アクションタイプ、拡張機能マニフェスト、コアモジュール、turbine オブジェクトといった、拡張機能を設定するためのツールについて詳しくは、このガイド内でリンクされているドキュメントを参照してください。
