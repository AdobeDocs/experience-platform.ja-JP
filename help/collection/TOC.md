---
audience: user
solution: Data Collection
user-guide-title: データ収集
breadcrumb-title: データ収集
user-guide-description: Adobe Experience Platformにデータを送信する方法を説明します。
feature: Data Collection
role: Developer
source-git-commit: 3abe25a9c538bf4d1b439d48f624d8cad109a99e
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 38%

---


# データ収集 {#collection}

+ [概要](home.md)
+ [権限](permissions.md)
+ BrightScript {#brightscript}
   + [BrightScript の概要](brightscript/brs-overview.md)
+ JavaScript {#js}
   + [JavaScriptの概要](js/js-overview.md)
   + [リリースノート](js/release-notes.md)
   + インストール {#install}
      + [インストールの概要](js/install/overview.md)
      + [ライブラリ](js/install/library.md)
      + [NPM](js/install/npm.md)
      + [カスタムビルド](js/install/create-custom-build.md)
   + コマンド {#commands}
      + [appendIdentityToUrl](js/commands/appendidentitytourl.md)
      + [applyPropositions](js/commands/applypropositions.md)
      + [applyResponse](js/commands/applyresponse.md)
      + configure {#configure}
         + [概要](js/commands/configure/overview.md)
         + [autoCollectPropositionInteractions](js/commands/configure/autocollectpropositioninteractions.md)
         + [clickCollection](js/commands/configure/clickcollection.md)
         + [clickCollectionEnabled](js/commands/configure/clickcollectionenabled.md)
         + [context](js/commands/configure/context.md)
         + [datastreamId](js/commands/configure/datastreamid.md)
         + [debugEnabled](js/commands/configure/debugenabled.md)
         + [defaultConsent](js/commands/configure/defaultconsent.md)
         + [downloadLinkQualifier](js/commands/configure/downloadlinkqualifier.md)
         + [edgeBasePath](js/commands/configure/edgebasepath.md)
         + [edgeConfigOverrides](js/commands/configure/edgeconfigoverrides.md)
         + [edgeDomain](js/commands/configure/edgedomain.md)
         + [idMigrationEnabled](js/commands/configure/idmigrationenabled.md)
         + [onBeforeEventSend](js/commands/configure/onbeforeeventsend.md)
         + [onBeforeLinkClickSend](js/commands/configure/onbeforelinkclicksend.md)
         + [orgId](js/commands/configure/orgid.md)
         + [prehidingStyle](js/commands/configure/prehidingstyle.md)
         + [pushNotification](js/commands/configure/pushnotifications.md)
         + [streamingMedia](js/commands/configure/streamingmedia.md)
         + [targetMigrationEnabled](js/commands/configure/targetmigrationenabled.md)
         + [thirdPartyCookiesEnabled](js/commands/configure/thirdpartycookiesenabled.md)
      + [createMediaSession](js/commands/createmediasession.md)
      + [getIdentity](js/commands/getidentity.md)
      + [getLibraryInfo](js/commands/getlibraryinfo.md)
      + [getMediaAnalyticsTracker](js/commands/getmediaanalyticstracker.md)
      + sendEvent {#sendevent}
         + [概要](js/commands/sendevent/overview.md)
         + [data](js/commands/sendevent/data.md)
         + [documentUnloading](js/commands/sendevent/documentunloading.md)
         + [edgeConfigOverrides](js/commands/sendevent/edgeconfigoverrides.md)
         + [eventType](js/commands/sendevent/eventtype.md)
         + [パーソナライズ機能](js/commands/sendevent/personalization.md)
         + [renderDecisions](js/commands/sendevent/renderdecisions.md)
         + [xdm](js/commands/sendevent/xdm.md)
      + [sendMediaEvent](js/commands/sendmediaevent.md)
      + [sendPushSubscription](js/commands/sendpushsubscription.md)
      + [setConsent](js/commands/setconsent.md)
      + [setDebug](js/commands/setdebug.md)
      + [subscribeRulesetItems](js/commands/subscriberulesetitems.md)
      + [コマンドの応答](js/commands/command-responses.md)
   + [フックの監視](js/monitoring-hooks.md)
   + [よくある質問](js/faq.md)
+ タグ {#tags}
   + [概要](tags/overview.md)
   + [buildInfo](tags/buildinfo.md)
   + [会社](tags/company.md)
   + [_container](tags/container.md)
   + [cookie](tags/cookie.md)
   + [environment](tags/environment.md)
   + [getVar](tags/getvar.md)
   + [getVisitorId](tags/getvisitorid.md)
   + [logger](tags/logger.md)
   + [_monitors](tags/monitors.md)
   + [setDebug](tags/setdebug.md)
   + [setVar](tags/setvar.md)
   + [track](tags/track.md)
+ ユースケース {#use-cases}
   + [概要](use-cases/overview.md)
   + [Client hints](use-cases/client-hints.md)
   + [クライアントの状態](use-cases/client-state.md)
   + [コマースデータの収集](use-cases/collect-commerce-data.md)
   + [CSP の設定](use-cases/configuring-a-csp.md)
   + [デバッグ](use-cases/debugging.md)
   + [イベントの重複排除](use-cases/event-duplication.md)
   + ID {#identity}
      + [概要](use-cases/identity/id-overview.md)
      + [ファーストパーティデバイス ID](use-cases/identity/first-party-device-ids.md)
      + [ID 共有](use-cases/identity/id-sharing.md)
   + [複数のSDK インスタンス](use-cases/multiple-instances.md)
   + パーソナライズ機能 {#personalization}
      + [概要](use-cases/personalization/pers-overview.md)
      + [イベントを表示](use-cases/personalization/display-events.md)
      + [フリッカーの管理](use-cases/personalization/manage-flicker.md)
      + [パーソナライズされたコンテンツのレンダリング](use-cases/personalization/rendering-personalization-content.md)
      + [上位および下位のページイベント](use-cases/personalization/top-bottom-page-events.md)
