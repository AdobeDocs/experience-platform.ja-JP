---
title: getLibraryInfo
description: 現在の Web SDK ライブラリバージョンに関する情報を取得します。
exl-id: f2bc0185-71c9-4d77-b9d2-b777a41a20e5
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# `getLibraryInfo`

`getLibraryInfo` コマンドは、現在使用されている Web SDK ライブラリのバージョンに関する情報を提供します。 このコマンドを使用して、様々な web プロパティにデプロイする Web SDK のバージョンを追跡できます。

## Web SDK タグ拡張機能を使用したライブラリ情報

タグ拡張機能には、このコマンドを送信するためのインターフェイスはありません。 JavaScript ライブラリ構文に従って、カスタムコードエディターを使用します。

タグ拡張機能を使用してこのコマンドを実行すると、タグ拡張機能のバージョンとライブラリのバージョンの両方が含まれ、`+` 記号で連結されます。 Web SDK ライブラリのバージョンが最初に表示され、次にタグ拡張機能のバージョンが表示されます。

## Web SDK JavaScript ライブラリを使用したライブラリ情報

設定済みの Web SDK インスタンスを呼び出す際に、`getLibraryInfo` コマンドを実行します。 このコマンドは、通常、入力されたオブジェクトを取得できるJavaScript promise とペアで使用されます。

```js
alloy("getLibraryInfo").then(function(result) {
  console.log(result.libraryInfo.version);
  console.log(result.libraryInfo.commands);
  console.log(result.libraryInfo.configs);
});
```

## 応答オブジェクト

このコマンドを使用して [&#x200B; 応答を処理 &#x200B;](command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`libraryInfo.commands`**：このバージョンの Web SDK がサポートするコマンドの配列。
* **`libraryInfo.configs`**：このバージョンの Web SDK がサポートする設定の配列。
* **`libraryInfo.version`**:Web SDK ライブラリのバージョン。
