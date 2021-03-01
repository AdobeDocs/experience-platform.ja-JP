---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience PlatformWeb SDK；プラットフォームWeb SDK;Web SDK；リリースノート；
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 10%

---


# リリースノート

## バージョン 2.3.0

* より厳密なコンテンツセキュリティポリシーを許可するために、nonceのサポートを追加しました。
* シングルページアプリのパーソナライゼーションサポートを追加しました。
* `window.console` APIを上書きする可能性のある他のページ上のJavaScriptコードとの互換性を改善しました。
* バグ修正：`documentUnloading`が`true`に設定された場合や、リンクのクリックが自動的に追跡された場合は、`sendBeacon`が使用されていませんでした。
* バグ修正：アンカー要素にHTMLコンテンツが含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用の`message`プロパティを含むブラウザーのエラーが適切に処理されなかったため、お客様に表示されるエラーが異なりました。
* バグ修正：iframe内でSDKを実行すると、iframeのHTMLページが親ウィンドウのHTMLページとは異なるサブドメインからのものである場合に、エラーが発生します。

## バージョン 2.2.0

* バグ修正：`idMigrationEnabled`が`true`の場合、オプトインオブジェクトはAlyが呼び出しを行うのをブロックしていました。
* バグ修正：ちらつきの問題を防ぐために、パーソナライズオファーが返される必要がある要求をAllyに認識させます。

## バージョン 2.1.0

* `syncIdentity`コマンドを削除し、`sendEvent`コマンドにこれらのIDを渡すことをサポートします。
* IAB 2.0同意基準をサポートします。
* `setConsent`コマンドで追加のIDを渡す機能をサポートします。
* `sendEvent`コマンドで`datasetId`の上書きをサポートします。
* サポート合金モニタ（[詳細情報](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)）
* 実装の詳細コンテキストデータで`environment: browser`を渡します。
