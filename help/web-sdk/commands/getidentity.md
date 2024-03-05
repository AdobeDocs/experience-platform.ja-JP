---
title: getIdentity
description: イベントデータを送信せずに、訪問者の ID を取得します。
source-git-commit: 90493d179e620604337bda96cb3b7f5401ca4a81
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 2%

---

# `getIdentity`

The `getIdentity` コマンドを使用すると、イベントデータを送信せずに訪問者 ID を取得できます。 実行時に、 `sendEvent` コマンドを使用すると、Web SDK は、訪問者の ID がまだ存在しない場合に、その ID を自動的に取得します。

訪問者 ID を生成してデータを送信するために別の呼び出しが必要な場合は、このコマンドを使用できます。

## Web SDK タグ拡張機能を使用した ID の取得

Web SDK タグ拡張は、タグ拡張 UI を通じてこのコマンドを提供しません。 JavaScript ライブラリ構文を使用して、カスタムコードエディターを使用します。

## Web SDK JavaScript ライブラリを使用した ID の取得

を実行します。 `getIdentity` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 このコマンドを設定する際には、次のオプションを使用できます。

* **`namespaces`**：名前空間の配列。 デフォルト値は `["ECID"]` です。有効な値は次のとおりです。 `["ECID"]`, `null`または `undefined`.
* **`edgeConfigOverrides`**: [datastream 設定オーバーライドオブジェクト](datastream-overrides.md).

```js
alloy("getIdentity",{
  "namespaces": ["ECID"]
});
```

## 応答オブジェクト

もしあなたが [応答を処理する](command-responses.md) このコマンドを使用すると、次のプロパティが応答オブジェクトで使用できます。

* **`identity.ECID`**：訪問者の ECID を含む文字列。
* **`edge.regionID`**:ID を取得した際にブラウザーがヒットした Edge ネットワーク領域を表す整数です。 これは、従来のロケーションヒントと同じAudience Managerです。
