---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience PlatformWeb SDK；プラットフォームWeb SDK;Web SDK；リリースノート；
translation-type: tm+mt
source-git-commit: b0e6d1f7cf7302bb3a7403bb18dfd8b7489d583e
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 6%

---


# リリースノート

## バージョン 2.4.0

* SDKをnpmパッケージ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html)として[インストールできるようになりました。
* [デフォルトの同意](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent)の設定時に、`out`オプションのサポートが追加され、同意が得られるまですべてのイベントが削除されます(既存の`pending`オプションはイベントをキューに入れ、同意が得られたら送信します)。
* [onBeforeEventSendコールバック](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#onbeforeeventsend)を使用して、イベントの送信を防ぐことができるようになりました。
* レンダリングまたはクリックされるパーソナライズされたコンテンツに関するイベントを送信する際に、`meta.personalization`の代わりにXDMミックスインを使用するようになりました。
* [getIdentityコマンド](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html#retrieving-the-visitor-id)は、IDと共にエッジ領域IDを返すようになりました。
* サーバーから受け取った警告およびエラーは改善され、より適切な方法で処理されました。
* [Adobeの同意2.0基準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard)のサポートを追加しました。
* 同意の環境設定を受け取ると、ハッシュ化されてローカルストレージに保存され、CMP、Platform Web SDK、Platform Edge Networkの間で最適化された統合を実現します。 同意の環境設定を収集する場合は、ページの読み込みごとに`setConsent`を呼び出すことをお勧めします。
* [監視フック](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)、`onCommandResolved`、`onCommandRejected`の2つが追加されました。
* バグ修正：パーソナライゼーションインタラクション通知イベントには、ユーザーが新しい単一ページのアプリ表示に移動し、元の表示に戻り、コンバージョンに該当する重複をクリックした場合に、同じアクティビティに関する情報が含まれます。
* バグ修正：SDKから送信された最初のイベントが`documentUnloading`を`true`に設定していた場合、[`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon)を使用してイベントが送信され、IDが確立されていないというエラーが発生します。

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
