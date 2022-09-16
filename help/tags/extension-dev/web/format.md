---
title: Web 拡張機能のライブラリモジュール
description: Adobe Experience Platform で Web 拡張機能のライブラリモジュールを書式設定する方法について説明します。
exl-id: 08f2bb01-9071-49c5-a0ff-47d592cc34a5
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 100%

---

# Web 拡張機能のライブラリモジュール

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

>[!IMPORTANT]
>
>このドキュメントでは、Web 拡張機能のライブラリモジュールの形式について説明します。エッジ拡張機能を開発する場合は、代わりに [エッジ拡張機能モジュールのフォーマット](../edge/format.md) に関するガイドを参照してください。

ライブラリモジュールは、拡張機能によって提供される再利用可能なコードで、Adobe Experience Platform のタグのランタイムライブラリ内で発行されます。 このライブラリは、クライアントの Web サイト上で実行されます。 例えば、`gesture` イベントタイプには、クライアントの Web サイト上で実行され、ユーザーのジェスチャーを検出するライブラリモジュールがあります。

ライブラリモジュールは、[CommonJS モジュール](https://nodejs.org/api/modules.html#modules-commonjs-modules)として構造化されています。CommonJS モジュール内では、次の変数を使用できます。

## [!DNL require]

`require` 関数を使用して、次のモジュールにアクセスできます。

1. タグで提供されるコアモジュール。 これらのモジュールには、`require('@adobe/reactor-name-of-module')` を使ってアクセスできます。詳しくは、利用可能な [コアモジュール](./core.md) のドキュメントを参照してください。
1. 拡張機能内の他のモジュール。拡張機能内のモジュールには、相対パスを介してアクセスできます。相対パスの先頭には `./` または `../` が必要です。

使用例：

```javascript
var cookie = require('@adobe/reactor-cookie');
cookie.set('foo', 'bar');
```

## [!DNL module]

`module` という名前の自由変数を使用できます。この変数を使って、モジュールの API をエクスポートできます。

使用例：

```javascript
module.exports = function(…) { … }
```

## [!DNL exports]

`exports` という名前の自由変数を使用できます。この変数を使って、モジュールの API をエクスポートできます。

使用例：

```javascript
exports.sayHello = function(…) { … }
```

これは `module.exports` の代わりに使用できますが、使い方は限られています。`module.exports` と `exports` の違い、および `exports` の使用に関する注意事項についての詳細は、[node.js の module.exports と exports についてのページ](https://www.sitepoint.com/understanding-module-exports-exports-node-js/)を参照してください。不確かな場合は、`exports` ではなく `module.exports` を使用してください。

## 実行とキャッシュ

タグのランタイムライブラリが実行されると、モジュールが直ちに「インストール」され、書き出しがキャッシュされます。次のようなモジュールであるとします。

```javascript
console.log('runs on startup');

module.exports = function(settings) {
  console.log('runs when necessary');
}
```

`runs on startup` が即座に記録される一方、`runs when necessary` は、書き出された関数がタグエンジンによって呼び出された場合にのみ記録されます。モジュールの目的によっては不要な場合がありますが、関数をエクスポートする前に必要な設定をおこなうことで、この機能を活用できます。
