---
title: 変数が自動的にAdobe Analyticsにマッピング
seo-title: Adobe Experience PlatformWeb SDKを使用してAdobe Analyticsで自動的にマッピングされる変数
description: Experience PlatformWeb SDKを使用してAdobe Analyticsで自動的にマッピングされる変数について学習します。
seo-description: Experience PlatformWeb SDKを使用してAdobe Analyticsで自動的にマッピングされる変数について学習します。
keywords: adobe analytics;variables;analytics;automatic map;automatically mapped;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 36%

---


# Variables automatically mapped in [!DNL Analytics]

Below is a list of variables that Adobe Experience Platform Edge Network automatically maps into [!DNL Analytics].

| XDM フィールドパス | [!DNL Analytics Query String] / HTTP ヘッダー | 説明 |
| ---------- | ------------------------- | ----------------------------------------- |
| `application.id` | `c.a.appid` | AppMeasurement コンテキストデータ `c.a.appid` のマッピング。 |
| `application.launches.value` | `c.a.launches` | AppMeasurement コンテキストデータ `c.a.launches` のマッピング。 |
| `commerce.checkouts.id` | `events` | `scCheckout` イベントのシリアル化。 このフィールドを除外すると(シリアライズされていないイベントの場合など)、独自のID値が生成され、エンティティに割り当てられます。 |
| `commerce.checkouts.value` | `events` | AppMeasurementクエリパラメーターイベント_リスト_FULLとコンバージョンCOMMERCE_SC_CHECKOUTとのマッピング（区切り文字を使用） `,`。 |
| `commerce.order.currencyCode` | `cc` | AppMeasurement クエリパラメーター CURRENCY のマッピング。 |
| `commerce.order.purchaseID` | `pi` | AppMeasurement クエリパラメーター PURCHASEID のマッピング。 |
| `commerce.productListAdds.id` | `events` | `scAdd` イベントのシリアル化。 このフィールドを除外すると(シリアライズされていないイベントの場合など)、独自のID値が生成され、エンティティに割り当てられます。 |
| `commerce.productListAdds.value` | `events` | AppMeasurementクエリパラメーターイベント_リスト_FULLのマッピング(コンバージョンCOMMERCE_SC_追加とのマッピング)。区切り文字を使用 `,`します。 |
| `commerce.productListOpens.id` | `events` | `scOpen` イベントのシリアル化。 このフィールドを除外すると(シリアライズされていないイベントの場合など)、独自のID値が生成され、エンティティに割り当てられます。 |
| `commerce.productListOpens.value` | `events` | AppMeasurementクエリパラメーターイベント_リスト_FULLのマッピング（コンバージョンCOMMERCE_SC_OPENとのマッピング）。区切り文字を使用 `,`します。 |
| `commerce.productListRemovals.id` | `events` | `scRemove` イベントのシリアル化。 このフィールドを除外すると(シリアライズされていないイベントの場合など)、独自のID値が生成され、エンティティに割り当てられます。 |
| `commerce.productListRemovals.value` | `events` | AppMeasurementクエリパラメーターイベント_リスト_FULLのマッピングとコンバージョンCOMMERCE_SC_REMOVEとの対応付け（区切り文字を使用） `,`。 |
| `commerce.productListViews.id` | `events` | `scView` イベントのシリアル化。 このフィールドを除外すると(シリアライズされていないイベントの場合など)、独自のID値が生成され、エンティティに割り当てられます。 |
| `commerce.productListViews.value` | `events` | AppMeasurementクエリパラメーターイベント_リスト_FULLの、コンバージョンCOMMERCE_SC_表示とのマッピング（区切り文字を使用） `,`。 |
| `commerce.productViews.id` | `events` | `prodView` イベントのシリアル化。 このフィールドを除外すると(シリアライズされていないイベントの場合など)、独自のID値が生成され、エンティティに割り当てられます。 |
| `commerce.productViews.value` | `events` | AppMeasurementクエリパラメータイベント_リスト_FULLは、コンバージョンCOMMERCE_PROD_表示との区切り文字を使用したマッピング `,`です。 |
| `commerce.purchases.value` | `events` | AppMeasurementクエリパラメーターイベント_リスト_FULLのを、区切り文字を使用して、コンバージョンCOMMERCE_PURCHASEとマッピング `,`します。 |
| `device.colorDepth` | `c` | AppMeasurement クエリパラメーター C_COLOR のマッピング。 |
| `device.screenHeight` | `s` | AppMeasurementクエリパラメーターの画面解像度のマッピング。 |
| `device.screenWidth` | `s` | AppMeasurementクエリパラメーターの画面解像度のマッピング。 |
| `environment.browserDetails.acceptLanguage` | `Accept-Language` | HTTP ヘッダーマッピング（HEADER_ACCEPT_LANGUAGE）です。 |
| `environment.browserDetails.cookiesEnabled` | `k` | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター COOKIES のマッピング。 |
| `environment.browserDetails.javaEnabled` | `v` | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター JAVA_ENABLED のマッピング。 |
| `environment.browserDetails.javaScriptVersion` | `j` | AppMeasurement クエリパラメーター J_JSCRIPT のマッピング。 |
| `environment.browserDetails.userAgent` | `User-Agent` | HTTP ヘッダーマッピング（HEADER_USER_AGENT）です。 |
| `environment.browserDetails.viewportHeight` | `bh` | AppMeasurement クエリパラメーター BROWSER_HEIGHT のマッピング。 |
| `environment.browserDetails.viewportWidth` | `bw` | AppMeasurement クエリパラメーター BROWSER_WIDTH のマッピング。 |
| `environment.connectionType` | `ct` | AppMeasurement クエリパラメーター CT_CONNECT_TYPE のマッピング。 |
| `environment.ipV4` | `X-Forwarded-For` | これはHTTPヘッダーマッピングで、X-FORWARDED-FORです。 |
| `identityMap.ECID.[0].id` | `mid` | AppMeasurement クエリパラメーター MID のマッピング。 |
| `marketing.trackingCode` | `v0` | AppMeasurement クエリパラメーター CAMPAIGN のマッピング。 |
| `media.mediaTimed.completes.value` | `c.a.media.complete` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.dropBeforeStart.value` | `c.a.media.view`、`c.a.media.timePlayed`、`c.a.media.play` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.federated.value` | `c.a.media.federated` | AppMeasurement コンテキストデータ `c.a.media.federated` のマッピング。 |
| `media.mediaTimed.firstQuartiles.value` | `c.a.media.progress25` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.mediaSegmentView.value` | `c.a.media.segmentView` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.midpoints.value` | `c.a.media.progress50` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.pauseTime.value` | `c.a.media.pauseTime` | AppMeasurement コンテキストデータ `c.a.media.pauseTime` のマッピング。 |
| `media.mediaTimed.pauses.value` | `c.a.media.pauseCount` | AppMeasurement コンテキストデータ `c.a.media.pauseCount` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.@id` | `c.a.media.asset` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.primaryAssetReference.dc:title` | `c.a.media.friendlyName` | AppMeasurement コンテキストデータ `c.a.media.friendlyName` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Creator.[N].iptc4xmpExt:Name` | `c.a.media.originator` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Episode.iptc4xmpExt:Number` | `c.a.media.episode` | AppMeasurement コンテキストデータ `c.a.media.episode` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Genre` | `c.a.media.genre` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Rating.[N].iptc4xmpExt:RatingValue` | `c.a.media.rating` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Season.iptc4xmpExt:Number` | `c.a.media.season` | AppMeasurement コンテキストデータ `c.a.media.season` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Identifier` | `a.media.name` | AppMeasurement コンテキストデータ `a.media.name` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Name` | `c.a.media.show` | AppMeasurement コンテキストデータ `c.a.media.show` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.showType` | `c.a.media.type` | コンバージョン VEDIO_SHOW_TYPE を使用した AppMeasurement コンテキストデータ `c.a.media.type` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.showType` | `c.a.media.type` | AppMeasurement context data `c.a.media.type` mapping with conversion VIDEO_SHOW_TYPE. |
| `media.mediaTimed.primaryAssetReference.xmpDM:duration` | `c.a.media.length` | AppMeasurement コンテキストデータ `c.a.media.length` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.@id` | `c.a.media.vsid` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastChannel` | `c.a.media.channel` | AppMeasurement コンテキストデータ `c.a.media.channel` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastContentType` | `c.a.contentType` | AppMeasurement コンテキストデータ `c.a.contentType` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastNetwork` | `c.a.media.network` | AppMeasurement コンテキストデータ `c.a.media.network` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.mediaSegmentView.value` | `c.a.media.segment` | AppMeasurement コンテキストデータ `c.a.media.segment` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.playerName` | `c.a.media.playerName` | AppMeasurement コンテキストデータ `c.a.media.playerName` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.playerSDKVersion.version` | `c.a.media.sdkVersion` | AppMeasurement コンテキストデータ `c.a.media.sdkVersion` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.sourceFeed` | `c.a.media.feed` | AppMeasurement コンテキストデータ `c.a.media.feed` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.streamFormat` | `c.a.media.format` | AppMeasurement コンテキストデータ `c.a.media.format` のマッピング。 |
| `media.mediaTimed.progress10.value` | `c.a.media.progress10` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.progress95.value` | `c.a.media.progress95` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.resumes.value` | `c.a.media.resume` | AppMeasurement コンテキストデータ `c.a.media.resume` のマッピング。 |
| `media.mediaTimed.starts.value` | `c.a.media.view` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.thirdQuartiles.value` | `c.a.media.progress75` | AppMeasurementコンテキストデータ。 |
| `media.mediaTimed.timePlayed.value` | `c.a.media.timePlayed` | AppMeasurement コンテキストデータ `c.a.media.timePlayed` のマッピング。 |
| `media.mediaTimed.totalTimePlayed.value` | `c.a.media.totalTimePlayed` | AppMeasurement コンテキストデータ `c.a.media.totalTimePlayed` のマッピング。 |
| `placeContext.geo.latitude` | `lat` | AppMeasurement クエリパラメーター LATITUDE のマッピング。 |
| `placeContext.geo.longitude` | `lon` | AppMeasurement クエリパラメーター LONGITUDE のマッピング。 |
| `placeContext.geo.postalCode` | `zip` | AppMeasurement クエリパラメーター ZIP のマッピング。 |
| `placeContext.geo.stateProvince` | `state` | AppMeasurement クエリパラメーター STATE のマッピング。 |
| `productlistitems.[N]._[NAME_SPACE].*` | `products` | AppMeasurementクエリパラメータ製品のマーチャンダイジングイベント/Evarのマッピング。 |
| `productlistitems.[N].name` | `products` | AppMeasurementクエリパラメーターの製品名のマッピング。 |
| `productlistitems.[N].priceTotal` | `products` | AppMeasurementクエリパラメーターの製品価格のマッピング。 |
| `productlistitems.[N].quantity` | `products` | AppMeasurementクエリパラメーターの製品数量のマッピング。 |
| `web.webInteraction.URL` | `pev1` | AppMeasurementクエリパラメーターPAGE_イベント_VAR1のマッピング。 |
| `web.webInteraction.name` | `pev2` | AppMeasurementクエリパラメーターPAGE_イベント_VAR2のマッピング。 |
| `web.webInteraction.type` | `pe` | `web.webInteraction.type=other` に変更 `pe=lnk_o`します。 `web.webInteraction.type=download` に変更 `pe=lnk_d`します。 `web.webInteraction.type=exit` to `pe=lnk_e` |
| `web.webPageDetails.URL` | `g` | AppMeasurement クエリパラメーター PAGE_URL のマッピング。 |
| `web.webPageDetails.errorPage` | `pageType` | 変換ERROR_PAGE_TYPEを伴うAppMeasurementクエリパラメーターPAGE_TYPE_FULLのマッピング。 |
| `web.webPageDetails.homePage` | `hp` | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター HOMEPAGE のマッピング。 |
| `web.webPageDetails.name` | `gn` | AppMeasurement クエリパラメーター PAGENAME のマッピング。 |
| `web.webPageDetails.server` | `sv` | AppMeasurement クエリパラメーター USER_SERVER のマッピング。 |
| `web.webPageDetails.siteSection` | `ch` | AppMeasurementクエリパラメーターのチャネルマッピング。 |
| `web.webReferrer.URL` | `r` | AppMeasurement クエリパラメーター REFERRER のマッピング。 |
