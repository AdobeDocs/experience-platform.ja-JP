---
title: Adobe Experience Platform Web SDKヘルプ
seo-title: Adobe Experience Platform Web SDKヘルプ
description: Adobe Experience Platform Web SDKの概要と、その使用方法を説明します。
seo-description: adobe Experience Cloudのお客様がExperience Cloudの様々なサービスとやり取りすることを許可する
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）Adobe Experience Platform Web SDKとは

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Web SDKは、Adobe Experience Cloudのお客様がExperience Cloudの様々なサービスを操作できるようにする、クライアント側のJavaScriptライブラリです。

## Adobe Experience Platform Web SDKに置き換わるSDK

Adobe Experience Platform Web SDKは、次のSDKを置き換えます。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではありません。 完全な書き直しだ タグを適切な順序で実行し、ライブラリのバージョン管理の課題との矛盾、依存関係の管理を改善し、課題を解決するために使用します。 これは、Experience Cloudを実装する新しい方法です。

新しいライブラリに加えて、アドビのソリューションに対するHTTP要求を合理化する新しいエンドポイントが追加されました。 以前は、Visitor.jsは訪問者IDサービスに対してブロック呼び出しを送信し、AT.jsはAdobe Targetに呼び出しを送信し、DIL.jsはAdobe Audience Managerに呼び出しを送信し、最後にAppMeasurement.jsはAdobe Analyticsに呼び出しを送信していました。 この新しいライブラリとエンドポイントでは、IDの取得、Targetエクスペリエンスの取得、Audience Managerへのデータの送信、1回の呼び出しでのAdobe Experience Platformへのデータの受け渡しが可能です。