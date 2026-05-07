---
title: Adobe Experience Platform Web SDK リリースノート
description: Adobe Experience Platform Web SDK の最新のリリースノートです。
keywords: Adobe Experience Platform Web SDK;Experience Platform Web SDK;Web SDK；リリースノート；
exl-id: efd4e866-6a27-4bd5-af83-4a97ca8adebd
source-git-commit: a8a466778b74e6f64d258f759a36e1a4361f0a6b
workflow-type: tm+mt
source-wordcount: '2988'
ht-degree: 51%

---


# Web SDK リリースノート

このドキュメントでは、Adobe Experience Platform Web SDK のリリースノートを示します。
Web SDK タグ拡張機能の最新のリリースノートについては、[Web SDK タグ拡張機能リリースノート](/help/tags/extensions/client/web-sdk/web-sdk-ext-release-notes.md)を参照してください。

## バージョン 2.33.1 - 2026年5月7日（PT）

- コンテキストなどの必要なコンポーネントがSDK バンドルから除外され、イベントが送信されない問題を修正しました。

## バージョン 2.33.0 - 2026年5月7日（PT）

- ブリッジメディア分析adBreak、Chapter、およびQoE イベントでAPI エラーが返される問題を修正しました。
- Advertisingが設定されているときに、送信エクスペリエンスイベントにAdobe Advertising `stitchId`を追加しました。
- ID宛先処理のブロックを削除することで、`sendEvent` コマンドのパフォーマンスが向上しました。
- Advertisingが設定されていなくても、Advertising ID解決がサードパーティのスクリプトとiframeを読み込む問題を修正しました。
- ハッシュから`adobe_mc` ID転送パラメーターを読み取る機能を追加しました（以前はクエリパラメーター内だけでした）。
- URLが複数回エンコードされたときに`adobe_mc`を読み取れなかった問題を修正しました。
- すべての送信Brand Concierge イベントにXDMを含めます。

## バージョン 2.32.0 - 2026年3月23日（PT）

