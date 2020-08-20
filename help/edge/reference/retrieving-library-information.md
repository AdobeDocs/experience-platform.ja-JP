---
title: ライブラリ情報の取得
seo-title: Adobe Experience Platform Web SDK を使用したライブラリ情報の取得
description: Web　サイトに読み込まれたライブラリに関する情報を取得する方法について説明します
seo-description: Adobe Experience Cloud SDK　が自動的に収集する、Web　サイトに読み込まれたライブラリに関する情報を取得する方法について説明します
keywords: library; library information;getLibraryInfo;libraryInfo;
translation-type: tm+mt
source-git-commit: 8c256b010d5540ea0872fa7e660f71f2903bfb04
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 100%

---


# ライブラリ情報の取得

多くの場合、Web サイトに読み込んだライブラリの背後にある詳細にアクセスすると便利です。これをおこなうには、次のように、`getLibraryInfo` コマンドを実行します。

```js
alloy("getLibraryInfo").then(function(libraryInfo) {
  console.log(libraryInfo.version);
});
```

現在、指定された `libraryInfo` オブジェクトには次のプロパティが含まれています。

* `version`：読み込まれたライブラリのバージョンです。例えば、読み込まれるライブラリのバージョンが 1.0.0 の場合、値は `1.0.0` になります。