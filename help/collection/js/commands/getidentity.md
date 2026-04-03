---
title: getIdentity
description: イベントデータを送信せずに訪問者のIDを取得します。
exl-id: 28b99f62-14c4-4e52-a5c7-9f6fe9852a87
source-git-commit: b292b9243816b1eed7fd3939096ddc30d6be0606
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 1%

---

# `getIdentity`

[`sendEvent`](sendevent/overview.md) コマンドを実行すると、Web SDKは訪問者のIDがまだ存在しない場合に自動的に取得します。 `getIdentity` コマンドを使用すると、イベントデータを送信せずに訪問者IDを取得できます。 訪問者IDを生成してデータを送信するために個別の呼び出しが必要な場合は、このコマンドを使用できます。

>[!IMPORTANT]
>
>クライアントサイドでID情報が必要な場合は、`getIdentity`を使用します。 ECIDをXDMにマッピングする必要がある場合のみ、代わりに[ データ収集](/help/datastreams/data-prep.md)または[ タグ ECID アクセス ガイダンス ](/help/tags/extensions/client/web-sdk/accessing-the-ecid.md)を使用してください。

`getIdentity` コマンドは、次のフローを実行して`ECID`を取得します。

1. Web SDKを使用して、`getIdentity`または[`appendIdentityToUrl`](appendidentitytourl.md)のいずれかを呼び出します。
1. Web SDKは、同意情報が提供されるのを待ちます。
1. Web SDKは、呼び出しで`ECID`名前空間が要求されたかどうかを確認します。 デフォルトでは、`ECID`名前空間は常に含まれます。
1. Web SDKは`kndctr` Cookieを読み取り、その値が存在する場合は`ECID`として返します。 これは`ECID`値のみを返し、`regionId`は返しません。
1. `kndctr` ID Cookieが設定されていないか、`"CORE"`名前空間が要求された場合、Web SDKはEdge Networkにリクエストを行います。
1. Edge Networkは、`ECID`と`regionId` （および必要に応じて`CORE ID`）の両方を返します。

Web SDKの設定済みインスタンスを呼び出す際に、`getIdentity` コマンドを実行します。 このコマンドを設定する際には、次のオプションを使用できます。

* **`namespaces`**：名前空間の配列。 デフォルト値は `["ECID"]` です。サポートされているその他の値は次のとおりです。
   * `["CORE"]`
   * `["ECID","CORE"]`
   * `null`
   * `undefined`

  `"ECID"`と`"CORE ID"`を同時にリクエストできます。 例：`"namespaces": ["ECID","CORE"]`。

* **`edgeConfigOverrides`**: [ データストリーム設定がオブジェクト ](configure/edgeconfigoverrides.md)を上書きします。

```js
alloy("getIdentity",{
  // This command retrieves both ECID and CORE IDs
  "namespaces": ["ECID","CORE"] 
});
```

## 応答オブジェクト

このコマンドで[応答を処理](command-responses.md)する場合、応答オブジェクトで次のプロパティを使用できます。

* **`identity.ECID`**：訪問者のECIDを含む文字列。
* **`identity.CORE`**：訪問者のCORE IDを含む文字列。
* **`edge.regionID`**: IDの取得時にブラウザーがヒットしたEdge Network リージョンを表す整数。 これは、従来のAudience Managerの場所ヒントと同じです。

```js
// Get the visitor's ECID
alloy('getIdentity').then(result => {
  console.log(result.identity.ECID);
});
```

## Web SDK タグ拡張機能を使用したIDの取得

Web SDK タグ拡張機能では、タグ拡張機能UIを使用してこのコマンドを提供しません。 JavaScript ライブラリの構文を使用して、カスタムコードエディターを使用します。
