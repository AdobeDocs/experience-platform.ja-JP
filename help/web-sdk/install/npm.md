---
title: NPM パッケージを使用した Web SDK のインストール
description: NPM パッケージを使用して、Web SDK ライブラリをインストールし、参照します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---


# NPM パッケージを使用した Web SDK のインストール

Adobe Experience Platform Web SDK は、 [NPM パッケージ](https://www.npmjs.com). NPM パッケージをインストールすると、Adobe Experience Platform Web SDK JavaScript ライブラリのビルドプロセスを制御できます。 NPM パッケージは、ブラウザーで実行する EcmaScript バージョン 5 モジュールまたは EcmaScript バージョン 2015(ES6) モジュールを公開します。

```bash
npm install @adobe/alloy
```

Adobe Experience Platform Web SDK の NPM パッケージは、 `createInstance` 関数に置き換えます。 関数に渡される name オプションは、ログに記録されるプレフィックスを制御します。 パッケージの使用例を以下に示します。

## パッケージを ECMAScript 2015(ES6) モジュールとして使用

```js
import { createInstance } from "@adobe/alloy";
const alloy = createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

>[!NOTE]
>
>NPM パッケージは CommonJS モジュールに依存しているので、バンドラーを使用する場合は、バンドラーが CommonJS モジュールをサポートしていることを確認してください。 一部のバンドラー（例： ） [ロールアップ](https://rollupjs.org)、が必要 [プラグイン](https://www.npmjs.com/package/@rollup/plugin-commonjs) を使用すれば、CommonJS のサポートを提供できます。

## ECMAScript 5 モジュールとしてのパッケージの使用

```js
var alloyLibrary = require("@adobe/alloy");
var alloy = alloyLibrary.createInstance({ name: "alloy" });
alloy("config", { ... });
alloy("sendEvent", { ... });
```
