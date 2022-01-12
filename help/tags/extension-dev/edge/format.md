---
title: エッジ拡張機能のライブラリモジュール
description: エッジプロパティのタグ拡張機能のライブラリモジュールの形式設定です。
exl-id: 82b98972-6fa2-4143-bcf4-c5dac1ca0e7f
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 98%

---

# エッジ拡張機能のライブラリモジュール

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

>[!IMPORTANT]
>
>このドキュメントでは、エッジ拡張のライブラリモジュールの形式について説明します。web 拡張機能を開発する場合は、 [web 拡張機能モジュールの形式](../web/format.md) に関するガイドを参照してください。

ライブラリモジュールは、拡張機能によって提供される再利用可能なコードで、Adobe Experience Platform（エッジノード上で実行されるライブラリ）のタグのランタイムライブラリ内で発行されます。例えば、`sendBeacon` アクションタイプには、エッジノード上で実行され、ビーコンを送信するライブラリモジュールがあります。

ライブラリモジュールは、[CommonJS モジュール](https://nodejs.org/api/modules.html#modules-commonjs-modules)として構造化されています。CommonJS モジュール内では、次の変数を使用できます。

## [!DNL require]

`require` 関数を使用して、拡張機能内のモジュールにアクセスできます。拡張機能内のモジュールには、相対パスを介してアクセスできます。相対パスの先頭には `./` または `../` が必要です。

使用例：

```js
var transformHelper = require('../helpers/transform');
transformHelper.execute({a: 'b'});
```

## [!DNL module]

`module` という名前の自由変数を使用できます。この変数を使って、モジュールの API をエクスポートできます。

使用例：

```js
module.exports = (…) => { … }
```

## [!DNL exports]

`exports` という名前の自由変数を使用できます。この変数を使って、モジュールの API をエクスポートできます。

使用例：

```js
exports.sayHello = (…) => { … }
```

これは `module.exports` の代わりに使用できますが、使い方は限られています。`module.exports` と `exports` の違い、および `exports` の使用に関する注意事項についての詳細は、[node.js の module.exports と exports についてのページ](https://www.sitepoint.com/understanding-module-exports-exports-node-js/)を参照してください。不確かな場合は、`exports` ではなく `module.exports` を使用してください。

## サーバーサイドモジュールの署名

拡張機能によって提供されるすべてのモジュールタイプ（データ要素、条件、またはアクション）は、同じパラメーター（[context](./context.md)）を使用して呼び出されます。

```js
exports.sayHello = (context) => { … }
```
