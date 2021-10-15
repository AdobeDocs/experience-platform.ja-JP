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

## バージョン 2.6.4 - 2021 年 9 月 8 日

* `head` 要素に適用したHTMLAdobe Targetのアクションが `head` コンテンツ全体を置き換える問題を修正しました。 次に、`head` 要素に適用した設定HTMLのアクションが、追加HTMLに変更されます。

## バージョン 2.6.3 - 2021 年 8 月 17 日

* 公開を目的としていないオブジェクトが、`configure` コマンドから解決されたプロミスで公開される問題を修正しました。

## バージョン 2.6.2 - 2021 年 8 月 5 日

* `result.decisions` プロパティにアクセスしていない場合でも、`result.decisions` の廃止に関する警告（`sendEvent` コマンドで提供）がコンソールに記録される問題を修正しました。 `result.decisions` プロパティにアクセスする際に警告は記録されませんが、プロパティは非推奨です。

## バージョン 2.6.1 - 2021 年 7 月 30 日

* パーソナライゼーションコンテンツのない単一ページのアプリ表示用にパーソナライゼーションをレンダリングするとエラーが発生し、`sendEvent` コマンドから返されたプロミスが拒否される問題を修正しました。

## バージョン 2.6.0 - 2021 年 7 月 28 日

* `sendEvent` で解決されたプロミスに、Adobe Targetのレスポンストークンなど、より多くのパーソナライゼーションコンテンツを提供します。 `sendEvent` コマンドを実行すると、プロミスが返され、最終的にはサーバから受け取った情報を含む `result` オブジェクトで解決されます。 以前は、この結果オブジェクトには `decisions` という名前のプロパティが含まれていました。 この `decisions` プロパティは非推奨です。 新しいプロパティ `propositions` が追加されました。 この新しいプロパティを使用すると、[ レスポンストークン ](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/accessing-response-tokens.html) など、より多くのパーソナライゼーションコンテンツにアクセスできます。

## バージョン 2.5.0 - 2021 年 6 月

* リダイレクトパーソナライゼーションオファーのサポートを追加しました。
* 自動的に収集されたビューポートの幅と高さが負の値の場合は、サーバーに送信されなくなります。
* `onBeforeEventSend` コールバックから `false` を返してイベントがキャンセルされた場合、メッセージがログに記録されるようになりました。
* 単一のイベントを対象とする特定の XDM データが複数のイベントにまたがって含まれていた問題を修正しました。

## バージョン 2.4.0 - 2021 年 3 月

* これで、SDK を npm パッケージ ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=ja) としてインストールできるようになりました。[
* [ デフォルトの同意 ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent) を設定する際に、`out` オプションがサポートされるようになりました。このオプションは、同意が得られるまですべてのイベントを破棄します（既存の `pending` オプションはイベントをキューに入れ、同意が得られると送信します）。
* [onBeforeEventSend コールバック ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#onbeforeeventsend) を使用して、イベントの送信を防ぐことができるようになりました。
* レンダリングやクリックがおこなわれるパーソナライズされたコンテンツに関するイベントを送信する際に、 `meta.personalization` の代わりに XDM スキーマフィールドグループを使用するようになりました。
* [getIdentity コマンド ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html#retrieving-the-visitor-id) は、ID と共にエッジ領域 ID を返すようになりました。
* サーバーから受信した警告とエラーが改善され、より適切な方法で処理されました。
* [Adobeの同意 2.0 標準 ](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard) のサポートを追加しました。
* 同意の環境設定を受け取ると、ハッシュ化されてローカルストレージに保存され、CMP、Platform Web SDK、Platform Edge Network 間で最適化された統合が実現します。 同意設定を収集する場合は、ページの読み込みごとに `setConsent` を呼び出すことをお勧めします。
* 2 つの [ 監視フック ](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)、`onCommandResolved`、`onCommandRejected` が追加されました。
* バグ修正：ユーザーが新しい単一ページのアプリビューに移動し、元のビューに戻って、コンバージョン条件を満たす要素をクリックした場合、パーソナライゼーションのインタラクション通知イベントに同じアクティビティに関する重複情報が含まれます。
* バグ修正：SDK から送信された最初のイベントの `documentUnloading` が `true` に設定されていた場合、[`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) を使用してイベントが送信され、ID が確立されないことに関するエラーが発生します。

## バージョン 2.3.0 - 2020 年 11 月

* コンテンツセキュリティポリシーをより厳格にするために、nonce のサポートを追加しました。
* シングルページアプリケーションのパーソナライゼーションのサポートを追加しました。
* `window.console` API を上書きする可能性のある他のページ上の JavaScript コードとの互換性を改善しました。
* バグ修正：`documentUnloading` が `true` に設定された場合や、リンクのクリックが自動的に追跡された場合は、`sendBeacon` が使用されていませんでした。
* バグ修正：アンカー要素にHTMLの内容が含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用の `message` プロパティを含むブラウザーエラーの一部が適切に処理されず、異なるエラーがお客様に表示されていました。
* バグ修正：iframe 内で SDK を実行すると、iframe のHTMLページが親ウィンドウのHTMLページとは異なるサブドメインからのものである場合に、エラーが発生します。

## バージョン 2.2.0 - 2020 年 10 月

* バグ修正：`idMigrationEnabled` が `true` の場合、オプトインオブジェクトは Alloy が呼び出しをおこなうのをブロックしていました。
* バグ修正：ちらつきの問題を防ぐために、パーソナライゼーションオファーを返す必要があるリクエストを Alloy に認識させます。

## バージョン 2.1.0 - 2020 年 8 月

* `syncIdentity` コマンドを削除し、`sendEvent` コマンドでこれらの ID を渡すことをサポートします。
* IAB 2.0 Consent Standard をサポートします。
* `setConsent` コマンドで追加の ID を渡す機能をサポートしました。
* `sendEvent` コマンドで `datasetId` の上書きをサポートします。
* サポートアロイモニタ（[ 詳細 ](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)）
* 実装の詳細コンテキストデータで `environment: browser` を渡します。
