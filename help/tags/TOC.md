---
audience: user
user-guide-title: タグのヘルプ
breadcrumb-title: タグ
user-guide-description: 顧客体験を向上させるための、分析、マーケティングおよび広告タグのデプロイと管理について説明します。
feature: Tags
solution: Data Collection
source-git-commit: c5cc36d9530ff6fbb52a1995844f495b38e938b3
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 100%

---


# タグ {#tags}

* [タグの概要](./home.md)
* はじめに {#get-started}
   * [クイックスタートガイド](./quick-start/quick-start.md)
   * [実装ガイド](./quick-start/implementation-guides.md)
* UI ガイド {#ui}
   * [概要](./ui/managing-resources/overview.md)
   * 拡張機能 {#extensions}
      * [概要](./ui/managing-resources/extensions/overview.md)
      * [拡張機能のアップグレード](./ui/managing-resources/extensions/extension-upgrade.md)
   * [データ要素](./ui/managing-resources/data-elements.md)
   * [ルール](./ui/managing-resources/rules.md)
   * [備考](./ui/managing-resources/notes.md)
   * [リソースのコピー](./ui/managing-resources/copying-resources.md)
   * [リソースリビジョンの比較](./ui/managing-resources/compare-resource-revisions.md)
   * [リソースの削除](./ui/managing-resources/delete-resources.md)
   * [ライブラリからのリソースの削除](./ui/managing-resources/remove-resources-from-library.md)
* パブリッシュ {#publish}
   * [概要](./ui/publishing/overview.md)
   * [公開フロー](./ui/publishing/publishing-flow.md)
   * ホスト {#hosts}
      * [概要](./ui/publishing/hosts/hosts-overview.md)
      * [アドビが管理するホスト](./ui/publishing/hosts/managed-by-adobe-host.md)
      * [SFTP ホスト](./ui/publishing/hosts/sftp-host.md)
   * 環境 {#environments}
      * [概要](./ui/publishing/environments.md)
      * [Adobe Experience Platform Debugger を使用した埋め込みコードのテスト](./ui/publishing/embed-code-testing.md)
   * [ビルド](./ui/publishing/builds.md)
   * [ライブラリ](./ui/publishing/libraries.md)
   * [自己ホスト型ライブラリ](./ui/publishing/hosts/self-hosting-libraries.md)
   * [ライブラリの再公開](./ui/publishing/republish.md)
   * [Premium CDN サポート（ベータ版）](./ui/publishing/premium-cdn.md)
* クライアント側の情報 {#client-side}
   * [概要](./ui/client-side/overview.md)
   * [非同期デプロイメント](./ui/client-side/asynchronous-deployment.md)
   * [Satellite オブジェクトのリファレンス](./ui/client-side/satellite-object.md)
   * [顧客の同意を管理する JavaScript タグのデプロイ](./ui/client-side/consent.md)
   * [コンテンツセキュリティポリシー（CSP）のサポート](./ui/client-side/content-security-policy.md)
   * [サブリソースの整合性（SRI）のサポート](./ui/client-side/sri.md)
* イベントの転送 {#event-forwarding}
   * [概要](./ui/event-forwarding/overview.md)
   * [はじめに](./ui/event-forwarding/getting-started.md)
   * [シークレットの設定](./ui/event-forwarding/secrets.md)
   * [モニタリング（ベータ版）](./ui/event-forwarding/monitoring.md)
* 管理 {#admin}
   * [概要](./ui/administration/overview.md)
   * [会社とプロパティ](./ui/administration/companies-and-properties.md)
   * [ユーザー権限](./ui/administration/user-permissions.md)
* 拡張機能 {#extensions}
   * [概要](./extensions/overview.md)
   * タグ拡張機能（クライアントサイド）{#client}
      * [概要](./extensions/client/overview.md)
      * [Accessible Site Speed Metrics](https://exchange.adobe.com/apps/ec/103053)
      * [Activity Map Customizer](https://exchange.adobe.com/apps/ec/101531)
      * [Action Page Refresh](https://exchange.adobe.com/apps/ec/102848)
      * [Adform Website Tracking](https://exchange.adobe.com/apps/ec/103195)
      * [Adobe Advertising Cloud](https://exchange.adobe.com/apps/ec/100155)
      * Adobe Analytics {#analytics}
         * [概要](./extensions/client/analytics/overview.md)
         * [共有モジュール](./extensions/client/analytics/shared-modules.md)
         * [リリースノート](./extensions/client/analytics/release-notes.md)
      * [Adobe Analytics &amp; Adobe Target](https://exchange.adobe.com/apps/ec/105363/6sense-for-analytics-and-target)
      * [Adobe Analytics &amp; Microsoft Dynamics](https://exchange.adobe.com/apps/ec/102966)
      * [Adobe Analytics &amp; Salesforce](https://exchange.adobe.com/apps/ec/101530)
      * Adobe Analytics 製品文字列 {#product-string}
         * [概要](./extensions/client/product-string/overview.md)
         * [リリースノート](./extensions/client/product-string/release-notes.md)
      * [Adobe Analytics Product String Builder](https://exchange.adobe.com/apps/ec/101461)
      * [Adobe Experience Platform Web SDK を使用した Adobe Analytics](https://exchange.adobe.com/apps/ec/108985/search-discovery-for-adobe-analytics-via-aep-web-sdk)
      * Adobe Audience Manager {#audience-manager}
         * [概要](./extensions/client/audience-manager/overview.md)
      * Adobe Client Data Layer {#client-data-layer}
         * [概要](./extensions/client/client-data-layer/overview.md)
         * [リリースノート](./extensions/client/client-data-layer/release-notes.md)
      * Adobe ContextHub {#contexthub}
         * [概要](./extensions/client/contexthub/overview.md)
      * [Adobe Experience Manager Forms](https://exchange.adobe.com/apps/ec/107493)
      * Adobe Experience Cloud ID サービス {#id-service}
         * [概要](./extensions/client/id-service/overview.md)
         * [リリースノート](./extensions/client/id-service/release-notes.md)
      * Adobe Experience Platform デモ {#platform-demo}
         * [概要](./extensions/client/platform-demo/overview.md)
      * Adobe Experience Platform Web SDK {#sdk}
         * [概要](./extensions/client/sdk/overview.md)
      * Adobe Experience Manager Asset Insights {#asset-insights}
         * [概要](./extensions/client/asset-insights/overview.md)
         * [リリースノート](./extensions/client/asset-insights/release-notes.md)
      * [Adobe Fonts](https://exchange.adobe.com/apps/ec/101538)
      * Adobe Media Analytics for Audio and Video {#media-analytics}
         * [概要](./extensions/client/media-analytics/overview.md)
         * [リリースノート](./extensions/client/media-analytics/release-notes.md)
      * Adobe Media Analytics（3.x SDK） {#media-analytics-3x}
         * [概要](./extensions/client/media-analytics-3x/overview.md)
         * [リリースノート](./extensions/client/media-analytics-3x/release-notes.md)
      * アドビプライバシー {#privacy}
         * [概要](./extensions/client/privacy/overview.md)
      * [Adobe Report Suite Selector](https://exchange.adobe.com/apps/ec/100640)
      * Adobe Target {#target}
         * [概要](./extensions/client/target/overview.md)
         * [リリースノート](./extensions/client/target/release-notes.md)
      * Adobe Target v2 {#target-v2}
         * [概要](./extensions/client/target-v2/overview.md)
         * [リリースノート](./extensions/client/target-v2/release-notes.md)
      * [Adobe Target Toolkit](https://exchange.adobe.com/apps/ec/100640)
      * [Advertising Cloud](https://exchange.adobe.com/apps/ec/100640)
      * [AEM Asset Insights](https://exchange.adobe.com/apps/ec/103406)
      * [Airbrake JS Notifier](https://exchange.adobe.com/apps/ec/103342)
      * [Amplitude](https://exchange.adobe.com/apps/ec/108010)
      * [Apollo QAX](https://exchange.adobe.com/apps/ec/105068)
      * [Awin Advertiser MasterTag](https://exchange.adobe.com/apps/ec/103176)
      * [Awin Conversion Tag](https://exchange.adobe.com/apps/ec/103240)
      * [Beemray Human Context](https://exchange.adobe.com/apps/ec/101063)
      * [Bing Ads Universal Event Tracking](https://exchange.adobe.com/apps/ec/100154)
      * [ブランチ](https://exchange.adobe.com/apps/ec/101382)
      * [!DNL BrightCove] ビデオトラッキング {#brightcove}
         * [概要](./extensions/client/brightcove/overview.md)
         * [リリースノート](./extensions/client/brightcove/release-notes.md)
      * [CallTrackingMetrics](https://exchange.adobe.com/apps/ec/107695)
      * [Channel Source Identifier](https://exchange.adobe.com/apps/ec/101412)
      * [Cheetah Experiences](https://exchange.adobe.com/apps/ec/102759)
      * [Clicktale](https://exchange.adobe.com/apps/ec/100082)
      * Common Analytics Plugins {#plugins}
         * [概要](./extensions/client/plugins/overview.md)
         * [リリースノート](./extensions/client/plugins/release-notes.md)
      * Common Web SDK Plugins {#web-sdk-plugins}
         * [概要](./extensions/client/web-sdk-plugins/overview.md)
         * [リリースノート](./extensions/client/web-sdk-plugins/release-notes.md)
      * [連結](https://exchange.adobe.com/apps/ec/104690)
      * [ContentSquare](https://exchange.adobe.com/apps/ec/100364)
      * [Cookie Consent Management by Usercentrics CMP v2](https://exchange.adobe.com/apps/ec/107037)
      * コア {#core}
         * [概要](./extensions/client/core/overview.md)
         * [リリースノート](./extensions/client/core/release-notes.md)
      * [Custom Debug Logger](https://exchange.adobe.com/apps/ec/104698)
      * [Customer Recognition](https://exchange.adobe.com/apps/ec/100688)
      * [Data Element Assistant (DEA)](https://exchange.adobe.com/apps/ec/101413)
      * [Data Layer Manager](https://exchange.adobe.com/apps/ec/101462)
      * [Decibel](https://exchange.adobe.com/apps/ec/100913)
      * [Demandbase](https://exchange.adobe.com/apps/ec/101605)
      * [Differential Privacy](https://exchange.adobe.com/apps/ec/104535)
      * [Dynamic Media ビューア](https://exchange.adobe.com/apps/ec/103048)
      * [EDDL Helper](https://exchange.adobe.com/apps/ec/107691)
      * [Flashtalking OneTag](https://exchange.adobe.com/apps/ec/101392)
      * [ForeSee](https://exchange.adobe.com/apps/ec/100164)
      * [Gainsight PX](https://exchange.adobe.com/apps/ec/103343)
      * [Genesys Predictive Engagement](https://exchange.adobe.com/apps/ec/106148)
      * Google Data Layer {#google-data-layer}
         * [概要](./extensions/client/google-data-layer/overview.md)
         * [リリースノート](./extensions/client/google-data-layer/release-notes.md)
      * [Google Global Site Tag (gtag)](https://exchange.adobe.com/apps/ec/101437/google-global-site-tag-gtag)
      * [InMoment](https://exchange.adobe.com/apps/ec/100847)
      * [JSON Helper](https://exchange.adobe.com/apps/ec/106449)
      * [JW Player Analytics](https://exchange.adobe.com/apps/ec/101523)
      * [KickFire](https://exchange.adobe.com/apps/ec/101621)
      * [Mapping Table](https://exchange.adobe.com/apps/ec/103136)
      * [!DNL Marketo Munchkin] {#marketo}
         * [概要](./extensions/client/marketo/overview.md)
         * [リリースノート](./extensions/client/marketo/release-notes.md)
      * [Master Property Manager](https://exchange.adobe.com/apps/ec/102992)
      * [!DNL Meta Pixel] {#meta}
         * [概要](./extensions/client/meta/overview.md)
      * [Monita](https://exchange.adobe.com/apps/ec/106544)
      * [Nielsen Digital SDK](https://exchange.adobe.com/apps/ec/101361)
      * [OneTrust Consent Management for Cookies](https://exchange.adobe.com/apps/ec/100340)
      * [Pepperjam](https://exchange.adobe.com/apps/ec/103587)
      * [Persado Connect](https://exchange.adobe.com/apps/ec/103745)
      * [Pinterest Conversion Tracking](https://exchange.adobe.com/apps/ec/100523)
      * [Pixel Loader](https://exchange.adobe.com/apps/ec/100152)
      * [Qualtrics Website Feedback](https://exchange.adobe.com/apps/ec/101569)
      * [Quantum Metric](https://exchange.adobe.com/apps/ec/101535)
      * [Resolve Momentum](https://exchange.adobe.com/apps/ec/108352)
      * [Rokt](https://exchange.adobe.com/apps/ec/107591)
      * [SDI Survey](https://exchange.adobe.com/apps/ec/102991)
      * [SDI Toolkit](https://exchange.adobe.com/apps/ec/101460)
      * [SessionCam](https://exchange.adobe.com/apps/ec/100517)
      * [SPA View Change Event](https://partners.adobe.com/exchangeprogram/experiencecloud/exchange.details.105867.html)
      * [Storage Spanner](https://exchange.adobe.com/apps/ec/102990)
      * [TAGS by Loop Horizon](https://exchange.adobe.com/apps/ec/106092)
      * [Tealium Collect](https://exchange.adobe.com/apps/ec/104217)
      * [Tealium Data Enrichment](https://exchange.adobe.com/apps/ec/104217)
      * [TMMData Foundation Platform](https://exchange.adobe.com/apps/ec/100148)
      * [TrustArc Cookie Consent Manager](https://exchange.adobe.com/apps/ec/107037)
      * [Vimeo Playback](https://exchange.adobe.com/apps/ec/108937)
      * [Web Vitals](https://exchange.adobe.com/apps/ec/106769)
      * [XDM Composer](https://exchange.adobe.com/apps/ec/106062)
      * [Yahoo Dot](https://exchange.adobe.com/apps/ec/106062)
      * [Yext Conversion Tracking](https://exchange.adobe.com/apps/ec/103174)
      * [[!DNL Youtube] 再生](https://exchange.adobe.com/apps/ec/103174)
      * [!DNL YouTube] ビデオトラッキング {#youtube}
         * [概要](./extensions/client/youtube/overview.md)
         * [リリースノート](./extensions/client/youtube/release-notes.md)
   * イベント転送拡張機能（サーバーサイド）{#server}
      * [概要](./extensions/server/overview.md)
      * Adobe Experience Platform Cloud Connector {#cloud-connector}
         * [概要](./extensions/server/cloud-connector/overview.md)
         * [リリースノート](./extensions/server/cloud-connector/release-notes.md)
      * [!DNL AWS] {#aws}
         * [概要](./extensions/server/aws/overview.md)
      * [!DNL Braze] {#braze}
         * [概要](./extensions/server/braze/overview.md)
      * [Cloud Connector for Google Analytics](https://exchange.adobe.com/apps/ec/106542)
      * コア {#core}
         * [概要](./extensions/server/core/overview.md)
      * [Epsilon Event API](https://exchange.adobe.com/apps/ec/109127)
      * Google Ads Enhanced Conversions {#google-ads-enhanced-conversions}
         * [概要](./extensions/server/google-ads-enhanced-conversions/overview.md)
      * [!DNL Mailchimp] Edge {#mailchimp}
         * [概要](./extensions/server/mailchimp/overview.md)
      * [!DNL Meta Conversions API] {#meta}
         * [概要](./extensions/server/meta/overview.md)
      * [!UICONTROL Microsoft Azure] {#azure}
         * [概要](./extensions/server/azure/overview.md)
      * [!DNL Mixpanel] {#mixpanel}
         * [概要](./extensions/server/mixpanel/overview.md)
      * [Pega Customer Decision Hub](https://exchange.adobe.com/apps/ec/107597)
      * [!DNL Pinterest] {#pinterest}
         * [概要](./extensions/server/pinterest/overview.md)
      * [Snap Conversions API](https://exchange.adobe.com/apps/ec/108550)
      * [!DNL Splunk] {#splunk}
         * [概要](./extensions/server/splunk/overview.md)
      * [!DNL Twitter] {#twitter}
         * [概要](./extensions/server/twitter/overview.md)
      * [!DNL Zendesk]イベント API {#zendesk}
         * [概要](./extensions/server/zendesk/overview.md)
* 拡張機能の開発 {#extension-dev}
   * [概要](./extension-dev/overview.md)
   * [はじめに](./extension-dev/getting-started.md)
   * [サポートされているブラウザー](./extension-dev/browsers.md)
   * 送信プロセス {#submit}
      * [概要](./extension-dev/submit/overview.md)
      * [会社の設定](./extension-dev/submit/setup.md)
      * [ユーザーアクセスの許可](./extension-dev/submit/access.md)
      * [拡張機能の開発](./extension-dev/submit/develop.md)
      * [Exchange リストの作成](./extension-dev/submit/create-listing.md)
      * [エンドツーエンドテストのアップロードと実装](./extension-dev/submit/upload-and-test.md)
      * [拡張機能のリリース](./extension-dev/submit/release.md)
   * [拡張機能の設定](./extension-dev/configuration.md)
   * [拡張機能マニフェスト](./extension-dev/manifest.md)
   * Web 拡張機能 {#web}
      * [拡張機能のフロー](./extension-dev/web/flow.md)
      * [ライブラリモジュールの形式](./extension-dev/web/format.md)
      * [件数](./extension-dev/web/views.md)
      * [イベントタイプ](./extension-dev/web/event-types.md)
      * [条件のタイプ](./extension-dev/web/condition-types.md)
      * [アクションタイプ](./extension-dev/web/action-types.md)
      * [データ要素タイプ](./extension-dev/web/data-element-types.md)
      * [コアモジュール](./extension-dev/web/core.md)
      * [共有モジュール](./extension-dev/web/shared.md)
   * エッジ拡張機能 {#edge}
      * [拡張機能のフロー](./extension-dev/edge/flow.md)
      * [ライブラリモジュールの形式](./extension-dev/edge/format.md)
      * [条件のタイプ](./extension-dev/edge/condition-types.md)
      * [アクションタイプ](./extension-dev/edge/action-types.md)
      * [データ要素タイプ](./extension-dev/edge/data-element-types.md)
      * [コンテキストパラメーター](./extension-dev/edge/context.md)
   * [サードパーティライブラリのホスト](./extension-dev/third-party-libraries.md)
   * [turbine 自由変数](./extension-dev/turbine.md)
   * [下位互換性規格](./extension-dev/backwards-compatibility.md)
* Reactor API {#api}
   * [概要](./api/overview.md)
   * [はじめに](./api/getting-started.md)
   * エンドポイント {#endpoints}
      * [会社](./api/endpoints/companies.md)
      * [プロパティ](./api/endpoints/properties.md)
      * [データ要素](./api/endpoints/data-elements.md)
      * [ルール](./api/endpoints/rules.md)
      * [ルールコンポーネント](./api/endpoints/rule-components.md)
      * [拡張機能パッケージ](./api/endpoints/extension-packages.md)
      * [拡張機能](./api/endpoints/extensions.md)
      * [ライブラリ](./api/endpoints/libraries.md)
      * [ビルド](./api/endpoints/builds.md)
      * [環境](./api/endpoints/environments.md)
      * [ホスト](./api/endpoints/hosts.md)
      * [アプリの設定](./api/endpoints/app-configurations.md)
      * [監査イベント](./api/endpoints/audit-events.md)
      * [コールバック](./api/endpoints/callbacks.md)
      * [備考](./api/endpoints/notes.md)
      * [プロファイル](./api/endpoints/profile.md)
      * [検索](./api/endpoints/search.md)
      * [秘密鍵](./api/endpoints/secrets.md)
   * ガイド {#guides}
      * [デリゲート記述子 ID](./api/guides/delegate-descriptor-ids.md)
      * [値の暗号化](./api/guides/encrypting-values.md)
      * [エラー処理](./api/guides/error-handling.md)
      * [回答のフィルタリング](./api/guides/filtering.md)
      * [応答のページ分割](./api/guides/pagination.md)
      * [応答の並べ替え](./api/guides/sorting.md)
      * [関係](./api/guides/relationships.md)
      * [リソースの検索](./api/guides/search.md)
      * [秘密鍵](./api/guides/secrets.md)
* [FAQ](./faq.md)
* [用語の更新](./term-updates.md)
* [Internet Explorer 10 および 11 のサポートの廃止](./ie-deprecation.md)
* リリースノート {#release-notes}
   * [最新](./release-notes/current.md)
   * [2021年リリースノート](./release-notes/2021.md)
   * [2020 年リリースノート](./release-notes/2020.md)
   * [2019 年リリースノート](./release-notes/2019.md)
   * [2018 年リリースノート](./release-notes/2018.md)

