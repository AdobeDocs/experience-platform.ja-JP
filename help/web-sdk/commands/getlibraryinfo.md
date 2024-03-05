---
title: getLibraryInfo
description: 現在の Web SDK ライブラリバージョンに関する情報を取得します。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# `getLibraryInfo`

The `getLibraryInfo` コマンドには、現在使用されている Web SDK ライブラリのバージョンに関する情報が表示されます。 このコマンドを使用して、様々な Web プロパティにデプロイする Web SDK のバージョンを追跡できます。

## Web SDK タグ拡張機能を使用するライブラリ情報

タグ拡張機能は、このコマンドを送信するためのインターフェイスを提供しません。 JavaScript ライブラリ構文に従って、カスタムコードエディターを使用します。

タグ拡張を使用してこのコマンドを実行すると、タグ拡張バージョンとライブラリバージョンの両方が含まれ、 `+` 記号。 Web SDK ライブラリのバージョンが最初に表示され、その後にタグ拡張のバージョンが表示されます。

## Web SDK JavaScript ライブラリを使用したライブラリ情報

を実行します。 `getLibraryInfo` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 このコマンドは、通常、入力されるオブジェクトを取得できる JavaScript プロミスと組み合わせられます。

```js
alloy("getLibraryInfo").then(function(result) {
  console.log(result.libraryInfo.version);
  console.log(result.libraryInfo.commands);
  console.log(result.libraryInfo.configs);
});
```

## 応答オブジェクト

もしあなたが [応答を処理する](command-responses.md) このコマンドを使用すると、次のプロパティが応答オブジェクトで使用できます。

* **`libraryInfo.commands`**：このバージョンの Web SDK がサポートするコマンドの配列。
* **`libraryInfo.configs`**：このバージョンの Web SDK がサポートする設定の配列。
* **`libraryInfo.version`**:Web SDK ライブラリのバージョン。
