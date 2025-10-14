---
title: NPM パッケージを使用して web SDKをインストールします
description: NPM パッケージを使用して、Web SDK ライブラリをインストールおよび参照します。
exl-id: 4c70ec5d-33fd-4ef7-ba9e-5b92ff6c3e86
source-git-commit: 8b6c958613923127880263679ce00ce359151300
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# NPM パッケージを使用して web SDKをインストールします

Adobe Experience Platform Web SDKは、[NPM パッケージ &#x200B;](https://www.npmjs.com) として入手できます。 NPM パッケージをインストールすると、Adobe Experience Platform Web SDK JavaScript ライブラリのビルドプロセスを制御できます。 NPM パッケージは、ブラウザーで実行する EcmaScript バージョン 5 モジュールまたは EcmaScript バージョン 2015 （ES6）モジュールを公開します。

```bash
npm install @adobe/alloy
```

Adobe Experience Platform Web SDKの NPM パッケージは、`createInstance` 関数を公開します。 関数に渡される name オプションは、ログに使用されるプレフィックスを制御します。 パッケージの使用例を以下に示します。

## パッケージを ECMAScript 2015 （ES6）モジュールとして使用する

```js
import { createInstance } from "@adobe/alloy";
const alloy = createInstance({ name: "alloy" });
alloy("configure", { ... });
alloy("sendEvent", { ... });
```

>[!NOTE]
>
>NPM パッケージは CommonJS モジュールに依存しているので、バンドラーを使用する場合は、バンドラーが CommonJS モジュールをサポートしていることを確認します。 [Rollup](https://rollupjs.org) などの一部のバンドラーには、CommonJS のサポートを提供する [&#x200B; プラグイン &#x200B;](https://www.npmjs.com/package/@rollup/plugin-commonjs) が必要です。

## ECMAScript 5 モジュールとしてのパッケージの使用

```js
var alloyLibrary = require("@adobe/alloy");
var alloy = alloyLibrary.createInstance({ name: "alloy" });
alloy("configure", { ... });
alloy("sendEvent", { ... });
```
