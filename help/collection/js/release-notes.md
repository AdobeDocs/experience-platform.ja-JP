---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience Platform Web SDK;Experience Platform Web SDK;Web SDK；リリースノート；
exl-id: efd4e866-6a27-4bd5-af83-4a97ca8adebd
source-git-commit: dc333f30f9a2cb7cd485d1cb13272c078da0bd76
workflow-type: tm+mt
source-wordcount: '2585'
ht-degree: 57%

---


# Web SDK リリースノート

このドキュメントでは、Adobe Experience Platform Web SDK のリリースノートを示します。
Web SDK タグ拡張機能の最新のリリースノートについては、[Web SDK タグ拡張機能リリースノート](/help/tags/extensions/client/web-sdk/web-sdk-ext-release-notes.md)を参照してください。

## バージョン 2.30.0 - 2025年9月24日（PT）

**新機能**

- プッシュ通知の表示がサポートされるようになりました。

## バージョン 2.29.0 - 2025年9月4日（PT）

**新機能**

- Adobe ジャーニー分析用のAdobe広告データの収集がサポートされるようになりました
- ユーザーのプロファイルにプッシュ購読の詳細を記録できるようになりました。

**修正点および改善点**

- 設定の上書きセクションが置き換えられずに結合される問題を修正しました。
- リンクコレクションがドキュメントのコンテンツ全体をリンク名として送信していた場合を修正しました。
- 特定の提案を再レンダリングできない問題を修正しました。

## バージョン 2.28.1 - 2025年7月31日（PT）

**修正点および改善点**

- カスタムビルドが実行されない問題を修正しました。

## バージョン 2.28.0 - 2025年7月24日（PT）

**新機能**

- Adobe Journey Optimizerの欠格規則をサポートするようになりました。

**修正点および改善点**

