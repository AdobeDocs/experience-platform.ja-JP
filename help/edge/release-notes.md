---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK；リリースノート；
exl-id: efd4e866-6a27-4bd5-af83-4a97ca8adebd
source-git-commit: 3d8e21f76b23bac3ee88702c19cf6d6a6e25a3ae
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 3%

---

# リリースノート

## バージョン 2.9.0 - 2022 年 3 月 10 日

* トラッキングのサポートを追加しました [!DNL control (default)] Adobe Targetエクスペリエンス。
* 単一ページアプリケーションのビュー変更イベントを最適化しました。 パーソナライズされたエクスペリエンスがレンダリングされた際に、表示通知が変更表示イベントに含まれるようになりました。
* 次の場合のコンソール警告を削除しました。 `eventType` が存在する。
* 問題を修正しました。 `propositions` プロパティはからのみ返されました `sendEvent` コマンドを使用します。 この `propositions` プロパティは、常に配列として定義されるようになりました。
* Adobe Experience Edge からエラーが返された場合に、非表示のコンテナが表示されない問題を修正しました。
* インタラクションイベントがAdobe Targetでカウントされない問題を修正しました。 これは、XDM の web.webPageDetails.viewName にビュー名を追加することで修正されました。
* コンソールメッセージのドキュメントのリンク切れを修正しました。

## バージョン 2.8.0 - 2022 年 1 月 20 日

* パーソナライゼーションのシャドウ DOM セレクターをサポートします。
* パーソナライゼーションイベントタイプの名前を変更しました。 (`display` および `click` 取り込む `decisioning.propositionDisplay` および `decisioning.propositionInteract`)
* インラインスクリプトタグを含むHTMLオファーで、スクリプトが 1 回だけ実行された場合でも、スクリプトタグが 2 回ページに追加されていた問題を修正しました。

## バージョン 2.7.0 - 2021 年 10 月 27 日

* Experience Edge からの追加情報を、次の戻り値に表示します。 `sendEvent`を含む `inferences` および `destinations`. これらの機能は現在、ベータ版の一部として公開されているので、これらのプロパティの形式は変わる場合があります。 詳しくは、 [イベントの追跡。](fundamentals/tracking-events.md)

## バージョン 2.6.4 - 2021 年 9 月 8 日

* 「HTMLAdobe Target 」アクションが `head` 要素が `head` コンテンツ。 次に、 `head` 要素が追加HTMLに変更されました。

## バージョン 2.6.3 - 2021 年 8 月 17 日

* 公開を目的としていないオブジェクトが、 `configure` コマンドを使用します。

## バージョン 2.6.2 - 2021 年 8 月 4 日

* の廃止に関する警告が発生する問題を修正しました。 `result.decisions` ( `sendEvent` ) がコンソールに記録されます。 `result.decisions` プロパティにアクセスできませんでした。 次の項目にアクセスする際に警告がログに記録されない `result.decisions` プロパティを使用しますが、プロパティは非推奨です。

## バージョン 2.6.1 - 2021 年 7 月 30 日

* パーソナライゼーションコンテンツを持たない単一ページアプリ表示用にパーソナライゼーションをレンダリングすると、エラーが発生し、 `sendEvent` コマンドを使用して拒否します。

## バージョン 2.6.0 - 2021 年 7 月 27 日

* より多くのパーソナライゼーションコンテンツを `sendEvent` Adobe Targetのレスポンストークンを含む、解決されたプロミス。 次の場合に `sendEvent` コマンドが実行されると、promise が返され、最終的に `result` サーバーから受信した情報を含むオブジェクト。 以前は、この結果オブジェクトには `decisions`. この `decisions` プロパティは非推奨になりました。 新しいプロパティ `propositions`、が追加されました。 この新しいプロパティは、次のようなパーソナライゼーションコンテンツにアクセスできるようにします。 [レスポンストークン](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/accessing-response-tokens.html).

## バージョン 2.5.0 - 2021 年 6 月

