---
solution: Data Collection
audience: user
user-guide-title: Adobe Experience Platform Web SDK ヘルプ
breadcrumb-title: Web SDK ガイド
user-guide-description: Edge ネットワーク経由で Experience Cloud サービスを操作します。
feature: Web SDK
role: Developer
source-git-commit: bb2c0b5483bf0b50e98e21bef23d1667660d1981
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 51%

---


# Adobe Experience Platform Web SDK {#web-sdk}

* [Web SDK の概要](home.md)
* [リリースノート](release-notes.md)
* Web SDK のインストール {#install}
   * [概要](install/overview.md)
   * [タグ拡張機能を使用した Web SDK のインストール](install/extension.md)
   * [JavaScript ライブラリを使用して Web SDK をインストールします](install/library.md)
   * [NPM パッケージを使用して Web SDK をインストール](install/npm.md)
* コマンド {#commands}
   * {#configure} の設定
      * [概要](commands/configure/overview.md)
      * [autoTrackPropositionInteractionsEnabled](commands/configure/autotrackpropositioninteractionsenabled.md)
      * [clickCollectionEnabled](commands/configure/clickcollectionenabled.md)
      * [clickCollection](commands/configure/clickcollection.md)
      * [context](commands/configure/context.md)
      * [debugEnabled](commands/configure/debugenabled.md)
      * [defaultConsent](commands/configure/defaultconsent.md)
      * [downloadLinkQualifier](commands/configure/downloadlinkqualifier.md)
      * [edgeBasePath](commands/configure/edgebasepath.md)
      * [edgeConfigId](commands/configure/edgeconfigid.md)
      * [edgeDomain](commands/configure/edgedomain.md)
      * [idMigrationEnabled](commands/configure/idmigrationenabled.md)
      * [streamingMedia](commands/configure/streamingmedia.md)
      * [onBeforeEventSend](commands/configure/onbeforeeventsend.md)
      * [onBeforeLinkClickSend](commands/configure/onbeforelinkclicksend.md)
      * [orgId](commands/configure/orgid.md)
      * [prehidingStyle](commands/configure/prehidingstyle.md)
      * [targetMigrationEnabled](commands/configure/targetmigrationenabled.md)
      * [thirdPartyCookiesEnabled](commands/configure/thirdpartycookiesenabled.md)
   * sendEvent {#sendevent}
      * [概要](commands/sendevent/overview.md)
      * [data](commands/sendevent/data.md)
      * [documentUnloading](commands/sendevent/documentunloading.md)
      * [パーソナライズ機能](commands/sendevent/personalization.md)
      * [renderDecisions](commands/sendevent/renderdecisions.md)
      * [タイプ](commands/sendevent/type.md)
      * [xdm](commands/sendevent/xdm.md)
   * [appendIdentityToUrl](commands/appendidentitytourl.md)
   * [applyPropositions](commands/applypropositions.md)
   * [applyResponse](commands/applyresponse.md)
   * [createMediaSession](commands/createmediasession.md)
   * [getIdentity](commands/getidentity.md)
   * [getLibraryInfo](commands/getlibraryinfo.md)
   * [getMediaAnalyticsTracker](commands/getmediaanalyticstracker.md)
   * [setConsent](commands/setconsent.md)
   * [setDebug](commands/setdebug.md)
   * [sendMediaEvent](commands/sendmediaevent.md)
   * [データストリームの上書きの設定](commands/datastream-overrides.md)
   * [コマンドの応答](commands/command-responses.md)

* ID {#identity}
   * [概要](identity/overview.md)
   * [ファーストパーティデバイス ID](identity/first-party-device-ids.md)
   * [モバイルから web、およびクロスドメインでの ID の共有](identity/id-sharing.md)

* パーソナライゼーション {#personalization}
   * [表示イベントの管理](personalization/display-events.md)
   * [パーソナライズされたコンテンツのレンダリング](personalization/rendering-personalization-content.md)
   * [ハイブリッド実装を使用したパーソナライゼーション](personalization/hybrid-personalization.md)
   * [フリッカーの管理](personalization/manage-flicker.md)
   * Adobe Target {#adobe-target}
      * [概要](personalization/adobe-target/target-overview.md)
      * [シングルページアプリケーションの実装](personalization/adobe-target/spa-implementation.md)
      * [レスポンストークンへのアクセス](personalization/adobe-target/accessing-response-tokens.md)
      * [mbox サードパーティ ID の使用](personalization/adobe-target/using-mbox-3rdpartyid.md)
      * [at.js ライブラリと Web SDK の比較](personalization/adobe-target/web-sdk-atjs-comparison.md)
      * Analytics for Target（A4T）ログ {#a4t}
         * [概要](personalization/adobe-target/analytics-logging/overview.md)
         * [クライアントサイドログ](personalization/adobe-target/analytics-logging/client-side.md)
         * [サーバーサイドログ](personalization/adobe-target/analytics-logging/server-side.md)
   * Offer Decisioning {#offer-decisioning}
      * [概要](personalization/offer-decisioning/offer-decisioning-overview.md)
   * Adobe Journey Optimizer {#ajo}
      * [概要](personalization/ajo/overview.md)
      * [シングルページアプリケーションの実装](personalization/ajo/web-spa-implementation.md)
      * [Web SDK での Web アプリ内メッセージのサポートの設定](personalization/web-in-app-messaging.md)

* 同意 {#consent}
   * IAB の透明性および同意フレームワーク 2.0 {#iab-tcf}
      * [概要](consent/iab-tcf/overview.md)
      * [タグを使用した統合](consent/iab-tcf/with-tags.md)
      * [タグなしでの統合](consent/iab-tcf/without-tags.md)

* ユースケース {#use-cases}
   * [概要](use-cases/overview.md)
   * [Web SDK を使用したAdobe Analyticsへのデータの送信](use-cases/adobe-analytics.md)
   * [User agent client hints](use-cases/client-hints.md)
   * [コマースデータの収集](use-cases/collect-commerce-data.md)
   * [CSP の設定](use-cases/configuring-a-csp.md)
   * [デバッグメソッド](use-cases/debugging.md)
   * [複数の Web SDK インスタンスの使用](use-cases/multiple-instances.md)
   * [上位および下位のページイベントの設定](use-cases/top-bottom-page-events.md)

* [よくある質問](faq.md)
* [リソース](resources.md)
