---
title: ライブラリ情報の取得
seo-title: Adobe Experience Platform Web SDKを使用したライブラリ情報の取得
description: Webサイトに読み込まれたライブラリに関する情報を取得する方法を説明します。
seo-description: Adobe Experience Cloud SDKが自動的に収集するWebサイトに読み込んだライブラリに関する情報を取得する方法を説明します
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）ライブラリ情報の取得

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

多くの場合、Webサイトに読み込んだライブラリの背後にある詳細にアクセスすると便利です。 これを行うには、次のコマンド `getLibraryInfo` を実行します。

```js
alloy("getLibraryInfo").then(function(libraryInfo) {
  console.log(libraryInfo.version);
});
```

現在、指定されたオブジェクトに `libraryInfo` は次のプロパティが含まれています。

* `version` これは、読み込まれたライブラリのバージョンです。 例えば、読み込まれるライブラリのバージョンが1.0.0の場合、値は `1.0.0`.