- [Media Analytics トラッカー &#x200B;](commands/getmediaanalyticstracker.md) で、メディアオブジェクトの `length` プロパティが無効なデータタイプを誤って受け入れたエラーを修正しました。
- ID 検索が失敗した場合にプロミス却下を適切に処理する [ID 管理 &#x200B;](../use-cases/identity/id-overview.md) エラー処理を改善しました。
- HTML コンテンツ項目を含むパーソナライゼーションコンテンツが、見つからない `renderStatusHandler` に関するエラーでレンダリングに失敗する問題を修正しました。
- 非 HTTP URL を適切に処理するための Activity Map[URL コレクション &#x200B;](commands/configure/clickcollectionenabled.md) を修正しました。

**既知の問題**

- [&#x200B; を使用する &#x200B;](/help/collection/js/install/create-custom-build.md) カスタムビルド `npx @adobe/alloy` プロセスは、現在、バージョン 2.28.0 で期待どおりに機能していません。選択したモジュールに関係なく、すべてのコンポーネントが生成されたビルドに含まれます。 この問題は、CDN で使用可能な標準のJavaScript ファイルには影響しません。 修正中です。

## バージョン 2.27.0 - 2025年5月20日（PT）

**修正点および改善点**

- カスタムスタイルが正しく適用されなかったアプリ内メッセージの問題を修正しました。
- イベント履歴の形式を変更しました。 これにより、古い履歴データが削除されると、アプリ内メッセージとコンテンツカードが再表示されます。
- SPA のユースケースで提案が再適用される問題を修正しました。
- シャドウ DOM 要素のクリックの追跡に関する問題を修正しました。

## バージョン 2.26.0 - 2025年3月5日（PT）

**新機能**

- Web SDK NPM パッケージを使用して、カスタム Web SDK ビルドを作成し、必要なライブラリコンポーネントのみを選択できるようになりました。 これにより、ライブラリのサイズが縮小され、読み込み時間が最適化されます。 [NPM パッケージを使用してカスタム web SDK ビルドを作成する &#x200B;](install/create-custom-build.md) 方法については、ドキュメントを参照してください。
- [`getIdentity`](commands/getidentity.md) コマンドは、`kndctr` ID cookie から直接 ECID を自動的に読み取るようになりました。 `getIdentity` 名前空間を使用して `ECID` を呼び出すと、ID Cookie が既に存在する場合、web SDKは、ID を取得するためにEdge Networkに対してリクエストを実行しなくなりました。 これで、cookie から ID を読み取るようになりました。

**修正点および改善点**

- `getIdentity` 呼び出しを送信した後、`collect` コマンドが ID を返さない問題を修正しました。
- リダイレクトが発生する前に、パーソナライゼーションのリダイレクトによってコンテンツがちらつく問題を修正しました。

## バージョン 2.25.0 - 2025年1月23日（PT）

**修正点および改善点**

- `setDebug` コマンドにオプション検証を追加しました。
- クリックコレクションが無効な場合に、`onBeforeLinkClickSend` 関数またはダウンロードリンク修飾子を設定する際の警告を追加しました。
- レンダリングされた提案がディスプレイ通知に含まれていない問題を修正しました。

**新機能**

- サードパーティ cookie が有効になっていて、adobedc.demdex.netへのリクエストがブロックされている場合に、設定済みのEdge ドメインへのフォールバックを実装しました。

## バージョン 2.24.1 - 2024年12月6日（PT）

**修正点および改善点**

- 一部のお客様の統合でエラーが発生していた、[Adobe Experience Platform ルールエンジン &#x200B;](https://github.com/adobe/aepsdk-rulesengine-typescript/) に関連する依存関係の問題を解決しました。 Web SDKには、[Adobe Experience Platform ルールエンジン &#x200B;](https://github.com/adobe/aepsdk-rulesengine-typescript/) バージョン 2.0.3 以降が必要です。

## バージョン 2.24.0 - 2024年10月31日（PT）

**新機能**

- [&#x200B; データストリームの上書き &#x200B;](/help/datastreams/overrides.md) がメディアセッションの開始時にサポートされるようになりました。

- [`onContentRendering`](monitoring-hooks.md#onContentRendering)monitoring フックでAdobe Target応答トークンがサポートされるようになりました。

**修正点および改善点**

- 複数のアプリ内メッセージが返される場合は、優先度が最も高いメッセージのみが表示されます。 その他は抑制されたとおりに記録されます。
- 空のデータストリームの上書きはEdge Networkに送信されなくなり、サーバーサイドのルーティング設定との競合の可能性が減ります。
- 他のAdobe SDK と整合させるために、次のログメッセージコンポーネント名を変更しました。
   - `DecisioningEngine` の名前は `RulesEngine` に変更されました
   - `LegacyMediaAnalytics` の名前は `MediaAnalyticsBridge` に変更されました
   - `Privacy` の名前は `Consent` に変更されました
- デフォルトのコンテンツ項目が [`applyPropositions`](commands/applypropositions.md) を使用してレンダリングされる際に発生していたエラーを修正しました。
- Adobe Targetの移動およびサイズ変更アクションにおける CSS エラーを修正しました。
- 応答から `machineLearning` キー [`sendEvent`](commands/sendevent/overview.md) 削除しました。

## バージョン 2.23.0 - 2024年9月19日（PT）

**新機能**

- [getIdentity](/help/collection/use-cases/identity/id-overview.md) コマンドでの [CORE ID](commands/getidentity.md) のリクエストに対するサポートを追加しました。

**修正点および改善点**

- Web SDKをローカルで実行すると、cookie が正しく書き込まれない問題を修正しました。

## バージョン 2.22.0 - 2024年8月22日（PT）

**新機能**

- パーソナライゼーション監視フックのサポートを追加しました。

**修正点および改善点**

- Internet Explorer のサポートが削除され、ライブラリの gzip サイズが 9% 削減されました。
- `onInstanceConfigured` モニターフックが呼び出されたときに Activity Map のリンクの詳細が初期化されない問題を修正しました。
- Cookie の宛先が正しいパスに設定されない問題を修正しました。
- への問い合わせに関するお客様の問題を修正しました。
- `adobe_mc` パラメーターの無効な URL エンコーディングが原因で [sendEvent](commands/sendevent/overview.md) 呼び出しが失敗する問題を修正しました。

## バージョン 2.21.1 - 2024年7月18日（PT）

**修正点および改善点**

- NPM ライブラリを使用する際のビルドエラーを修正しました。

## バージョン 2.21.0 - 2024年7月16日（PT）

**新機能**

- 自動提案インタラクショントラッキングのサポートを追加しました。
- alloy.js ファイルを提供するカスタムビルドスクリプトを追加しました。
- ActivityMap とイベントのグループ化のサポートにより、クリックの収集が改善されました。

## バージョン 2.20.0 - 2024年5月21日（PT）

**新機能**

- [&#x200B; ストリーミングメディアコレクション &#x200B;](commands/configure/streamingmedia.md) のサポートを追加。

**修正点および改善点**

- 同意がオプトアウトされたときに、事前非表示スニペットによってデフォルトコンテンツが非表示になるバグを修正しました。

## バージョン 2.19.2 - 2024年1月10日（PT）

**修正点および改善点**

- ID エラーが他のエラーをマスクし、ID エラーを警告に変更していた問題を修正しました。
- `renderDecisions` が `false` に設定された top of page 呼び出しがある場合に、bottom of page 呼び出しが送信されない問題を修正しました。
- 複数の `adobe_mc` クエリ文字列パラメーターが存在する場合に、web SDKがクロスドメイン ID を読み取れない問題を修正しました。

## バージョン 2.19.1 - 2023年11月10日（PT）

**修正点および改善点**

- `sendEvent` の呼び出しから返された提案配列が常に空である問題を修正しました。

## バージョン 2.19.0 - 2023年11月1日（PT）

**新機能**

- Adobe Journey Optimizerからのアプリ内メッセージのレンダリングがサポートされるようになりました。
- [&#x200B; ページイベントの上部と下部 &#x200B;](../use-cases/personalization/top-bottom-page-events.md) のサポートを追加しました。
- [`defaultPersonalizationEnabled`](commands/sendevent/personalization.md) コマンドに `sendEvent` ページ全体の範囲とデフォルトサーフェスの要求を制御するオプションを追加しました。

**修正点および改善点**

- 複数のタイプのパーソナライゼーションをレンダリングする際に、パーソナライゼーションの表示イベントを組み合わせたものです。
- 単一ページアプリケーションビュー名で大文字と小文字が区別される問題を修正しました。
- シャドウ DOM のパーソナライズされたオファーセレクターの問題を修正しました。

## バージョン 2.18.0 - 2023年7月31日（PT）

**新機能**

- [&#128279;](/help/datastreams/overrides.md)データストリーム ID のコマンドごとの上書きのサポートを追加しました。

**修正点および改善点**

- ドメインがクエリの一部として含まれていることが原因で離脱リンクが適合しない問題を修正しました。
- Web SDK 設定では `edgeConfigId` が非推奨となり、`datastreamId` が優先されます。

## バージョン 2.17.0 - 2023年5月17日（PT）

**修正点および改善点**

- Web SDK は、[Data Integration Library（DIL）](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja)と同様に、Audience Manager cookie の宛先値をエンコードするようになりました。

## バージョン 2.16.0 - 2023年4月25日（PT）

**新機能**

- [データストリーム設定の上書き](/help/datastreams/overrides.md)のサポートを追加しました。

## バージョン 2.15.0 - 2023年3月30日（PT）

**新機能**

- [`onBeforeLinkClickSend`](commands/configure/onbeforelinkclicksend.md) リンククリックのコールバックのサポートを追加しました。
- Adobe Journey Optimizer のクリックの追跡に対するサポートを追加しました。

**修正点および改善点**

- リンクコレクションに、リンク名と訪問者の地域が含まれるようになりました。
- 失敗した URL 宛先のコンソールエラーを削除しました。

## バージョン 2.14.0 - 2023年1月25日（PT）

- （ベータ版）Adobe Journey Optimizer のサーフェスと提案のサポートを追加しました。

**修正点および改善点**

- コードが [!DNL at.js] とは別の場所に挿入される、Adobe Target VEC カスタムコードアクションの問題を修正しました。
- 一部のエッジの場合に「リファラー」ヘッダーが Edge Network へのリクエストに適切に設定されない問題を修正しました。
- [ユーザーエージェントクライアントヒント](../use-cases/client-hints.md)プロパティが間違ったタイプに設定される可能性がある問題を修正しました。
- `placeContext.localTime` がスキーマに一致しない問題を修正しました。

## バージョン 2.13.1 - 2022年10月13日（PT）

- 設定後に window.Visitor が定義されると訪問者の移行が機能しない問題を修正しました。 これは、Adobe タグを使用して実行する場合に特に問題になります。
- 一部の環境で `device.screenWidth` と `device.screenHeight` が文字列として入力される問題を修正しました。

## バージョン 2.13.0 - 2022年9月28日（PT）

**新機能**

- ページごとの完全移行のサポートを追加しました。 訪問者が at.js ページと Web SDK ページの間を移動しても、Adobe Target プロファイルが保持されるようになりました。
- [高エントロピーの User-Agent クライアントヒント](../use-cases/client-hints.md)の設定可能なサポートを追加しました。
- [`applyResponse`](commands/applyresponse.md) コマンドのサポートを追加。 これにより、[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) を介してハイブリッドパーソナライゼーションが可能になります。
- QA モードのリンクが複数のページで機能するようになりました。

**修正点および改善点**

- リンクトラッキングが無効になっている場合に、パーソナライゼーションのクリックの追跡指標が更新されなかった問題を修正しました。
- 不明なオプションが指定された場合に検証エラーをスローするようにコマンドを更新しました。
- 表示およびインタラクションのパーソナライゼーションイベントを自動的に送信する際に、`_experience.decisioning.propositionEventType` プロパティが入力されるようになりました。
- `getIdentity` コマンドに重複した名前空間の検証を追加しました。
- `sendEvent` コマンドの重複した決定範囲の検証を追加しました。

## バージョン 2.12.0 - 2022年6月29日（PT）

- URL の一部として `cluster` cookie の場所のヒントを使用するように、Edge Network へのリクエストを変更します。 これにより、セッション中に場所を変更したユーザー（VPN 経由やモバイルデバイスでの運転など）が同じエッジに到達し、同じパーソナライゼーションプロファイルを持つようになります。
- getLibraryInfo コマンド応答で設定された関数を文字列化します。

## バージョン 2.11.0 - 2022年6月13日（PT）

**新機能**

- モバイルアプリとモバイル web コンテンツの間、およびドメイン間で訪問者 ID を共有することで、パーソナライズされたエクスペリエンスをより正確に提供できるようになりました。詳しくは、[専用のドキュメント](../use-cases/identity/id-sharing.md)を参照してください。
- 分析指標を増分せずに、[!DNL Adobe Target] からシングルページアプリケーションに提案の配列をレンダリングまたは実行できるようになりました。これにより、レポートのエラーが減り、分析の精度が向上します。詳しくは、[専用のドキュメント](../use-cases/personalization/rendering-personalization-content.md)を参照してください。
- 使用可能なコマンドやインスタンスの最終的な設定など、`getLibraryInfo` コマンドに追加情報を追加しました。

**修正点および改善点**

- [!DNL HTTPS] ページで `sameSite="none"` と `secure` フラグを使用するように cookie 設定を更新しました。
- `eq` 疑似セレクターを使用すると、パーソナライズされたコンテンツが正しく適用されない問題を修正しました。
- `localTimezoneOffset` が Experience Platform の検証に失敗する可能性がある問題を修正しました。

## バージョン 2.10.1 - 2022年5月3日（PT）

- ID 同期とセグメント宛先に対して複数の永続的な iframe が作成される問題を修正しました。

## バージョン 2.10.0 - 2022年4月22日（PT）

- すべての ID 同期とセグメント宛先に永続的な iframe を使用します。
- 結合された指標の提案が `sendEvent` の結果で重複していた問題を修正しました。

## バージョン 2.9.0 - 2022年3月10日（PT）

- トラッキング [!DNL control (default)] Adobe Target エクスペリエンスのサポートを追加しました。
- 単一ページアプリケーションのビュー変更イベントを最適化しました。パーソナライズされたエクスペリエンスがレンダリングされる際に、表示通知がビュー変更イベントに含まれるようになりました。
- `eventType` が存在しない場合のコンソール警告を削除しました。
- エクスペリエンスがリクエストされた際、またはキャッシュから取得された際に、`propositions` プロパティが `sendEvent` コマンドからのみ返される問題を修正しました。 `propositions` プロパティは、常に配列として定義されるようになりました。
- Edge Networkからエラーが返された場合に、非表示のコンテナが表示されない問題を修正しました。
- 操作イベントが Adobe Target でカウントされない問題を修正しました。この問題は、web.webPageDetails.viewName でビュー名を XDM に追加することで修正しました。
- コンソールメッセージのドキュメントのリンク切れを修正します。

## バージョン 2.8.0 - 2022年1月19日（PT）

- パーソナライゼーション用のシャドウ DOM セレクターをサポートします。
- パーソナライゼーションイベントタイプの名前を変更しました。（`display` と `click` は、`decisioning.propositionDisplay` と `decisioning.propositionInteract` になります）
- スクリプトが 1 回しか実行されていないにも関わらず、インラインスクリプトタグを含む HTML オファーで、スクリプトタグがページに 2 回追加される問題を修正しました。

## バージョン 2.7.0 - 2021年10月26日（PT）

- `sendEvent` や `inferences` など、Edge Networkからの追加情報を `destinations` からの戻り値で公開します。 これらの機能は現在、Betaの一部としてロールアウトされているので、これらのプロパティの形式は変わる可能性があります。

## バージョン 2.6.4 - 2021年9月7日（PT）

- `head` 要素に適用された HTML Adobe Target を設定アクションが `head` コンテンツ全体を置き換えていた問題を修正しました。 `head` 要素に適用される HTML を設定アクションを、HTML を追加するように変更しました。

## バージョン 2.6.3 - 2021年8月16日（PT）

- `configure` コマンドから解決された promise を介して、公開を目的としていないオブジェクトが公開される問題を修正しました。

## バージョン 2.6.2 - 2021年8月4日（PT）

- `result.decisions` プロパティがアクセスされていない場合でも、`result.decisions`（`sendEvent` コマンドによって提供される）の非推奨（廃止予定）に関する警告がコンソールに記録される問題を修正しました。`result.decisions` プロパティにアクセスしても警告はログに記録されませんが、プロパティはまだ非推奨（廃止予定）です。

## バージョン 2.6.1 - 2021年7月29日（PT）

- パーソナライゼーションコンテンツを持たない単一ページアプリビューのパーソナライゼーションをレンダリングすると、エラーがスローされ、`sendEvent` コマンドから返される promise が拒否される問題を修正しました。

## バージョン 2.6.0 - 2021年7月27日（PT）

- Adobe Target 応答トークンなど、`sendEvent` で解決された promise でより多くのパーソナライゼーションコンテンツを提供します。`sendEvent` コマンドを実行すると、promise が返されます。これは、サーバーから受信した情報を含む `result` オブジェクトで最終的に解決されます。以前は、この結果オブジェクトには `decisions` という名前のプロパティが含まれていました。この `decisions` プロパティは、非推奨（廃止予定）です。新しいプロパティ `propositions` を追加しました。この新しいプロパティにより、顧客は応答トークンなど、より多くのパーソナライゼーションコンテンツにアクセスできます。

## バージョン 2.5.0 - 2021年6月

- リダイレクトパーソナライゼーションオファーのサポートを追加しました。
- 自動的に収集されたビューポートの幅と高さが負の値の場合、サーバーに送信されなくなりました。
- `onBeforeEventSend` コールバックから `false` を返すことによってイベントがキャンセルされると、メッセージがログに記録されるようになりました。
- 単一のイベントを対象とした特定の XDM データが複数のイベントにわたって含まれていた問題を修正しました。

## バージョン 2.4.0 - 2021年3月

- これで、SDKを [NPM パッケージ &#x200B;](install/npm.md) としてインストールできます。
- [デフォルトの同意を設定](commands/configure/defaultconsent.md)する際に、同意が得られるまですべてのイベントをドロップする `out` オプションのサポートを追加しました（既存の `pending` オプションは、同意が得られるとイベントをキューに入れ、送信します）。
- [`onBeforeEventSend`](commands/configure/onbeforeeventsend.md) コールバックを使用して、イベントが送信されないようにすることができるようになりました。
- レンダリングまたはクリックされたパーソナライズされたコンテンツに関するイベントを送信する際に、`meta.personalization` の代わりに XDM スキーマフィールドグループを使用するようになりました。
- [`getIdentity`](commands/getidentity.md) コマンドは、ID と共にエッジ地域 ID を返すようになりました。
- サーバーから受信した警告とエラーが改善され、より適切な方法で処理されます。
- [`setConsent`](commands/setconsent.md) コマンドにAdobeの同意 2.0 標準をサポートするようになりました。
- 同意環境設定を受信すると、CMP、Experience Platform Web SDKおよびExperience Platform Edge Network間の統合を最適化するために、ハッシュされてローカルストレージに保存されます。 同意環境設定を収集している場合は、ページが読み込まれるたびに `setConsent` を呼び出すことをお勧めします。
- `onCommandResolved` と `onCommandRejected` の 2 つの[モニタリングフック](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)を追加しました。
- バグ修正：パーソナライゼーションインタラクション通知イベントには、ユーザーが新しいシングルページアプリビューに移動し、元のビューに戻り、コンバージョンの対象となる要素をクリックした際に、同じアクティビティに関する重複した情報が含まれていました。
- バグ修正：SDK によって送信された最初のイベントで `documentUnloading` が `true` に設定されていた場合、[`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) を使用してイベントが送信され、ID が確立されていないというエラーが発生していました。

## バージョン 2.3.0 - 2020年11月

- より厳格なコンテンツセキュリティポリシーを可能にする nonce サポートを追加しました。
- シシングルページアプリケーションのパーソナライゼーションサポートを追加しました。
- `window.console` API を上書きする可能性のある他のページ上の JavaScript コードとの互換性を改善しました。
- バグ修正： `documentUnloading` が `true` に設定されている場合や、リンクのクリックが自動的に追跡されている場合、`sendBeacon` は使用されませんでした。
- バグ修正：アンカー要素に HTML コンテンツが含まれている場合、リンクは自動的に追跡されません。
- バグ修正：読み取り専用の `message` プロパティを含む特定のブラウザーエラーが適切に処理されなかったので、別のエラーがお客様に公開されていました。
- バグ修正：iframe の HTML ページが親ウィンドウの HTML ページとは異なるサブドメインにある場合、iframe 内で SDK を実行するとエラーが発生します。

## バージョン 2.2.0 - 2020年10月

- バグ修正：`idMigrationEnabled` が `true` 効な場合、オプトインオブジェクトが web SDKの呼び出しをブロックしていました。
- バグ修正：ちらつきの問題を防ぐために、パーソナライゼーションオファーを返す必要があるリクエストを Web SDKに認識させます。

## バージョン 2.1.0 - 2020年8月

- `syncIdentity` コマンドを削除し、`sendEvent` コマンドでこれらの ID を渡すことをサポートします。
- IAB 2.0 同意標準をサポートします。
- `setConsent` コマンドで追加の ID を渡すことをサポートします。
- `sendEvent` コマンドでの `datasetId` の上書きをサポートします。
- モニタリングフックのサポート（[&#x200B; 詳細を表示 &#x200B;](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)）
- 実装の詳細コンテキストデータで `environment: browser` を渡します。
