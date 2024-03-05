---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;リリースノート;
exl-id: efd4e866-6a27-4bd5-af83-4a97ca8adebd
source-git-commit: cf6df4d005486aa297dcced7c8811f87f5e988c2
workflow-type: tm+mt
source-wordcount: '1723'
ht-degree: 86%

---


# リリースノート

このドキュメントでは、Adobe Experience Platform Web SDK のリリースノートを示します。
Web SDK タグ拡張機能の最新のリリースノートについては、[Web SDK タグ拡張機能リリースノート](../tags/extensions/client/web-sdk/web-sdk-ext-release-notes.md)を参照してください。

## バージョン 2.19.2 - 2024年1月10日（PT）

**修正点および改善点**

* ID エラーで他のエラーがマスクされ、ID エラーが警告に変更される問題を修正しました。
* を使用したページ呼び出しのトップがある場合、ページの呼び出しの下部が送信されない問題を修正しました。 `renderDecisions` に設定 `false`.
* 複数のドメイン ID が存在する場合に Web SDK がクロスドメイン ID を読み取れなかった問題を修正しました `adobe_mc` クエリー文字列パラメーター。

## バージョン 2.19.1 - 2023年11月10日（PT）

**修正点および改善点**

* 提案配列が `sendEvent` 呼び出しは常に空でした。

## バージョン 2.19.0 - 2023年11月1日（PT）

**新機能**

* Adobe Journey Optimizerからのアプリ内メッセージのレンダリングのサポートを追加しました。
* のサポートを追加しました。 [ページイベントの上部と下部](use-cases/top-bottom-page-events.md).
* 追加済み [`defaultPersonalizationEnabled`](commands/sendevent/personalization.md) オプションを `sendEvent` コマンドを使用して、ページ全体の範囲とデフォルトのサーフェスのリクエストを制御します。

**修正点および改善点**

* 複数のタイプのパーソナライゼーションをレンダリングする場合、組み合わされたパーソナライゼーションの表示イベントが一緒に表示されます。
* 単一ページのアプリケーションビュー名で大文字と小文字が区別される問題を修正しました。
* シャドウ DOM でパーソナライズされたオファーセレクターの問題を修正しました。

## バージョン 2.18.0 - 2023年7月31日（PT）

**新機能**

