---
title: getIdentity
description: イベントデータを送信せずに訪問者の ID を取得します。
exl-id: 28b99f62-14c4-4e52-a5c7-9f6fe9852a87
source-git-commit: aea46e3804d315c1237fc853540771f1b5c2b767
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 2%

---

# `getIdentity`

[`sendEvent`](sendevent/overview.md) コマンドを実行すると、訪問者の ID がまだ存在しない場合、Web SDKはその ID を自動的に取得します。 `getIdentity` コマンドを使用すると、イベントデータを送信せずに訪問者 ID を取得できます。 訪問者 ID を生成してデータを送信するために別々の呼び出しが必要な場合は、このコマンドを使用できます。

`getIdentity` コマンドは、`ECID` を取得するために次のフローを実行します。

1. Web SDKを使用して `getIdentity` または [`appendIdentityToUrl`](appendidentitytourl.md) を呼び出します。
1. Web SDKは、同意情報が提供されるのを待ちます。
1. Web SDKは、呼び出し時に `ECID` 名前空間がリクエストされたかどうかを確認します。 デフォルトでは、`ECID` 名前空間は常に含まれます。
1. Web SDKは `kndctr` cookie を読み取り、その値を `ECID` として返します（存在する場合）。 これは `ECID` 値のみを返し、`regionId` は返しません。
1. `kndctr` ID cookie が設定されていない場合や `"CORE"` 名前空間がリクエストされた場合、Web SDKはEdge Networkに対してリクエストを行います。
1. Edge Networkは、`ECID` と `regionId` （および要求された場合は `CORE ID`）の両方を返します。

設定済みの Web SDK インスタンスを呼び出す際に、`getIdentity` コマンドを実行します。 このコマンドを設定する際には、次のオプションを使用できます。

* **`namespaces`**：名前空間の配列。 デフォルト値は `["ECID"]` です。その他にサポートされている値は次のとおりです。
   * `["CORE"]`
   * `["ECID","CORE"]`
   * `null`
   * `undefined`

  `"ECID"` と `"CORE ID"` を同時にリクエストできます。 例：`"namespaces": ["ECID","CORE"]`。

* **`edgeConfigOverrides`**: [&#x200B; データストリーム設定の上書きオブジェクト &#x200B;](configure/edgeconfigoverrides.md)。

```js
alloy("getIdentity",{
  // This command retrieves both ECID and CORE IDs
  "namespaces": ["ECID","CORE"] 
});
```

## 応答オブジェクト

このコマンドを使用して [&#x200B; 応答を処理 &#x200B;](command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`identity.ECID`**：訪問者の ECID を含む文字列。
* **`identity.CORE`**：訪問者のコア ID を含む文字列。
* **`edge.regionID`**:ID を取得する際にブラウザーがヒットしたEdge Network リージョンを表す整数。 従来のAudience Managerの場所のヒントと同じです。

```js
// Get the visitor's ECID
alloy('getIdentity').then(result => {
  console.log(result.identity.ECID);
});
```

## Web SDK タグ拡張機能を使用した ID の取得

Web SDK タグ拡張機能では、タグ拡張機能 UI を通じてこのコマンドを提供しません。 JavaScript ライブラリ構文を使用したカスタムコードエディターを使用します。
