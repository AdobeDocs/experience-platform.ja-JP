---
title: Analyticsで自動的にマッピングされる変数
seo-title: AnalyticsでAdobe Experience Platform Web SDKと自動的にマッピングされる変数
description: AnalyticsでExperience Platform Web SDKを使用して自動的にマッピングされる変数について説明します。
seo-description: AnalyticsでExperience Platform Web SDKを使用して自動的にマッピングされる変数について説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）変数がAnalyticsに自動的にマッピングされる

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

以下に、Adobe Experience Platform Edge Networkが自動的にAnalyticsにマッピングする変数の一覧を示します。

| XDM Field Path | Analyticsクエリ文字列/HTTPヘッダー | 説明 |
| ---------- | ------------------------- | -------- |
| `environment.browserDetails.userAgent` | `User-Agent` | これは、HTTPヘッダーマッピングHEADER_USER_AGENTです。 |
| `environment.browserDetails.acceptLanguage` | `Accept-Language` | これは、HTTPヘッダーのマッピングであるHEADER_ACCEPT_LANGUAGEです。 |
| `environment.browserDetails.cookiesEnabled` | `k` | AppMeasurementクエリパラメーターCOOKIEと変換BOOLEAN_TO_YNとの対応付け。 |
| `environment.browserDetails.javaScriptVersion` | `j` | AppMeasurementクエリパラメーターJ_JSCRIPTのマッピング。 |
| `environment.browserDetails.javaEnabled` | `v` | AppMeasurementクエリパラメーターJAVA_ENABLEDと変換BOOLEAN_TO_YNとのマッピング。 |
| `environment.browserDetails.viewportHeight` | `bh` | AppMeasurementクエリパラメーターBROWSER_HEIGHTのマッピング。 |
| `environment.browserDetails.viewportWidth` | `bw` | AppMeasurementクエリパラメーターBROWSER_WIDTHのマッピング。 |
| `environment.connectionType` | `ct` | AppMeasurementクエリパラメーターCT_CONNECT_TYPEのマッピング。 |
| `device.colorDepth` | `c` | AppMeasurementクエリパラメーターC_COLORのマッピング。 |
| `placeContext.geo.stateProvince` | `state` | AppMeasurementクエリパラメーターのSTATEマッピング。 |
| `placeContext.geo.postalCode` | `zip` | AppMeasurementクエリパラメーターのZIPマッピング。 |
| `placeContext.geo.latitude` | `lat` | AppMeasurementクエリパラメーターのLATITUDEマッピング。 |
| `placeContext.geo.longitude` | `lon` | AppMeasurementクエリパラメーターの経度のマッピング。 |
| `web.webPageDetails.server` | `sv` | AppMeasurementクエリパラメーターUSER_SERVERのマッピング。 |
| `web.webPageDetails.name` | `gn` | AppMeasurementクエリパラメーターのPAGENAMEマッピング。 |
| `web.webPageDetails.URL` | `g` | AppMeasurementクエリパラメーターPAGE_URLのマッピング。 |
| `web.webPageDetails.homePage` | `hp` | AppMeasurementクエリパラメーターHOMEPAGEマッピングと変換BOOLEAN_TO_YN。 |
| `web.webReferrer.URL` | `r` | AppMeasurementクエリパラメーターのREFERRERマッピング。 |
| `application.id` | `c.a.appid` | AppMeasurementコンテキストデータマッ `c.a.appid` ピング。 |
| `application.launches.value` | `c.a.launches` | AppMeasurementコンテキストデータマッ `c.a.launches` ピング。 |
| `marketing.trackingCode` | `v0` | AppMeasurementクエリパラメーターのCAMPAIGNマッピング。 |
| `commerce.purchaseID` | `pi` | AppMeasurementクエリパラメーターのPURCHASEIDマッピング。 |
| `commerce.currencyCode` | `cc` | AppMeasurementクエリパラメーターのCURRENCYマッピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Identifier` | `a.media.name` | AppMeasurementコンテキストデータマッ `a.media.name` ピング。 |
| `media.mediaTimed.primaryAssetReference.xmpDM:duration` | `c.a.media.length` | AppMeasurementコンテキストデータマッ `c.a.media.length` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastContentType` | `c.a.contentType` | AppMeasurementコンテキストデータマッ `c.a.contentType` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.playerName` | `c.a.media.playerName` | AppMeasurementコンテキストデータマッ `c.a.media.playerName` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastChannel` | `c.a.media.channel` | AppMeasurementコンテキストデータマッ `c.a.media.channel` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.mediaSegmentView.value` | `c.a.media.segment` | AppMeasurementコンテキストデータマッ `c.a.media.segment` ピング。 |
| `media.mediaTimed.primaryAssetReference.dc:title` | `c.a.media.friendlyName` | AppMeasurementコンテキストデータマッ `c.a.media.friendlyName` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.playerSDKVersion.version` | `c.a.media.sdkVersion` | AppMeasurementコンテキストデータマッ `c.a.media.sdkVersion` ピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Name` | `c.a.media.show` | AppMeasurementコンテキストデータマッ `c.a.media.show` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.streamFormat` | `c.a.media.format` | AppMeasurementコンテキストデータマッ `c.a.media.format` ピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Season.iptc4xmpExt:Number` | `c.a.media.season` | AppMeasurementコンテキストデータマッ `c.a.media.season` ピング。 |
| `media.mediaTimed.primaryAssetReference.iptc4xmpExt:Episode.iptc4xmpExt:Number` | `c.a.media.episode` | AppMeasurementコンテキストデータマッ `c.a.media.episode` ピング。 |
| `media.mediaTimed.primaryAssetViewDetails.broadcastNetwork` | `c.a.media.network` | AppMeasurementコンテキストデータマッ `c.a.media.network` ピング。 |
| `media.mediaTimed.primaryAssetReference.showType` | `c.a.media.type` | コンバージョンVEDIO_SHOW_TYPE `c.a.media.type` を使用したAppMeasurementコンテキストデータのマッピング。 |
| `media.mediaTimed.primaryAssetViewDetails.sourceFeed` | `c.a.media.feed` | AppMeasurementコンテキストデータマッ `c.a.media.feed` ピング。 |
| `media.mediaTimed.timePlayed.value` | `c.a.media.timePlayed` | AppMeasurementコンテキストデータマッ `c.a.media.timePlayed` ピング。 |
| `media.mediaTimed.totalTimePlayed.value` | `c.a.media.totalTimePlayed` | AppMeasurementコンテキストデータマッ `c.a.media.totalTimePlayed` ピング。 |
| `media.mediaTimed.federated.value` | `c.a.media.federated` | AppMeasurementコンテキストデータマッ `c.a.media.federated` ピング。 |
| `media.mediaTimed.pauses.value` | `c.a.media.pauseCount` | AppMeasurementコンテキストデータマッ `c.a.media.pauseCount` ピング。 |
| `media.mediaTimed.pauseTime.value` | `c.a.media.pauseTime` | AppMeasurementコンテキストデータマッ `c.a.media.pauseTime` ピング。 |
| `media.mediaTimed.resumes.value` | `c.a.media.resume` | AppMeasurementコンテキストデータマッ `c.a.media.resume` ピング。 |
| `identitymap.ecid.[0].id` | `mid` | AppMeasurementクエリパラメーターのMIDマッピング。 |