* リダイレクトパーソナライゼーションオファーのサポートを追加しました。
* 自動的に収集されたビューポートの幅と高さが負の値の場合、サーバに送信されなくなります。
* イベントが `false` から `onBeforeEventSend` コールバックの場合、メッセージがログに記録されます。
* 単一のイベントを対象とした特定の XDM データが複数のイベントにわたって含まれていた問題を修正しました。

## バージョン 2.4.0 - 2021 年 3 月

* これで、SDK は [npm パッケージとしてインストールされる](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=ja).
* のサポートを追加しました。 `out` オプション [デフォルト同意の設定](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent)：同意が得られるまですべてのイベントを破棄します ( 既存の `pending` オプションはイベントをキューに追加し、同意を受け取ったら送信します )。
* この [onBeforeEventSend コールバック](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#onbeforeeventsend) を使用して、イベントが送信されるのを防ぐことができるようになりました。
* ではなく XDM スキーマフィールドグループを使用するようになりました。 `meta.personalization` パーソナライズされたコンテンツのレンダリングまたはクリックに関するイベントを送信する際。
* この [getIdentity コマンド](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html#retrieving-the-visitor-id) では、id と共にエッジ地域 ID を返すようになりました。
* サーバーから受け取った警告とエラーが改善され、より適切な方法で処理されました。
* のサポートを追加しました。 [Adobeの同意 2.0 標準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard).
* 同意設定を受け取ると、ハッシュ化されてローカルストレージに保存され、CMP、Platform Web SDK、Platform Edge Network 間で最適化された統合がおこなわれます。 同意設定を収集している場合は、 `setConsent` は、ページ読み込みのたびに
* 2 [フックの監視](https://github.com/adobe/alloy/wiki/Monitoring-Hooks), `onCommandResolved` および `onCommandRejected`、が追加されました。
* バグ修正：パーソナライゼーションのインタラクション通知イベントで、ユーザーが新しい単一ページのアプリビューに移動し、元のビューに戻って、コンバージョン条件を満たす要素をクリックした場合、同じアクティビティに関する重複情報が含まれます。
* バグ修正：SDK によって最初に送信されたイベントが `documentUnloading` に設定 `true`, [`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) がイベントの送信に使用され、id が確立されていないことに関するエラーが発生します。

## バージョン 2.3.0 - 2020 年 11 月

* より厳しいコンテンツセキュリティポリシーを可能にするために、nonce のサポートを追加しました。
* シングルページアプリケーションのパーソナライゼーションのサポートを追加しました。
* 上書きする可能性のある他のページ上の JavaScript コードとの互換性を改善しました `window.console` API
* バグ修正： `sendBeacon` は、 `documentUnloading` が `true` またはリンクのクリックが自動的に追跡されたタイミング。
* バグ修正：アンカー要素にHTMLの内容が含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用を含む特定のブラウザーエラー `message` プロパティが適切に処理されず、異なるエラーがお客様に表示されていました。
* バグ修正：iframe 内で SDK を実行すると、iframe のHTMLページが親ウィンドウのHTMLページとは異なるサブドメインからのものである場合に、エラーが発生します。

## バージョン 2.2.0 - 2020 年 10 月

* バグ修正：オプトインオブジェクトが、 `idMigrationEnabled` が `true`.
* バグ修正：ちらつきの問題を防ぐために、パーソナライゼーションオファーを返す必要があるリクエストを Alloy に認識させる。

## バージョン 2.1.0 - 2020 年 8 月

* を削除します。 `syncIdentity` コマンドを実行し、 `sendEvent` コマンドを使用します。
* IAB 2.0 Consent Standard をサポートします。
* 追加の ID を `setConsent` コマンドを使用します。
* の上書きのサポート `datasetId` 内 `sendEvent` コマンドを使用します。
* 支持合金モニタ ([詳細を表示](https://github.com/adobe/alloy/wiki/Monitoring-Hooks))
* パス `environment: browser` （実装の詳細のコンテキストデータ）。
