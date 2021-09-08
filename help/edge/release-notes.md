---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK；リリースノート；
exl-id: efd4e866-6a27-4bd5-af83-4a97ca8adebd
source-git-commit: f5d3c5911357d4b76e4d38564bf637e2549469d6
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 4%

---

# リリースノート

## バージョン2.6.4 - 2021年9月8日

* `head`要素に適用されたHTML Adobe Targetアクションが`head`コンテンツ全体を置き換える問題を修正しました。 次に、`head`要素に適用されたHTMLアクションの設定が、HTMLを追加するように変更されます。

## バージョン2.6.3 - 2021年8月17日

* `configure`コマンドで解決されたプロミスによって、公開用でないオブジェクトが公開される問題を修正しました。

## バージョン2.6.2 - 2021年8月5日

* `result.decisions`プロパティにアクセスしていない場合でも、`result.decisions`の廃止に関する警告（`sendEvent`コマンドで提供）がコンソールに記録される問題を修正しました。 `result.decisions`プロパティにアクセスする際には警告は記録されませんが、プロパティは非推奨です。

## バージョン2.6.1 - 2021年7月30日

* パーソナライゼーションコンテンツを持たないシングルページアプリビュー用にパーソナライゼーションをレンダリングするとエラーが発生し、`sendEvent`コマンドから返されたプロミスが拒否される問題を修正しました。

## バージョン2.6.0 - 2021年7月28日

* Adobe Targetのレスポンストークンなど、 `sendEvent`で解決されたプロミスに、より多くのパーソナライゼーションコンテンツを提供します。 `sendEvent`コマンドが実行されると、プロミスが返され、サーバーから受信した情報を含む`result`オブジェクトを使用して解決されます。 以前は、この結果オブジェクトには`decisions`という名前のプロパティが含まれていました。 この`decisions`プロパティは非推奨です。 新しいプロパティ`propositions`が追加されました。 この新しいプロパティは、[レスポンストークン](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/accessing-response-tokens.html)など、より多くのパーソナライゼーションコンテンツにアクセスできるようにします。

## バージョン2.5.0 - 2021年6月

* リダイレクトパーソナライゼーションオファーのサポートを追加しました。
* 自動的に収集されたビューポートの幅と高さが負の値の場合、サーバーに送信されなくなります。
* `onBeforeEventSend`コールバックから`false`を返してイベントがキャンセルされた場合に、メッセージがログに記録されるようになりました。
* 単一のイベントを対象とする特定のXDMデータが複数のイベントにまたがって含まれる問題を修正しました。

## バージョン2.4.0 - 2021年3月

* これで、SDKをnpmパッケージ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=ja)として[インストールできます。
* [デフォルトの同意](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent)を設定する際の`out`オプションのサポートが追加され、同意が得られるまですべてのイベントが破棄されます（既存の`pending`オプションはイベントをキューに追加し、同意が得られると送信します）。
* [onBeforeEventSendコールバック](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#onbeforeeventsend)を使用して、イベントの送信を防ぐことができるようになりました。
* レンダリングまたはクリックされるパーソナライズされたコンテンツに関するイベントを送信する際に、 `meta.personalization`の代わりにXDMスキーマフィールドグループを使用するようになりました。
* [getIdentityコマンド](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html#retrieving-the-visitor-id)は、IDと共にエッジ領域IDを返すようになりました。
* サーバーから受信した警告とエラーは改善され、より適切に処理されました。
* [Adobeの同意2.0標準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard)のサポートを追加しました。
* 同意の環境設定を受け取ると、CMP、Platform Web SDK、Platform Edge Network間で最適化された統合のために、ハッシュ化されてローカルストレージに保存されます。 同意設定を収集する場合は、ページの読み込みごとに`setConsent`を呼び出すことをお勧めします。
* 2つの[監視フック](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)、`onCommandResolved`、`onCommandRejected`が追加されました。
* バグ修正：ユーザーが新しい単一ページのアプリビューに移動して元のビューに戻り、コンバージョンに適した要素をクリックした場合、パーソナライズ機能のインタラクション通知イベントに同じアクティビティに関する重複情報が含まれます。
* バグ修正：SDKから送信された最初のイベントの`documentUnloading`が`true`に設定されていた場合、[`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon)を使用してイベントが送信され、IDが確立されないことに関するエラーが発生します。

## バージョン2.3.0 - 2020年11月

* コンテンツセキュリティポリシーをより厳格にするために、nonceのサポートを追加しました。
* シングルページアプリケーションのパーソナライゼーションのサポートを追加しました。
* `window.console` APIを上書きする可能性のある、他のページ上のJavaScriptコードとの互換性を改善しました。
* バグ修正：`documentUnloading`が`true`に設定された場合や、リンクのクリックが自動的に追跡された場合は、`sendBeacon`が使用されていませんでした。
* バグ修正：アンカー要素にHTMLコンテンツが含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用の`message`プロパティを含むブラウザーエラーの一部が適切に処理されず、結果として異なるエラーがお客様に表示されていました。
* バグ修正：iframeのHTMLページが親ウィンドウのHTMLページとは異なるサブドメインからのものである場合、iframe内でSDKを実行するとエラーが発生します。

## バージョン2.2.0 - 2020年10月

* バグ修正：`idMigrationEnabled`が`true`の場合、オプトインオブジェクトはAlloyが呼び出しを行うのをブロックしていました。
* バグ修正：ちらつきの問題を防ぐために、パーソナライゼーションオファーを返す必要があるリクエストをAlloyに認識させます。

## バージョン2.1.0 - 2020年8月

* `syncIdentity`コマンドを削除し、`sendEvent`コマンドでこれらのIDを渡すことをサポートします。
* IAB 2.0 Consent Standardをサポートします。
* `setConsent`コマンドで追加のIDを渡す機能をサポートします。
* `sendEvent`コマンドでの`datasetId`の上書きをサポートします。
* サポート合金モニタ（[詳細](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)）
* 実装の詳細コンテキストデータで`environment: browser`を渡します。
