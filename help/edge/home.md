---
title: Adobe Experience Platform Web SDK のヘルプ
seo-title: Adobe Experience Platform Web SDK のヘルプ
description: Adobe Experience Platform Web SDK の概要と、その使用方法を説明します。
seo-description: Adobe Experience Cloud のお客様が　Experience Cloud　の様々なサービスを利用できるようにします
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）Adobe Experience Platform Web SDK とは

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Web SDK は、Adobe Experience Cloud のお客様が Experience Cloud の様々なサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。タグを適切な順序で実行し、ライブラリのバージョン管理の課題との矛盾、依存関係の管理を改善し、課題を解決するために使用します。これは、Experience Cloud の新しい実装方法です。

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリとエンドポイントでは、ID および Target エクスペリエンスの取得、Audience Manager へのデータの送信、1 回の呼び出しでの Adobe Experience Platform へのデータの受け渡しが可能です。