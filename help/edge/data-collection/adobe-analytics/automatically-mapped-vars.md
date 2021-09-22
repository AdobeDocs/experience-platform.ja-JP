---
title: Adobe Experience Platform Web SDKで自動的にマッピングされたAdobe Analytics変数
description: Adobe AnalyticsでExperience PlatformWeb SDKを使用して自動的にマッピングされる変数について説明します。
seo-description: Learn which variables are automatically mapped in Adobe Analytics with the Adobe Experience Platform Web SDK
keywords: adobe analytics；変数；analytics；自動マップ；自動マッピング；
exl-id: 856fada7-b62c-4fd2-9372-a19ae1cdec33
source-git-commit: 7809e64abab80f72af979e685f268c0799e74eca
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 35%

---

# [!DNL Analytics]に自動的にマッピングされる変数

以下に、Adobe Experience Platform Edge NetworkがAdobe Analyticsに自動的にマッピングする変数のリストを示します。 Adobe Analyticsデータ収集クエリーパラメーターに関する詳細については、『Analytics導入ガイド](https://experienceleague.adobe.com/docs/analytics/implementation/validate/query-parameters.html)』を参照してください。[

| XDM フィールドパス | [!DNL Analytics Query String] / HTTP ヘッダー | 説明 |
| ---------- | ------------------------- | ----------------------------------------- |
| application.id | c.a.appid | AppMeasurement コンテキストデータ `c.a.appid` のマッピング。 |
| application.launches.value | c.a.launches | AppMeasurement コンテキストデータ `c.a.launches` のマッピング。 |
| commerce.checkouts.id | イベント | `scCheckout` イベントのシリアル化。このフィールドを除外すると（つまり、シリアル化されていないイベントの場合）、システムは独自のID値を生成し、エンティティに割り当てます。 |
| commerce.checkouts.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_SC_CHECKOUTを使用したAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| commerce.order.currencyCode | cc | AppMeasurement クエリパラメーター CURRENCY のマッピング。 |
| commerce.order.purchaseID | pi | AppMeasurement クエリパラメーター PURCHASEID のマッピング。 |
| commerce.productListAdds.id | イベント | `scAdd` イベントのシリアル化。このフィールドを除外すると（つまり、シリアル化されていないイベントの場合）、システムは独自のID値を生成し、エンティティに割り当てます。 |
| commerce.productListAdds.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_SC_ADDを使用したAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| commerce.productListOpens.id | イベント | `scOpen` イベントのシリアル化。このフィールドを除外すると（つまり、シリアル化されていないイベントの場合）、システムは独自のID値を生成し、エンティティに割り当てます。 |
| commerce.productListOpens.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_SC_OPENを使用したAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| commerce.productListRemovals.id | イベント | `scRemove` イベントのシリアル化。このフィールドを除外すると（つまり、シリアル化されていないイベントの場合）、システムは独自のID値を生成し、エンティティに割り当てます。 |
| commerce.productListRemovals.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_SC_REMOVEを使用したAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| commerce.productListViews.id | イベント | `scView` イベントのシリアル化。このフィールドを除外すると（つまり、シリアル化されていないイベントの場合）、システムは独自のID値を生成し、エンティティに割り当てます。 |
| commerce.productListViews.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_SC_VIEWとのAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| commerce.productViews.id | イベント | `prodView` イベントのシリアル化。このフィールドを除外すると（つまり、シリアル化されていないイベントの場合）、システムは独自のID値を生成し、エンティティに割り当てます。 |
| commerce.productViews.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_PROD_VIEWとのAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| commerce.purchases.value | イベント | 区切り文字`,`を使用した、コンバージョンCOMMERCE_PURCHASEとのAppMeasurementクエリパラメーターEVENT_LIST_FULLのマッピング。 |
| device.colorDepth | c | AppMeasurement クエリパラメーター C_COLOR のマッピング。 |
| device.screenHeight | s | AppMeasurementクエリパラメーターの画面解像度のマッピング。 |
| device.screenWidth | s | AppMeasurementクエリパラメーターの画面解像度のマッピング。 |
| environment.browserDetails.acceptLanguage | Accept-Language | HTTP ヘッダーマッピング（HEADER_ACCEPT_LANGUAGE）です。 |
| environment.browserDetails.cookiesEnabled | k | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター COOKIES のマッピング。 |
| environment.browserDetails.javaEnabled | v | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター JAVA_ENABLED のマッピング。 |
| environment.browserDetails.javaScriptVersion | j | AppMeasurement クエリパラメーター J_JSCRIPT のマッピング。 |
| environment.browserDetails.userAgent | User-Agent | HTTP ヘッダーマッピング（HEADER_USER_AGENT）です。 |
| environment.browserDetails.viewportHeight | bh | AppMeasurement クエリパラメーター BROWSER_HEIGHT のマッピング。 |
| environment.browserDetails.viewportWidth | bw | AppMeasurement クエリパラメーター BROWSER_WIDTH のマッピング。 |
| environment.connectionType | ct | AppMeasurement クエリパラメーター CT_CONNECT_TYPE のマッピング。 |
| environment.ipV4 | X-Forwarded-For | これはHTTPヘッダーマッピングで、X-FORWARDED-FORです。 |
| identityMap.ECID[0].id | mid | AppMeasurement クエリパラメーター MID のマッピング。 |
| marketing.trackingCode | v0 | AppMeasurement クエリパラメーター CAMPAIGN のマッピング。 |
| media.mediaTimed.completes.value | c.a.media.complete | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.dropBeforeStart.value | c.a.media.view, c.a.media.timePlayed, c.a.media.play | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.federated.value | c.a.media.federated | AppMeasurement コンテキストデータ `c.a.media.federated` のマッピング。 |
| media.mediaTimed.firstQuartiles.value | c.a.media.progress25 | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.mediaSegmentView.value | c.a.media.segmentView | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.midpoints.value | c.a.media.progress50 | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.pauseTime.value | c.a.media.pauseTime | AppMeasurement コンテキストデータ `c.a.media.pauseTime` のマッピング。 |
| media.mediaTimed.pauses.value | c.a.media.pauseCount | AppMeasurement コンテキストデータ `c.a.media.pauseCount` のマッピング。 |
| media.mediaTimed.primaryAssetReference.@id | c.a.media.asset | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.dc:title | c.a.media.friendlyName | AppMeasurement コンテキストデータ `c.a.media.friendlyName` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Creator[N].iptc4xmpExt:Name | c.a.media.originator | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Episode.iptc4xmpExt:Number | c.a.media.episode | AppMeasurement コンテキストデータ `c.a.media.episode` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Genre | c.a.media.genre | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Rating[N].iptc4xmpExt:RatingValue | c.a.media.rating | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Season.iptc4xmpExt:Number | c.a.media.season | AppMeasurement コンテキストデータ `c.a.media.season` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Identifier | a.media.name | AppMeasurement コンテキストデータ `a.media.name` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Name | c.a.media.show | AppMeasurement コンテキストデータ `c.a.media.show` のマッピング。 |
| media.mediaTimed.primaryAssetReference.showType | c.a.media.type | コンバージョン VEDIO_SHOW_TYPE を使用した AppMeasurement コンテキストデータ `c.a.media.type` のマッピング。 |
| media.mediaTimed.primaryAssetReference.showType | c.a.media.type | コンバージョンVIDEO_SHOW_TYPEを使用したAppMeasurementコンテキストデータ`c.a.media.type`のマッピング。 |
| media.mediaTimed.primaryAssetReference.xmpDM:duration | c.a.media.length | AppMeasurement コンテキストデータ `c.a.media.length` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.@id | c.a.media.vsid | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.primaryAssetViewDetails.broadcastChannel | c.a.media.channel | AppMeasurement コンテキストデータ `c.a.media.channel` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.broadcastContentType | c.a.contentType | AppMeasurement コンテキストデータ `c.a.contentType` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.broadcastNetwork | c.a.media.network | AppMeasurement コンテキストデータ `c.a.media.network` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.mediaSegmentView.value | c.a.media.segment | AppMeasurement コンテキストデータ `c.a.media.segment` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.playerName | c.a.media.playerName | AppMeasurement コンテキストデータ `c.a.media.playerName` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.playerSDKVersion.version | c.a.media.sdkVersion | AppMeasurement コンテキストデータ `c.a.media.sdkVersion` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.sourceFeed | c.a.media.feed | AppMeasurement コンテキストデータ `c.a.media.feed` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.streamFormat | c.a.media.format | AppMeasurement コンテキストデータ `c.a.media.format` のマッピング。 |
| media.mediaTimed.progress10.value | c.a.media.progress10 | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.progress95.value | c.a.media.progress95 | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.resumes.value | c.a.media.resume | AppMeasurement コンテキストデータ `c.a.media.resume` のマッピング。 |
| media.mediaTimed.starts.value | c.a.media.view | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.thirdQuartiles.value | c.a.media.progress75 | AppMeasurementコンテキストデータ。 |
| media.mediaTimed.timePlayed.value | c.a.media.timePlayed | AppMeasurement コンテキストデータ `c.a.media.timePlayed` のマッピング。 |
| media.mediaTimed.totalTimePlayed.value | c.a.media.totalTimePlayed | AppMeasurement コンテキストデータ `c.a.media.totalTimePlayed` のマッピング。 |
| placeContext.geo.latitude | lat | AppMeasurement クエリパラメーター LATITUDE のマッピング。 |
| placeContext.geo.longitude | lon | AppMeasurement クエリパラメーター LONGITUDE のマッピング。 |
| placeContext.geo.postalCode | 郵便番号 | AppMeasurement クエリパラメーター ZIP のマッピング。 |
| placeContext.geo.stateProvince | state | AppMeasurement クエリパラメーター STATE のマッピング。 |
| productListItems[N].lineItemId | 製品 | AppMeasurementクエリパラメーター「製品カテゴリ」のマッピング。 |
| productlistitems[N].name | 製品 | AppMeasurementクエリパラメーター「製品名」のマッピング。 |
| productlistitems[N].priceTotal | 製品 | AppMeasurementクエリパラメーター「製品価格」のマッピング。 |
| productlistitems[N].quantity | 製品 | AppMeasurementクエリパラメーター「製品数量」のマッピング。 |
| web.webInteraction.URL | pev1 | AppMeasurementクエリパラメーターPAGE_EVENT_VAR1のマッピング。 |
| web.webInteraction.name | pev2 | AppMeasurementクエリパラメーターPAGE_EVENT_VAR2のマッピング。 |
| web.webInteraction.type | pe | `web.webInteraction.type=other` を次の値に設定 `pe=lnk_o`します。 `web.webInteraction.type=download` を次の値に設定 `pe=lnk_d`します。 `web.webInteraction.type=exit` を  `pe=lnk_e` |
| web.webPageDetails.URL | g | AppMeasurement クエリパラメーター PAGE_URL のマッピング。 |
| web.webPageDetails.errorPage | pageType | コンバージョンERROR_PAGE_TYPEを使用したAppMeasurementクエリパラメーターPAGE_TYPE_FULLのマッピング。 |
| web.webPageDetails.homePage | hp | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター HOMEPAGE のマッピング。 |
| web.webPageDetails.name | gn | AppMeasurement クエリパラメーター PAGENAME のマッピング。 |
| web.webPageDetails.server | sv | AppMeasurement クエリパラメーター USER_SERVER のマッピング。 |
| web.webPageDetails.siteSection | ch | AppMeasurementクエリパラメーターCHANNELのマッピング。 |
| web.webReferrer.URL | r | AppMeasurement クエリパラメーター REFERRER のマッピング。 |

{style=&quot;table-layout:auto&quot;}
