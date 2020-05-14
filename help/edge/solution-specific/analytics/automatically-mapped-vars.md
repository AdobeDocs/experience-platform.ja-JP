---
title: Analytics で自動的にマッピングされる変数
seo-title: Adobe Experience Platform Web SDK を使用して Analytics で自動的にマッピングされる変数
description: Adobe Experience Platform Web SDK を使用して Analytics で自動的にマッピングされる変数について説明します
seo-description: Adobe Experience Platform Web SDK を使用して Analytics で自動的にマッピングされる変数について説明します
translation-type: tm+mt
source-git-commit: e9fb726ddb84d7a08afb8c0f083a643025b0f903
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 100%

---


# Analytics で自動的にマッピングされる変数

以下に、Adobe Experience Platform Edge Network が自動的に Analytics へとマッピングする変数の一覧を示します。

| XDM フィールドパス | Analytics クエリ文字列／HTTP ヘッダー | 説明 |
| ---------- | ------------------------- | -------- |
| `environment.browserDetails.userAgent` | `User-Agent` | HTTP ヘッダーマッピング（HEADER_USER_AGENT）です。 |
| `environment.browserDetails.acceptLanguage` | `Accept-Language` | HTTP ヘッダーマッピング（HEADER_ACCEPT_LANGUAGE）です。 |
| `environment.browserDetails.cookiesEnabled` | `k` | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター COOKIES のマッピング。 |
| `environment.browserDetails.javaScriptVersion` | `j` | AppMeasurement クエリパラメーター J_JSCRIPT のマッピング。 |
| `environment.browserDetails.javaEnabled` | `v` | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター JAVA_ENABLED のマッピング。 |
| `environment.browserDetails.viewportHeight` | `bh` | AppMeasurement クエリパラメーター BROWSER_HEIGHT のマッピング。 |
| `environment.browserDetails.viewportWidth` | `bw` | AppMeasurement クエリパラメーター BROWSER_WIDTH のマッピング。 |
| `environment.connectionType` | `ct` | AppMeasurement クエリパラメーター CT_CONNECT_TYPE のマッピング。 |
| `device.colorDepth` | `c` | AppMeasurement クエリパラメーター C_COLOR のマッピング。 |
| `placeContext.geo.stateProvince` | `state` | AppMeasurement クエリパラメーター STATE のマッピング。 |
| `placeContext.geo.postalCode` | `zip` | AppMeasurement クエリパラメーター ZIP のマッピング。 |
| `placeContext.geo.latitude` | `lat` | AppMeasurement クエリパラメーター LATITUDE のマッピング。 |
| `placeContext.geo.longitude` | `lon` | AppMeasurement クエリパラメーター LONGITUDE のマッピング。 |
| `web.webPageDetails.server` | `sv` | AppMeasurement クエリパラメーター USER_SERVER のマッピング。 |
| `web.webPageDetails.name` | `gn` | AppMeasurement クエリパラメーター PAGENAME のマッピング。 |
| `web.webPageDetails.URL` | `g` | AppMeasurement クエリパラメーター PAGE_URL のマッピング。 |
| `web.webPageDetails.homePage` | `hp` | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター HOMEPAGE のマッピング。 |
| `web.webReferrer.URL` | `r` | AppMeasurement クエリパラメーター REFERRER のマッピング。 |
| `application.id` | `c.a.appid` | AppMeasurement コンテキストデータ `c.a.appid` のマッピング。 |
| `application.launches.value` | `c.a.launches` | AppMeasurement コンテキストデータ `c.a.launches` のマッピング。 |
| `marketing.trackingCode` | `v0` | AppMeasurement クエリパラメーター CAMPAIGN のマッピング。 |
| `commerce.purchaseID` | `pi` | AppMeasurement クエリパラメーター PURCHASEID のマッピング。 |
| `commerce.currencyCode` | `cc` | AppMeasurement クエリパラメーター CURRENCY のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Identifier` | `a.media.name` | AppMeasurement コンテキストデータ `a.media.name` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.xmpDM:duration` | `c.a.media.length` | AppMeasurement コンテキストデータ `c.a.media.length` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastContentType` | `c.a.contentType` | AppMeasurement コンテキストデータ `c.a.contentType` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.playerName` | `c.a.media.playerName` | AppMeasurement コンテキストデータ `c.a.media.playerName` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastChannel` | `c.a.media.channel` | AppMeasurement コンテキストデータ `c.a.media.channel` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.mediaSegmentView.value` | `c.a.media.segment` | AppMeasurement コンテキストデータ `c.a.media.segment` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.dc:title` | `c.a.media.friendlyName` | AppMeasurement コンテキストデータ `c.a.media.friendlyName` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.playerSDKVersion.version` | `c.a.media.sdkVersion` | AppMeasurement コンテキストデータ `c.a.media.sdkVersion` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Name` | `c.a.media.show` | AppMeasurement コンテキストデータ `c.a.media.show` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.streamFormat` | `c.a.media.format` | AppMeasurement コンテキストデータ `c.a.media.format` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Season.iptc4xmpExt:Number` | `c.a.media.season` | AppMeasurement コンテキストデータ `c.a.media.season` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Episode.iptc4xmpExt:Number` | `c.a.media.episode` | AppMeasurement コンテキストデータ `c.a.media.episode` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastNetwork` | `c.a.media.network` | AppMeasurement コンテキストデータ `c.a.media.network` のマッピング。 |
| `media.mediaTimed.primaryAssetReference.showType` | `c.a.media.type` | コンバージョン VEDIO_SHOW_TYPE を使用した AppMeasurement コンテキストデータ `c.a.media.type` のマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.sourceFeed` | `c.a.media.feed` | AppMeasurement コンテキストデータ `c.a.media.feed` のマッピング。 |
| `media.mediaTimed.timePlayed.value` | `c.a.media.timePlayed` | AppMeasurement コンテキストデータ `c.a.media.timePlayed` のマッピング。 |
| `media.mediaTimed.totalTimePlayed.value` | `c.a.media.totalTimePlayed` | AppMeasurement コンテキストデータ `c.a.media.totalTimePlayed` のマッピング。 |
| `media.mediaTimed.federated.value` | `c.a.media.federated` | AppMeasurement コンテキストデータ `c.a.media.federated` のマッピング。 |
| `media.mediaTimed.pauses.value` | `c.a.media.pauseCount` | AppMeasurement コンテキストデータ `c.a.media.pauseCount` のマッピング。 |
| `media.mediaTimed.pauseTime.value` | `c.a.media.pauseTime` | AppMeasurement コンテキストデータ `c.a.media.pauseTime` のマッピング。 |
| `media.mediaTimed.resumes.value` | `c.a.media.resume` | AppMeasurement コンテキストデータ `c.a.media.resume` のマッピング。 |
| `identitymap.ecid.[0].id` | `mid` | AppMeasurement クエリパラメーター MID のマッピング。 |
