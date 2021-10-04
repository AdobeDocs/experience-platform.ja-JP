---
title: Web 拡張機能用のコアライブラリモジュール
description: Web 拡張機能で使用できるコアライブラリモジュールについて説明します。
exl-id: 7fb63208-aed0-4add-b6da-8e4aea063d0a
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 100%

---

# Web 拡張機能用のコアライブラリモジュール

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

このドキュメントでは、Web 拡張機能内で使用できるコアライブラリモジュールの一覧を示します。これらのモジュールには `require('@adobe/{MODULE}')` を使ってアクセスできます。`{MODULE}` は、使用するコアモジュールの名前です。

## [!DNL reactor-object-assign]

`reactor-object-assign` は、ソースオブジェクトからターゲットオブジェクトにプロパティをコピーして、ネイティブ [`Object.assign`](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) メソッドを模倣します。

```javascript
var objectAssign = require('@adobe/reactor-object-assign');
var all = objectAssign({ a: 'a' }, { b: 'b' });
```

## [!DNL reactor-cookie]

`reactor-cookie` オブジェクトは、Cookie の読み取りと書き込みを行うユーティリティです。詳しくは、 [js-cookie npm のパッケージ](https://www.npmjs.com/package/js-cookie) を参照してください。

```javascript
var cookie = require('@adobe/reactor-cookie');
cookie.set('foo', 'bar');
console.log(cookie.get('foo'));
cookie.remove('foo');
```

### [!DNL reactor-document]

`reactor-document` は、[`Document`](https://developer.mozilla.org/ja-JP/docs/Web/API/Document) オブジェクトを表します。[`inject-loader`](https://www.npmjs.com/package/inject-loader) のようなユーティリティを使用した、モック `document` オブジェクトの挿入テストが可能になるため、これはモジュールをテストする際に役立ちます。

```javascript
var document = require('@adobe/reactor-document');
console.log(document.location);
```

### [!DNL reactor-query-string]

`reactor-query-string` は、[クエリ文字列](https://developer.mozilla.org/ja/docs/Web/API/HTMLHyperlinkElementUtils/search)を解析およびシリアル化するためのユーティリティです。

```javascript
var queryString = require('@adobe/reactor-query-string');
var parsed = queryString.parse(location.search);
console.log(parsed.campaign);
var obj = {
  campaign: 'Campaign A'
};
var stringified = queryString.stringify(obj);
```

このユーティリティには次のメソッドがあります。

* `queryString.parse({STRING})`：クエリ文字列を解析してオブジェクトにします。クエリ文字列の先頭にある `?`、`#`、`&` の文字は無視されます。
* `queryString.stringify({OBJECT})`：オブジェクトをクエリ文字列にします。

### [!DNL reactor-load-script]

`reactor-load-script` は、URL が指定されたときにスクリプトを読み込む関数です。スクリプトタグが作成され、ドキュメントの `head` ノード内に配置されます。[promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) が返されます。この値を使用して、スクリプトの読み込みの成功または失敗を判断できます。

```javascript
var loadScript = require('@adobe/reactor-load-script');
var url = 'http://code.jquery.com/jquery-3.1.1.js';
loadScript(url).then(function() {
  // Do something ...
})
```

### [!DNL reactor-promise]

`reactor-promise` は、ECMAScript 6 にネイティブの [Promise API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) を模倣するコンストラクタです。ネイティブの Promise API が使用可能な場合は、代わりに返されます。

```javascript
var Promise = require('@adobe/reactor-promise');
new Promise(function(resolve) {
  resolve();
}, function(err) {
  console.error(err);
});
```

### [!DNL reactor-window]

`reactor-window` は、[`Window`](https://developer.mozilla.org/ja-JP/docs/Web/API/Window) オブジェクトを表します。[`inject-loader`](https://www.npmjs.com/package/inject-loader) のようなユーティリティを使用した、モック `Window` オブジェクトの挿入テストが可能になるため、これはモジュールをテストする際に役立ちます。

```javascript
var window = require('@adobe/reactor-window');
console.log(window.document);
```