* ](../datastreams/overrides.md)データストリーム ID のコマンドごとの上書き[のサポートを追加しました。

**修正点および改善点**

* ドメインがクエリの一部として含まれていることが原因で離脱リンクが適合しない問題を修正しました。
* Web SDK 設定では `edgeConfigId` が非推奨となり、`datastreamId` が優先されます。

## バージョン 2.17.0 - 2023年5月17日（PT）

**修正点および改善点**

* Web SDK は、[Data Integration Library（DIL）](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja)と同様に、Audience Manager cookie の宛先値をエンコードするようになりました。

## バージョン 2.16.0 - 2023年4月25日（PT）

**新機能**

* [データストリーム設定の上書き](../datastreams/overrides.md)のサポートを追加しました。

## バージョン 2.15.0 - 2023年3月30日（PT）

**新機能**

* [`onBeforeLinkClickSend`](/help/web-sdk/commands/configure/onbeforelinkclicksend.md) リンククリックのコールバックのサポートを追加しました。
* Adobe Journey Optimizer のクリックの追跡に対するサポートを追加しました。

**修正点および改善点**

* リンクコレクションに、リンク名と訪問者の地域が含まれるようになりました。
* 失敗した URL 宛先のコンソールエラーを削除しました。

## バージョン 2.14.0 - 2023年1月25日（PT）

* （ベータ版）Adobe Journey Optimizer のサーフェスと提案のサポートを追加しました。

**修正点および改善点**

* コードが [!DNL at.js] とは別の場所に挿入される、Adobe Target VEC カスタムコードアクションの問題を修正しました。
* 一部のエッジの場合に「リファラー」ヘッダーが Edge Network へのリクエストに適切に設定されない問題を修正しました。
* [ユーザーエージェントクライアントヒント](/help/web-sdk/use-cases/client-hints.md)プロパティが間違ったタイプに設定される可能性がある問題を修正しました。
* `placeContext.localTime` がスキーマに一致しない問題を修正しました。

## バージョン 2.13.1 - 2022年10月13日（PT）

* 設定後に window.Visitor が定義されると訪問者の移行が機能しない問題を修正しました。 これは、Adobe タグを使用して実行する場合に特に問題になります。
* 一部の環境で `device.screenWidth` と `device.screenHeight` が文字列として入力される問題を修正しました。

## バージョン 2.13.0 - 2022年9月28日（PT）

**新機能**

* [ページごとの完全移行](home.md#migrating-to-web-sdk)のサポートを追加しました。訪問者が at.js ページと Web SDK ページの間を移動しても、Adobe Target プロファイルが保持されるようになりました。
* [高エントロピーの User-Agent クライアントヒント](/help/web-sdk/use-cases/client-hints.md)の設定可能なサポートを追加しました。
* のサポートを追加しました。 [`applyResponse`](/help/web-sdk/commands/applyresponse.md) コマンドを使用します。 これにより、[Edge Network Server API](../server-api/overview.md) を使用してハイブリッドパーソナライゼーションが可能になります。
* QA モードのリンクが複数のページで機能するようになりました。

**修正点および改善点**

* リンクトラッキングが無効になっている場合に、パーソナライゼーションのクリックの追跡指標が更新されなかった問題を修正しました。
* 不明なオプションが指定された場合に検証エラーをスローするようにコマンドを更新しました。
* 表示およびインタラクションのパーソナライゼーションイベントを自動的に送信する際に、`_experience.decisioning.propositionEventType` プロパティが入力されるようになりました。
* `getIdentity` コマンドに重複した名前空間の検証を追加しました。
* `sendEvent` コマンドの重複した決定範囲の検証を追加しました。

## バージョン 2.12.0 - 2022年6月29日（PT）

* URL の一部として `cluster` cookie の場所のヒントを使用するように、Edge Network へのリクエストを変更します。 これにより、セッション中に場所を変更したユーザー（VPN 経由やモバイルデバイスでの運転など）が同じエッジに到達し、同じパーソナライゼーションプロファイルを持つようになります。
* getLibraryInfo コマンド応答で設定された関数を文字列化します。

## バージョン 2.11.0 - 2022年6月13日（PT）

**新機能**

* モバイルアプリとモバイル web コンテンツの間、およびドメイン間で訪問者 ID を共有することで、パーソナライズされたエクスペリエンスをより正確に提供できるようになりました。詳しくは、[専用のドキュメント](identity/id-sharing.md)を参照してください。
* 分析指標を増分せずに、[!DNL Adobe Target] からシングルページアプリケーションに提案の配列をレンダリングまたは実行できるようになりました。これにより、レポートのエラーが減り、分析の精度が向上します。詳しくは、[専用のドキュメント](personalization/rendering-personalization-content.md#applypropositions)を参照してください。
* 使用可能なコマンドやインスタンスの最終的な設定など、`getLibraryInfo` コマンドに追加情報を追加しました。

**修正点および改善点**

* [!DNL HTTPS] ページで `sameSite="none"` と `secure` フラグを使用するように cookie 設定を更新しました。
* `eq` 疑似セレクターを使用すると、パーソナライズされたコンテンツが正しく適用されない問題を修正しました。
* `localTimezoneOffset` が Experience Platform の検証に失敗する可能性がある問題を修正しました。

## バージョン 2.10.1 - 2022年5月3日（PT）

* ID 同期とセグメント宛先に対して複数の永続的な iframe が作成される問題を修正しました。

## バージョン 2.10.0 - 2022年4月22日（PT）

* すべての ID 同期とセグメント宛先に永続的な iframe を使用します。
* 結合された指標の提案が `sendEvent` の結果で重複していた問題を修正しました。

## バージョン 2.9.0 - 2022年3月10日（PT）

* トラッキング [!DNL control (default)] Adobe Target エクスペリエンスのサポートを追加しました。
* 単一ページアプリケーションのビュー変更イベントを最適化しました。パーソナライズされたエクスペリエンスがレンダリングされる際に、表示通知がビュー変更イベントに含まれるようになりました。
* `eventType` が存在しない場合のコンソール警告を削除しました。
* エクスペリエンスがリクエストされた際、またはキャッシュから取得された際に、`propositions` プロパティが `sendEvent` コマンドからのみ返される問題を修正しました。 `propositions` プロパティは、常に配列として定義されるようになりました。
* Edge ネットワークからエラーが返された場合に、非表示のコンテナが表示されない問題を修正しました。
* 操作イベントが Adobe Target でカウントされない問題を修正しました。この問題は、web.webPageDetails.viewName でビュー名を XDM に追加することで修正しました。
* コンソールメッセージのドキュメントのリンク切れを修正します。

## バージョン 2.8.0 - 2022年1月19日（PT）

* パーソナライゼーション用のシャドウ DOM セレクターをサポートします。
* パーソナライゼーションイベントタイプの名前を変更しました。（`display` と `click` は、`decisioning.propositionDisplay` と `decisioning.propositionInteract` になります）
* スクリプトが 1 回しか実行されていないにも関わらず、インラインスクリプトタグを含む HTML オファーで、スクリプトタグがページに 2 回追加される問題を修正しました。

## バージョン 2.7.0 - 2021年10月26日（PT）

* からの戻り値に Edge ネットワークからの追加情報を公開します。 `sendEvent`を含む `inferences` および `destinations`. これらの機能は現在、ベータ版の一部として公開されているので、これらのプロパティの形式は変わる場合があります。

## バージョン 2.6.4 - 2021年9月7日（PT）

* `head` 要素に適用された HTML Adobe Target を設定アクションが `head` コンテンツ全体を置き換えていた問題を修正しました。 `head` 要素に適用される HTML を設定アクションを、HTML を追加するように変更しました。

## バージョン 2.6.3 - 2021年8月16日（PT）

* `configure` コマンドから解決された promise を介して、公開を目的としていないオブジェクトが公開される問題を修正しました。

## バージョン 2.6.2 - 2021年8月4日（PT）

* `result.decisions` プロパティがアクセスされていない場合でも、`result.decisions`（`sendEvent` コマンドによって提供される）の非推奨（廃止予定）に関する警告がコンソールに記録される問題を修正しました。`result.decisions` プロパティにアクセスしても警告はログに記録されませんが、プロパティはまだ非推奨（廃止予定）です。

## バージョン 2.6.1 - 2021年7月29日（PT）

* パーソナライゼーションコンテンツを持たない単一ページアプリビューのパーソナライゼーションをレンダリングすると、エラーがスローされ、`sendEvent` コマンドから返される promise が拒否される問題を修正しました。

## バージョン 2.6.0 - 2021年7月27日（PT）

* Adobe Target 応答トークンなど、`sendEvent` で解決された promise でより多くのパーソナライゼーションコンテンツを提供します。`sendEvent` コマンドを実行すると、promise が返されます。これは、サーバーから受信した情報を含む `result` オブジェクトで最終的に解決されます。以前は、この結果オブジェクトには `decisions` という名前のプロパティが含まれていました。この `decisions` プロパティは、非推奨（廃止予定）です。新しいプロパティ `propositions` を追加しました。この新しいプロパティにより、顧客は[応答トークン](/help/web-sdk/personalization/adobe-target/accessing-response-tokens.md)など、より多くのパーソナライゼーションコンテンツにアクセスできます。

## バージョン 2.5.0 - 2021年6月

* リダイレクトパーソナライゼーションオファーのサポートを追加しました。
* 自動的に収集されたビューポートの幅と高さが負の値の場合、サーバーに送信されなくなりました。
* `onBeforeEventSend` コールバックから `false` を返すことによってイベントがキャンセルされると、メッセージがログに記録されるようになりました。
* 単一のイベントを対象とした特定の XDM データが複数のイベントにわたって含まれていた問題を修正しました。

## バージョン 2.4.0 - 2021年3月

* これで、SDK を [NPM パッケージ](/help/web-sdk/install/npm.md).
* [デフォルトの同意を設定](/help/web-sdk/commands/configure/defaultconsent.md)する際に、同意が得られるまですべてのイベントをドロップする `out` オプションのサポートを追加しました（既存の `pending` オプションは、同意が得られるとイベントをキューに入れ、送信します）。
* The [`onBeforeEventSend`](/help/web-sdk/commands/configure/onbeforeeventsend.md) コールバックを使用して、イベントが送信されないようにすることができるようになりました。
* レンダリングまたはクリックされたパーソナライズされたコンテンツに関するイベントを送信する際に、`meta.personalization` の代わりに XDM スキーマフィールドグループを使用するようになりました。
* The [`getIdentity`](/help/web-sdk/commands/getidentity.md) コマンドは、ID と共にエッジ地域 ID を返すようになりました。
* サーバーから受信した警告とエラーが改善され、より適切な方法で処理されます。
* Adobeの Consent 2.0 標準に対する [`setConsent`](/help/web-sdk/commands/setconsent.md) コマンドを使用します。
* 同意環境設定を受信すると、CMP、Platform Web SDK および Platform Edge Network 間の統合を最適化するために、ハッシュされてローカルストレージに保存されます。同意環境設定を収集している場合は、ページが読み込まれるたびに `setConsent` を呼び出すことをお勧めします。
* `onCommandResolved` と `onCommandRejected` の 2 つの[モニタリングフック](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)を追加しました。
* バグ修正：パーソナライゼーションインタラクション通知イベントには、ユーザーが新しいシングルページアプリビューに移動し、元のビューに戻り、コンバージョンの対象となる要素をクリックした際に、同じアクティビティに関する重複した情報が含まれていました。
* バグ修正：SDK によって送信された最初のイベントで `documentUnloading` が `true` に設定されていた場合、[`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) を使用してイベントが送信され、ID が確立されていないというエラーが発生していました。

## バージョン 2.3.0 - 2020年11月

* より厳格なコンテンツセキュリティポリシーを可能にする nonce サポートを追加しました。
* シシングルページアプリケーションのパーソナライゼーションサポートを追加しました。
* `window.console` API を上書きする可能性のある他のページ上の JavaScript コードとの互換性を改善しました。
* バグ修正： `documentUnloading` が `true` に設定されている場合や、リンクのクリックが自動的に追跡されている場合、`sendBeacon` は使用されませんでした。
* バグ修正：アンカー要素に HTML コンテンツが含まれている場合、リンクは自動的に追跡されません。
* バグ修正：読み取り専用の `message` プロパティを含む特定のブラウザーエラーが適切に処理されなかったので、別のエラーがお客様に公開されていました。
* バグ修正：iframe の HTML ページが親ウィンドウの HTML ページとは異なるサブドメインにある場合、iframe 内で SDK を実行するとエラーが発生します。

## バージョン 2.2.0 - 2020年10月

* バグ修正：`idMigrationEnabled` が `true` の場合、オプトインオブジェクトが Alloy の呼び出しをブロックしていました。
* バグ修正：ちらつきの問題を防ぐために、パーソナライズオファーを返す必要があるリクエストを Alloy に認識させます。

## バージョン 2.1.0 - 2020年8月

* `syncIdentity` コマンドを削除し、`sendEvent` コマンドでこれらの ID を渡すことをサポートします。
* IAB 2.0 同意標準をサポートします。
* `setConsent` コマンドで追加の ID を渡すことをサポートします。
* `sendEvent` コマンドでの `datasetId` の上書きをサポートします。
* Alloy モニターをサポートします（[詳細情報](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)）
* 実装の詳細コンテキストデータで `environment: browser` を渡します。
