---
title: Adobe Experience PlatformウェブSDKリリースノート
seo-title: Adobe Experience PlatformウェブSDKリリースノート
description: Adobe Experience Platform Web SDK リリースノート。
seo-description: Adobe Experience Platform Web SDK リリースノート。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;release notes;
translation-type: tm+mt
source-git-commit: 77c1e693668bc50a81713d02cfe4b0fabc661404
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 8%

---


# リリースノート

## バージョン 2.3.0

* より厳密なコンテンツセキュリティポリシーを許可するために、nonceのサポートを追加しました。
* シングルページアプリのパーソナライゼーションサポートを追加しました。
* APIを上書きする可能性がある他のページ上のJavaScriptコードとの互換性を改善しました `window.console` 。
* バグ修正： `sendBeacon` が使用されていない問題を修正しま `documentUnloading``true` した。
* バグ修正：アンカー要素にHTMLコンテンツが含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用の `message` プロパティを含むブラウザーのエラーが適切に処理されなかったため、顧客に表示されるエラーが異なりました。
* バグ修正：iframe内でSDKを実行すると、iframeのHTMLページが親ウィンドウのHTMLページとは異なるサブドメインからのものである場合に、エラーが発生します。

## バージョン 2.2.0

* バグ修正：オプトインオブジェクトが、Allyがを呼び出すのをブロックし `idMigrationEnabled` ていました `true`。
* バグ修正：ちらつきの問題を防ぐために、パーソナライズオファーが返される必要がある要求をAllyに認識させます。

## バージョン 2.1.0

* コマンドを削除し、 `syncIdentity` コマンドにこれらのIDを渡すことをサポート `sendEvent` します。
* IAB 2.0同意基準をサポートします。
* コマンドに追加のIDを渡す機能をサポートし `setConsent` ます。
* コマンドでのの上書き `datasetId` をサポートし `sendEvent` ます。
* サポートアロイモニタ([詳細情報](https://github.com/adobe/alloy/wiki/Monitoring-Hooks))
* 実装 `environment: browser` の詳細コンテキストデータを渡します。
