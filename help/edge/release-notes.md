---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK；リリースノート；
exl-id: efd4e866-6a27-4bd5-af83-4a97ca8adebd
source-git-commit: 7d7a9357f17b941a8f7800be86f211bb1276698d
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 4%

---

# リリースノート

## バージョン 2.7.0 - 2021 年 10 月 27 日

* Experience Edge からの追加情報を、 `sendEvent`を含む `inferences` および `destinations`. これらの機能は現在、ベータ版の一部として公開されているので、これらのプロパティの形式は変更される場合があります。 詳しくは、 [イベントの追跡。](fundamentals/tracking-events.md)

## バージョン 2.6.4 - 2021 年 9 月 8 日

* 「HTMLAdobe Target 」アクションを `head` 要素が全体を置き換えていた `head` コンテンツ 次に、 `head` 要素が追加HTMLに変更される。

## バージョン 2.6.3 - 2021 年 8 月 17 日

* 公開を目的としていないオブジェクトが、 `configure` コマンド

## バージョン 2.6.2 - 2021 年 8 月 5 日

* の廃止に関する警告が発生する問題を修正しました。 `result.decisions` ( `sendEvent` コマンド ) が、 `result.decisions` プロパティにアクセスできませんでした。 にアクセスする際に警告はログに記録されません `result.decisions` プロパティを使用する必要がありますが、プロパティは非推奨です。

## バージョン 2.6.1 - 2021 年 7 月 30 日

* パーソナライゼーションコンテンツを持たない単一ページのアプリ表示用にパーソナライゼーションをレンダリングするとエラーが発生し、 `sendEvent` コマンドを使用して、拒否されます。

## バージョン 2.6.0 - 2021 年 7 月 28 日

* より多くのパーソナライゼーションコンテンツを `sendEvent` Adobe Targetのレスポンストークンを含む、解決されたプロミス。 この `sendEvent` コマンドが実行されると、プロミスが返され、最終的に `result` サーバーから受信した情報を含むオブジェクト。 以前は、この結果オブジェクトには `decisions`. この `decisions` プロパティは非推奨です。 新しいプロパティ `propositions`、が追加されました。 この新しいプロパティを使用すると、 [レスポンストークン](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/accessing-response-tokens.html).

## バージョン 2.5.0 - 2021 年 6 月

* リダイレクトパーソナライゼーションオファーのサポートを追加しました。
* 自動的に収集されたビューポートの幅と高さが負の値の場合は、サーバーに送信されなくなります。
* イベントが `false` から `onBeforeEventSend` コールバックの場合、メッセージがログに記録されます。
* 単一のイベントを対象とする特定の XDM データが複数のイベントにまたがって含まれていた問題を修正しました。

## バージョン 2.4.0 - 2021 年 3 月

* これで、SDK は [npm パッケージとしてインストールされる](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=ja).
* のサポートを追加しました。 `out` オプション [デフォルトの同意の設定](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent)：同意が得られるまで、すべてのイベントを破棄します ( `pending` オプションはイベントをキューに追加し、同意を受け取るとイベントを送信します )。
* 10. [onBeforeEventSend コールバック](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#onbeforeeventsend) を使用して、イベントの送信を防ぐことができるようになりました。
* では、の代わりに XDM スキーマフィールドグループを使用するようになりました `meta.personalization` パーソナライズされたコンテンツのレンダリングまたはクリックに関するイベントを送信する際。
* 10. [getIdentity コマンド](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html#retrieving-the-visitor-id) は、id と共にエッジ地域 ID を返すようになりました。
* サーバーから受信した警告とエラーが改善され、より適切な方法で処理されました。
* のサポートを追加しました。 [Adobeの同意 2.0 標準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard).
* 同意の環境設定を受け取ると、ハッシュ化されてローカルストレージに保存され、CMP、Platform Web SDK、Platform Edge Network 間で最適化された統合が実現します。 同意設定を収集している場合は、 `setConsent` を設定します。
* 2 [フックの監視](https://github.com/adobe/alloy/wiki/Monitoring-Hooks), `onCommandResolved` および `onCommandRejected`、が追加されました。
* バグ修正：ユーザーが新しい単一ページのアプリビューに移動し、元のビューに戻って、コンバージョン条件を満たす要素をクリックした場合、パーソナライゼーションのインタラクション通知イベントに同じアクティビティに関する重複情報が含まれます。
* バグ修正：SDK によって送信された最初のイベントが `documentUnloading` に設定 `true`, [`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) がイベントの送信に使用され、id が確立されないことに関するエラーが発生します。

## バージョン 2.3.0 - 2020 年 11 月

* コンテンツセキュリティポリシーをより厳格にするために、nonce のサポートを追加しました。
* シングルページアプリケーションのパーソナライゼーションのサポートを追加しました。
* 上書きする可能性のある他のページ上の JavaScript コードとの互換性を改善しました `window.console` API
* バグ修正： `sendBeacon` が `documentUnloading` が `true` またはリンククリック数が自動的に追跡されたタイミング。
* バグ修正：アンカー要素にHTMLの内容が含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用を含む特定のブラウザーエラー `message` プロパティが適切に処理されず、異なるエラーがお客様に表示されていました。
* バグ修正：iframe 内で SDK を実行すると、iframe のHTMLページが親ウィンドウのHTMLページとは異なるサブドメインからのものである場合に、エラーが発生します。

## バージョン 2.2.0 - 2020 年 10 月

* バグ修正：オプトインオブジェクトは、 `idMigrationEnabled` が `true`.
* バグ修正：ちらつきの問題を防ぐために、パーソナライゼーションオファーを返す必要があるリクエストを Alloy に認識させます。

## バージョン 2.1.0 - 2020 年 8 月

* を削除します。 `syncIdentity` コマンドを使用して、 `sendEvent` コマンド
* IAB 2.0 Consent Standard をサポートします。
* 追加の ID を `setConsent` コマンド
* の上書きのサポート `datasetId` を `sendEvent` コマンド
* 支持合金モニタ ([詳細を表示](https://github.com/adobe/alloy/wiki/Monitoring-Hooks))
* パス `environment: browser` （実装の詳細のコンテキストデータ）。