- 共有コアユーティリティは、拡張機能と統合で使用するためのスタンドアロンのnpm パッケージ（[@adobe/alloy-core](https://www.npmjs.com/package/@adobe/alloy-core)）として公開されるようになりました。
- `placeContext`が[`context`](/help/collection/js/commands/configure/context.md)構成変数に含まれている場合、XDM フィールド `xdm.placeContext.ianaTimezone`にIANA タイムゾーンが含まれるようになりました。
- Brand Concierge: [`stickyConversationSession`](/help/collection/js/commands/configure/conversation.md)が無効になっている場合のセッション IDの問題を修正しました。

## バージョン 2.31.1 - 2026年2月11日（PT）

- URLに複数の広告関連パラメーター`s_kwcid`または`ef_id`がある場合にWeb SDKがクラッシュする問題を修正しました。
- Advertising データが送信され、同意が与えられる前にCookieが作成される問題を修正しました。
- Safariで、Brand Concierge ストリームが正しく解析されない問題を修正しました。

## バージョン 2.31.0 - 2026年2月9日（PT）

**新機能**

- 文字列の[`context`](commands/configure/context.md)配列に`"oneTimeAnalyticsReferrer"`の可用性を追加しました。
- Brand Concierge コンポーネントを追加しました。
- イベントの作成から送信時間までの時間を記録するために、`meta.queueTimeMillis`をネットワークリクエストに追加しました。
- ID マップを保持して、後続の呼び出しで入力できるようにする機能。

**修正点および改善点**

- `aria-label`および`name`属性は、[自動リンク収集](commands/configure/clickcollectionenabled.md)で考慮されるようになりました。
- カスタムコードアクションが1回しか実行されない問題を修正しました。

## バージョン 2.30.0 - 2025年9月24日（PT）

**新機能**

- プッシュ通知の表示に対応しました。

## バージョン 2.29.0 - 2025年9月4日（PT）

**新機能**

- Adobe ジャーニーアナリティクス用のAdobe広告データの収集のサポートが追加されました
- ユーザーのプロファイルにプッシュサブスクリプションの詳細を記録するためのサポートを追加しました。

**修正点および改善点**

- 設定の上書きセクションが置き換えられる代わりに結合される問題を修正しました。
- リンク収集がドキュメント全体のコンテンツをリンク名として送信するケースを修正しました。
- 特定の提案を再レンダリングできない問題を修正しました。

## バージョン 2.28.1 - 2025年7月31日（PT）

**修正点および改善点**

- カスタムビルドを実行できない問題を修正しました。

## バージョン 2.28.0 - 2025年7月24日（PT）

**新機能**

- Adobe Journey Optimizer失格ルールのサポートを追加しました。

**修正点および改善点**

- メディアオブジェクトの`length` プロパティが無効なデータタイプを誤って受け入れた[Media Analytics トラッカー](commands/getmediaanalyticstracker.md)のエラーを修正しました。
- IDの検索が失敗した場合にPromiseの拒否を適切に処理するために、[ID管理](../identity/overview.md)のエラー処理を改善しました。
- HTML コンテンツアイテムを含むパーソナライゼーションコンテンツのレンダリングが失敗し、見つからない`renderStatusHandler`に関連するエラーが発生する問題を解決しました。
- 非HTTP URLを適切に処理するために、アクティビティマップ [URL コレクション ](commands/configure/clickcollectionenabled.md)を修正しました。

**既知の問題**

- `npx @adobe/alloy`を使用する[ カスタムビルド ](/help/collection/js/install/create-custom-build.md) プロセスは、現在、バージョン 2.28.0で期待どおりに機能していません。 選択したモジュールに関係なく、すべてのコンポーネントが生成されたビルドに含まれます。 この問題は、CDNで使用可能な標準のJavaScript ファイルには影響しません。 修正が進行中です。

## バージョン 2.27.0 - 2025年5月20日（PT）

**修正点および改善点**

- カスタムスタイルが正しく適用されなかったアプリ内メッセージの問題を修正しました。
- イベント履歴の形式を変更しました。 これにより、古い履歴データが削除されると、アプリ内メッセージやコンテンツカードが再表示されます。
- SPA ユースケースで提案が再適用される問題を修正しました。
- シャドウ DOM要素でのクリックトラッキングの問題を修正しました。

## バージョン 2.26.0 - 2025年3月5日（PT）

**新機能**

- Web SDK NPM パッケージを使用して、カスタム Web SDK ビルドを作成し、必要なライブラリコンポーネントのみを選択できるようになりました。 これにより、ライブラリのサイズを削減し、読み込み時間を最適化できます。 NPM パッケージ ](install/create-custom-build.md)を使用してカスタム Web SDK ビルドを[作成する方法については、ドキュメントを参照してください。
- [`getIdentity`](commands/getidentity.md) コマンドは、`kndctr` ID CookieからECIDを直接自動的に読み取るようになりました。 `getIdentity`を`ECID`名前空間で呼び出し、既にID Cookieが存在する場合、Web SDKはIDを取得するためのEdge Networkへのリクエストを行わなくなります。 これで、CookieからIDを読み取ります。

**修正点および改善点**

- `collect`呼び出しが送信された後、`getIdentity` コマンドがIDを返さない問題を修正しました。
- パーソナライゼーションリダイレクトによって、リダイレクトが発生する前にコンテンツのちらつきが発生する問題を修正しました。

## バージョン 2.25.0 - 2025年1月23日（PT）

**修正および改善**

- オプションの検証を`setDebug` コマンドに追加しました。
- クリック収集が無効になっている場合に、`onBeforeLinkClickSend`関数またはダウンロードリンク修飾子を設定する際に警告を追加しました。
- レンダリングされた提案が表示通知に含まれない問題を修正しました。

**新機能**

- サードパーティ Cookieが有効になっていて、adobedc.demdex.netへのリクエストがブロックされている場合に、設定されたEdge ドメインへのフォールバックを実装しました。

## バージョン 2.24.1 - 2024年12月6日（PT）

**修正および改善**

- [Adobe Experience Platform Rules Engine](https://github.com/adobe/aepsdk-rulesengine-typescript/)に関する依存関係の問題を解決しました。一部のお客様との統合でエラーが発生していました。 Web SDKには、[Adobe Experience Platform Rules Engine](https://github.com/adobe/aepsdk-rulesengine-typescript/) バージョン 2.0.3以降が必要になりました。

## バージョン 2.24.0 - 2024年10月31日（PT）

**新機能**

- [ データストリームの上書き](/help/datastreams/overrides.md)が、メディアセッションの開始時にサポートされるようになりました。

- [`onContentRendering`](monitoring-hooks.md#onContentRendering)監視フックにAdobe Target応答トークンのサポートを追加しました。

**修正点および改善点**

- 複数のアプリ内メッセージが返された場合、優先度が最も高いメッセージのみが表示されます。 その他は抑制として記録されます。
- 空のデータストリームの上書きはEdge Networkに送信されなくなりました。これにより、サーバーサイドのルーティング設定との潜在的な競合を減らすことができます。
- 他のAdobe SDKと一致するように、次のログメッセージコンポーネント名の名前を変更しました。
   - `DecisioningEngine`は`RulesEngine`に名前変更されました
   - `LegacyMediaAnalytics`は`MediaAnalyticsBridge`に名前変更されました
   - `Privacy`は`Consent`に名前変更されました
- デフォルトのコンテンツ項目が[`applyPropositions`](commands/applypropositions.md)を介してレンダリングされたときに発生したエラーを修正しました。
- Adobe Targetの移動およびサイズ変更アクションで発生するCSS エラーを修正しました。
- [`sendEvent`](commands/sendevent/overview.md)件の回答から`machineLearning` キーを削除しました。

## バージョン 2.23.0 - 2024年9月19日（PT）

**新機能**

- [getIdentity](commands/getidentity.md) コマンドで[CORE ID](/help/collection/identity/overview.md#core-id-and-third-party-identity)をリクエストするためのサポートを追加しました。

**修正点および改善点**

- Web SDKをローカルで実行する際にCookieが正しく書き込まれない問題を修正しました。

## バージョン 2.22.0 - 2024年8月22日（PT）

**新機能**

- パーソナライゼーション監視フックのサポートを追加しました。

**修正点および改善点**

- Internet Explorerのサポートを削除し、ライブラリ Gzip サイズを9%削減しました。
- `onInstanceConfigured` モニターフックが呼び出されたときに、アクティビティマップリンクの詳細が初期化されない問題を修正しました。
- Cookieの宛先が正しいパスに設定されない問題を修正しました。
- を呼び出すに関する顧客の問題を修正しました。
- `adobe_mc` パラメーターの無効なURL エンコーディングが[sendEvent](commands/sendevent/overview.md)呼び出しを失敗させる問題を修正しました。

## バージョン 2.21.1 - 2024年7月18日（PT）

**修正点および改善点**

- NPM ライブラリを使用する際のビルドエラーを修正しました。

## バージョン 2.21.0 - 2024年7月16日（PT）

**新機能**

- 自動提案インタラクショントラッキングのサポートを追加しました。
- `alloy.js` ファイルを提供するカスタムビルドスクリプトを追加しました。
- ActivityMapとイベントグループ化のサポートにより、クリック収集が改善されました。

## バージョン 2.20.0 - 2024年5月21日（PT）

**新機能**

- [ ストリーミングメディアコレクション ](commands/configure/streamingmedia.md)のサポートを追加しました。

**修正点および改善点**

- 同意がオプトアウトされたときに、事前非表示スニペットによってデフォルトのコンテンツが非表示になるバグを修正しました。

## バージョン 2.19.2 - 2024年1月10日（PT）

**修正点および改善点**

- ID エラーが他のエラーをマスキングし、ID エラーを警告に変更する問題を修正しました。
- `renderDecisions`が`false`に設定されたページ上部の呼び出しがあったときに、ページ下部の呼び出しが送信されない問題を修正しました。
- 複数の`adobe_mc` クエリ文字列パラメーターがある場合にWeb SDKがクロスドメイン IDを読み取れなかった問題を修正しました。

## バージョン 2.19.1 - 2023年11月10日（PT）

**修正点および改善点**

- `sendEvent`呼び出しから返されたpropositions配列が常に空になる問題を修正しました。

## バージョン 2.19.0 - 2023年11月1日（PT）

**新機能**

- Adobe Journey Optimizerからのアプリ内メッセージのレンダリングのサポートを追加しました。
- [ ページイベントの上下](../use-cases/personalization/top-bottom-page-events.md)のサポートを追加しました。
- ページ全体のスコープとデフォルトサーフェスのリクエストを制御するために、`sendEvent` コマンドに[`defaultPersonalizationEnabled`](commands/sendevent/personalization.md) オプションを追加しました。

**修正点および改善点**

- 複数のタイプのパーソナライゼーションをレンダリングする場合、結合されたパーソナライゼーションディスプレイイベントが一緒に表示されます。
- シングルページアプリケーションビュー名で大文字と小文字が区別される問題を修正しました。
- シャドウ DOM パーソナライズされたオファーセレクターに関する問題を修正しました。

## バージョン 2.18.0 - 2023年7月31日（PT）

**新機能**

- ](/help/datastreams/overrides.md)データストリーム ID のコマンドごとの上書き[のサポートを追加しました。

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

- ウィンドウで訪問者の移行が機能しない問題を修正しました。訪問者は設定後に定義されます。 これは、Adobe タグを使用して実行する場合に特に問題になります。
- 一部の環境で `device.screenWidth` と `device.screenHeight` が文字列として入力される問題を修正しました。

## バージョン 2.13.0 - 2022年9月28日（PT）

**新機能**

- Page by Page Full Migrationのサポートを追加しました。 訪問者が at.js ページと Web SDK ページの間を移動しても、Adobe Target プロファイルが保持されるようになりました。
- [高エントロピーの User-Agent クライアントヒント](../use-cases/client-hints.md)の設定可能なサポートを追加しました。
- [`applyResponse`](commands/applyresponse.md) コマンドのサポートを追加しました。 これにより、[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/)を介したハイブリッドパーソナライゼーションが可能になります。
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

- モバイルアプリとモバイル web コンテンツの間、およびドメイン間で訪問者 ID を共有することで、パーソナライズされたエクスペリエンスをより正確に提供できるようになりました。 詳細については、「[ データ収集のID](../identity/overview.md)」を参照してください。
- 分析指標を増分せずに、[!DNL Adobe Target] からシングルページアプリケーションに提案の配列をレンダリングまたは実行できるようになりました。 これにより、レポートのエラーが減り、分析の精度が向上します。
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
- 単一ページアプリケーションのビュー変更イベントを最適化しました。 パーソナライズされたエクスペリエンスがレンダリングされる際に、表示通知がビュー変更イベントに含まれるようになりました。
- `eventType` が存在しない場合のコンソール警告を削除しました。
- エクスペリエンスがリクエストされた際、またはキャッシュから取得された際に、`propositions` プロパティが `sendEvent` コマンドからのみ返される問題を修正しました。 `propositions` プロパティは、常に配列として定義されるようになりました。
- Edge Networkから返されたエラーが発生したときに、非表示のコンテナが表示されない問題を修正しました。
- 操作イベントが Adobe Target でカウントされない問題を修正しました。 この問題は、web.webPageDetails.viewName でビュー名を XDM に追加することで修正しました。
- コンソールメッセージのドキュメントのリンク切れを修正します。

## バージョン 2.8.0 - 2022年1月19日（PT）

- パーソナライゼーション用のシャドウ DOM セレクターをサポートします。
- パーソナライゼーションイベントタイプの名前を変更しました。 （`display` と `click` は、`decisioning.propositionDisplay` と `decisioning.propositionInteract` になります）
- スクリプトが 1 回しか実行されていないにも関わらず、インラインスクリプトタグを含む HTML オファーで、スクリプトタグがページに 2 回追加される問題を修正しました。

## バージョン 2.7.0 - 2021年10月26日（PT）

- `inferences`と`destinations`を含むEdge Networkの追加情報を`sendEvent`の戻り値で公開します。 これらの機能が現在Betaの一部としてロールアウトされている場合、これらのプロパティのフォーマットが変わる可能性があります。

## バージョン 2.6.4 - 2021年9月7日（PT）

- `head` 要素に適用された HTML Adobe Target を設定アクションが `head` コンテンツ全体を置き換えていた問題を修正しました。 `head` 要素に適用される HTML を設定アクションを、HTML を追加するように変更しました。

## バージョン 2.6.3 - 2021年8月16日（PT）

- `configure` コマンドから解決された promise を介して、公開を目的としていないオブジェクトが公開される問題を修正しました。

## バージョン 2.6.2 - 2021年8月4日（PT）

- `result.decisions` プロパティがアクセスされていない場合でも、`result.decisions`（`sendEvent` コマンドによって提供される）の非推奨（廃止予定）に関する警告がコンソールに記録される問題を修正しました。 `result.decisions` プロパティにアクセスしても警告はログに記録されませんが、プロパティはまだ非推奨（廃止予定）です。

## バージョン 2.6.1 - 2021年7月29日（PT）

- パーソナライゼーションコンテンツを持たない単一ページアプリビューのパーソナライゼーションをレンダリングすると、エラーがスローされ、`sendEvent` コマンドから返される promise が拒否される問題を修正しました。

## バージョン 2.6.0 - 2021年7月27日（PT）

- Adobe Target 応答トークンなど、`sendEvent` で解決された promise でより多くのパーソナライゼーションコンテンツを提供します。 `sendEvent` コマンドを実行すると、promise が返されます。これは、サーバーから受信した情報を含む `result` オブジェクトで最終的に解決されます。 以前は、この結果オブジェクトには `decisions` という名前のプロパティが含まれていました。 この `decisions` プロパティは、非推奨（廃止予定）です。 新しいプロパティ `propositions` を追加しました。 この新しいプロパティでは、応答トークンを含む、より多くのパーソナライゼーションコンテンツにアクセスできます。

## バージョン 2.5.0 - 2021年6月

- リダイレクトパーソナライゼーションオファーのサポートを追加しました。
- 自動的に収集されたビューポートの幅と高さが負の値の場合、サーバーに送信されなくなりました。
- `onBeforeEventSend` コールバックから `false` を返すことによってイベントがキャンセルされると、メッセージがログに記録されるようになりました。
- 単一のイベントを対象とした特定の XDM データが複数のイベントにわたって含まれていた問題を修正しました。

## バージョン 2.4.0 - 2021年3月

- SDKを[NPM パッケージ ](install/npm.md)としてインストールできるようになりました。
- [デフォルトの同意を設定](commands/configure/defaultconsent.md)する際に、同意が得られるまですべてのイベントをドロップする `out` オプションのサポートを追加しました（既存の `pending` オプションは、同意が得られるとイベントをキューに入れ、送信します）。
- [`onBeforeEventSend`](commands/configure/onbeforeeventsend.md) コールバックを使用して、イベントが送信されないようにできるようになりました。
- レンダリングまたはクリックされたパーソナライズされたコンテンツに関するイベントを送信する際に、`meta.personalization` の代わりに XDM スキーマフィールドグループを使用するようになりました。
- [`getIdentity`](commands/getidentity.md) コマンドは、IDと共にエッジ領域IDを返すようになりました。
- サーバーから受信した警告とエラーが改善され、より適切な方法で処理されます。
- [`setConsent`](commands/setconsent.md) コマンドに対するAdobeの同意管理2.0標準のサポートを追加しました。
- 同意の設定を受け取ると、ハッシュ化され、ローカルストレージに保存されます。これにより、CMP、Experience Platform Web SDK、Experience Platform Edge Network間の統合が最適化されます。 同意環境設定を収集している場合は、ページが読み込まれるたびに `setConsent` を呼び出すことをお勧めします。
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

- バグ修正：`idMigrationEnabled`が`true`の場合、オプトインオブジェクトがWeb SDKの呼び出しをブロックしていました。
- バグ修正：ちらつき問題を防ぐために、パーソナライゼーションオファーを返す必要があるリクエストをWeb SDKで認識できるようにします。

## バージョン 2.1.0 - 2020年8月

- `syncIdentity` コマンドを削除し、`sendEvent` コマンドでこれらの ID を渡すことをサポートします。
- IAB 2.0 同意標準をサポートします。
- `setConsent` コマンドで追加の ID を渡すことをサポートします。
- `sendEvent` コマンドでの `datasetId` の上書きをサポートします。
- 監視フックのサポート （[詳細情報](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)）
- 実装の詳細コンテキストデータで `environment: browser` を渡します。
