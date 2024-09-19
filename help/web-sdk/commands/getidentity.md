---
title: getIdentity
description: イベントデータを送信せずに訪問者の ID を取得します。
exl-id: 28b99f62-14c4-4e52-a5c7-9f6fe9852a87
source-git-commit: a884790aa48fb97eebe2421124fc5d5f76c8a79d
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 2%

---

# `getIdentity`

`getIdentity` コマンドを使用すると、イベントデータを送信せずに訪問者 ID を取得できます。 `sendEvent` コマンドを実行すると、訪問者の ID がまだ存在しない場合、Web SDK はその ID を自動的に取得します。

訪問者 ID を生成してデータを送信するために別々の呼び出しが必要な場合は、このコマンドを使用できます。

## Web SDK タグ拡張機能を使用した ID の取得

Web SDK タグ拡張機能では、タグ拡張機能 UI を通じてこのコマンドを提供しません。 JavaScript ライブラリ構文を使用したカスタムコードエディターを使用します。

## Web SDK JavaScript ライブラリを使用した ID の取得

設定済みの Web SDK インスタンスを呼び出す際に、`getIdentity` コマンドを実行します。 このコマンドを設定する際には、次のオプションを使用できます。

* **`namespaces`**：名前空間の配列。 デフォルト値は `["ECID"]` です。その他にサポートされている値：`["CORE"]`、`null`、`undefined`。 [!DNL ECID] と [!DNL CORE ID] を同時にリクエストできます。 例：`"namespaces": ["ECID","CORE"]`。
* **`edgeConfigOverrides`**: [ データストリーム設定の上書きオブジェクト ](datastream-overrides.md)。

```js
alloy("getIdentity",{
  "namespaces": ["ECID","CORE"] //this command retrieves both ECID and CORE IDs.
});
```

## 応答オブジェクト

このコマンドを使用して [ 応答を処理 ](command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`identity.ECID`**：訪問者の ECID を含む文字列。
* **`identity.CORE`**：訪問者のコア ID を含む文字列。
* **`edge.regionID`**:ID を取得する際にブラウザーがヒットしたEdge Network領域を表す整数。 これは、従来のAudience Managerの場所のヒントと同じです。
