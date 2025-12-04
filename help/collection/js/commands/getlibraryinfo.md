---
title: getLibraryInfo
description: 現在の Web SDK ライブラリのバージョンに関する情報を取得します。
exl-id: f2bc0185-71c9-4d77-b9d2-b777a41a20e5
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# `getLibraryInfo`

`getLibraryInfo` コマンドは、現在使用されている Web SDK ライブラリのバージョンに関する情報を提供します。 このコマンドを使用して、様々な web プロパティにデプロイする web SDKのバージョンをトラッキングできます。

設定済みの Web SDK インスタンスを呼び出す際に、`getLibraryInfo` コマンドを実行します。 このコマンドは、通常、入力されたオブジェクトを取得できるJavaScript promise とペアで使用されます。

```js
alloy("getLibraryInfo").then(function(result) {
  console.log(result.libraryInfo.version);
  console.log(result.libraryInfo.commands);
  console.log(result.libraryInfo.configs);
});
```

## 応答オブジェクト

このコマンドを使用して [ 応答を処理 ](command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`libraryInfo.commands`**：このバージョンの web SDKがサポートするコマンドの配列。
* **`libraryInfo.configs`**：このバージョンの web SDKがサポートする設定の配列。
* **`libraryInfo.version`**:Web SDK ライブラリのバージョン。

## Web SDK タグ拡張機能を使用したライブラリ情報

タグ拡張機能には、このコマンドを送信するためのインターフェイスはありません。 JavaScript ライブラリ構文に従って、カスタムコードエディターを使用します。

タグ拡張機能を使用してこのコマンドを実行すると、タグ拡張機能のバージョンとライブラリのバージョンの両方が含まれ、`+` 記号で連結されます。 Web SDK ライブラリのバージョンが最初に表示され、次にタグ拡張機能のバージョンが表示されます。
