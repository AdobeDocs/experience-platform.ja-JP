---
title: Adobe Experience Platform Web SDK で自動的にマッピングされたAdobe Analytics変数
description: Experience PlatformWeb SDK を使用してAdobe Analyticsで自動的にマッピングされる変数について説明します
seo-description: Learn which variables are automatically mapped in Adobe Analytics with the Adobe Experience Platform Web SDK
keywords: adobe analytics；変数；analytics；自動マップ；自動マッピング；
exl-id: 856fada7-b62c-4fd2-9372-a19ae1cdec33
source-git-commit: dcbe4c1b5a085878562990ed2db8e5cb27b93e28
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 35%

---

# 変数は、 [!DNL Analytics]

以下に、Adobe Experience Platform Edge Network がAdobe Analyticsに自動的にマッピングする変数のリストを示します。 Adobe Analyticsのデータ収集クエリーパラメーターについて詳しくは、 [Analytics 実装ガイド](https://experienceleague.adobe.com/docs/analytics/implementation/validate/query-parameters.html?lang=ja).

>[!NOTE]
>このページの情報は、Mobile SDKAdobeにも当てはまります。

| XDM フィールドパス | [!DNL Analytics Query String] / HTTP ヘッダー | 説明 |
| ---------- | ------------------------- | ----------------------------------------- |
| application.id | c.a.appid | AppMeasurement コンテキストデータ `c.a.appid` のマッピング。 |
| application.launches.value | c.a.launches | AppMeasurement コンテキストデータ `c.a.launches` のマッピング。 |
| commerce.checkouts.id | イベント | `scCheckout` イベントのシリアル化。 このフィールドを除外する（シリアル化されていないイベントの場合）と、システムは独自の ID 値を生成してエンティティに割り当てます。 |
| commerce.checkouts.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_SC_CHECKOUT を使用した AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| commerce.order.currencyCode | cc | AppMeasurement クエリパラメーター CURRENCY のマッピング。 |
| commerce.order.purchaseID | pi | AppMeasurement クエリパラメーター PURCHASEID のマッピング。 |
| commerce.productListAdds.id | イベント | `scAdd` イベントのシリアル化。 このフィールドを除外する（シリアル化されていないイベントの場合）と、システムは独自の ID 値を生成してエンティティに割り当てます。 |
| commerce.productListAdds.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_SC_ADD を使用した AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| commerce.productListOpens.id | イベント | `scOpen` イベントのシリアル化。 このフィールドを除外する（シリアル化されていないイベントの場合）と、システムは独自の ID 値を生成してエンティティに割り当てます。 |
| commerce.productListOpens.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_SC_OPEN を伴う AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| commerce.productListRemovals.id | イベント | `scRemove` イベントのシリアル化。 このフィールドを除外する（シリアル化されていないイベントの場合）と、システムは独自の ID 値を生成してエンティティに割り当てます。 |
| commerce.productListRemovals.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_SC_REMOVE を使用した AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| commerce.productListViews.id | イベント | `scView` イベントのシリアル化。 このフィールドを除外する（シリアル化されていないイベントの場合）と、システムは独自の ID 値を生成してエンティティに割り当てます。 |
| commerce.productListViews.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_SC_VIEW を使用した AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| commerce.productViews.id | イベント | `prodView` イベントのシリアル化。 このフィールドを除外する（シリアル化されていないイベントの場合）と、システムは独自の ID 値を生成してエンティティに割り当てます。 |
| commerce.productViews.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_PROD_VIEW を使用した AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| commerce.purchases.value | イベント | 区切り文字を使用した、コンバージョン COMMERCE_PURCHASE を使用した AppMeasurement クエリパラメーター EVENT_LIST_FULL のマッピング `,`. |
| device.colorDepth | c | AppMeasurement クエリパラメーター C_COLOR のマッピング。 |
| device.screenHeight | s | AppMeasurement クエリパラメーター画面解像度のマッピング。 |
| device.screenWidth | s | AppMeasurement クエリパラメーター画面解像度のマッピング。 |
| environment.browserDetails.acceptLanguage | Accept-Language | HTTP ヘッダーマッピング（HEADER_ACCEPT_LANGUAGE）です。 |
| environment.browserDetails.cookiesEnabled | k | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター COOKIES のマッピング。 |
| environment.browserDetails.javaEnabled | v | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター JAVA_ENABLED のマッピング。 |
| environment.browserDetails.javaScriptVersion | j | AppMeasurement クエリパラメーター J_JSCRIPT のマッピング。 |
| environment.browserDetails.userAgent | User-Agent | HTTP ヘッダーマッピング（HEADER_USER_AGENT）です。 |
| environment.browserDetails.viewportHeight | bh | AppMeasurement クエリパラメーター BROWSER_HEIGHT のマッピング。 |
| environment.browserDetails.viewportWidth | bw | AppMeasurement クエリパラメーター BROWSER_WIDTH のマッピング。 |
| environment.connectionType | ct | AppMeasurement クエリパラメーター CT_CONNECT_TYPE のマッピング。 |
| environment.ipV4 | X-Forwarded-For | これは HTTP ヘッダーマッピングで、X-FORWARDED-FOR です。 |
| identityMap.ECID[0].id | mid | AppMeasurement クエリパラメーター MID のマッピング。 |
| marketing.trackingCode | v0 | AppMeasurement クエリパラメーター CAMPAIGN のマッピング。 |
| media.mediaTimed.completes.value | c.a.media.complete | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.dropBeforeStart.value | c.a.media.view, c.a.media.timePlayed, c.a.media.play | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.federated.value | c.a.media.federated | AppMeasurement コンテキストデータ `c.a.media.federated` のマッピング。 |
| media.mediaTimed.firstQuartiles.value | c.a.media.progress25 | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.mediaSegmentView.value | c.a.media.segmentView | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.midpoints.value | c.a.media.progress50 | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.pauseTime.value | c.a.media.pauseTime | AppMeasurement コンテキストデータ `c.a.media.pauseTime` のマッピング。 |
| media.mediaTimed.pauses.value | c.a.media.pauseCount | AppMeasurement コンテキストデータ `c.a.media.pauseCount` のマッピング。 |
| media.mediaTimed.primaryAssetReference.@id | c.a.media.asset | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.dc:title | c.a.media.friendlyName | AppMeasurement コンテキストデータ `c.a.media.friendlyName` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Creator[N].iptc4xmpExt:Name | c.a.media.originator | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Episode.iptc4xmpExt:Number | c.a.media.episode | AppMeasurement コンテキストデータ `c.a.media.episode` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Genre | c.a.media.genre | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Rating[N].iptc4xmpExt:RatingValue | c.a.media.rating | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Season.iptc4xmpExt:Number | c.a.media.season | AppMeasurement コンテキストデータ `c.a.media.season` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Identifier | a.media.name | AppMeasurement コンテキストデータ `a.media.name` のマッピング。 |
| media.mediaTimed.primaryAssetReference.iptc4xmpExt:Series.iptc4xmpExt:Name | c.a.media.show | AppMeasurement コンテキストデータ `c.a.media.show` のマッピング。 |
| media.mediaTimed.primaryAssetReference.showType | c.a.media.type | コンバージョン VEDIO_SHOW_TYPE を使用した AppMeasurement コンテキストデータ `c.a.media.type` のマッピング。 |
| media.mediaTimed.primaryAssetReference.showType | c.a.media.type | AppMeasurement コンテキストデータ `c.a.media.type` 変換 VIDEO_SHOW_TYPE を使用したマッピング。 |
| media.mediaTimed.primaryAssetReference.xmpDM:duration | c.a.media.length | AppMeasurement コンテキストデータ `c.a.media.length` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.@id | c.a.media.vsid | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.primaryAssetViewDetails.broadcastChannel | c.a.media.channel | AppMeasurement コンテキストデータ `c.a.media.channel` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.broadcastContentType | c.a.contentType | AppMeasurement コンテキストデータ `c.a.contentType` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.broadcastNetwork | c.a.media.network | AppMeasurement コンテキストデータ `c.a.media.network` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.mediaSegmentView.value | c.a.media.segment | AppMeasurement コンテキストデータ `c.a.media.segment` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.playerName | c.a.media.playerName | AppMeasurement コンテキストデータ `c.a.media.playerName` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.playerSDKVersion.version | c.a.media.sdkVersion | AppMeasurement コンテキストデータ `c.a.media.sdkVersion` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.sourceFeed | c.a.media.feed | AppMeasurement コンテキストデータ `c.a.media.feed` のマッピング。 |
| media.mediaTimed.primaryAssetViewDetails.streamFormat | c.a.media.format | AppMeasurement コンテキストデータ `c.a.media.format` のマッピング。 |
| media.mediaTimed.progress10.value | c.a.media.progress10 | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.progress95.value | c.a.media.progress95 | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.resumes.value | c.a.media.resume | AppMeasurement コンテキストデータ `c.a.media.resume` のマッピング。 |
| media.mediaTimed.starts.value | c.a.media.view | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.thirdQuartiles.value | c.a.media.progress75 | AppMeasurement コンテキストデータ。 |
| media.mediaTimed.timePlayed.value | c.a.media.timePlayed | AppMeasurement コンテキストデータ `c.a.media.timePlayed` のマッピング。 |
| media.mediaTimed.totalTimePlayed.value | c.a.media.totalTimePlayed | AppMeasurement コンテキストデータ `c.a.media.totalTimePlayed` のマッピング。 |
| placeContext.geo.latitude | lat | AppMeasurement クエリパラメーター LATITUDE のマッピング。 |
| placeContext.geo.longitude | lon | AppMeasurement クエリパラメーター LONGITUDE のマッピング。 |
| placeContext.geo.postalCode | 郵便番号 | AppMeasurement クエリパラメーター ZIP のマッピング。 |
| placeContext.geo.stateProvince | state | AppMeasurement クエリパラメーター STATE のマッピング。 |
| productListItems[N].lineItemId | 製品 | AppMeasurement クエリパラメーター「製品カテゴリ」のマッピング。 |
| productlistitems[N].name | 製品 | AppMeasurement クエリパラメーター「製品名」のマッピング。 |
| productlistitems[N].priceTotal | 製品 | AppMeasurement クエリパラメーター「製品価格」のマッピング。 |
| productlistitems[N].quantity | 製品 | AppMeasurement クエリパラメーター「製品数量」のマッピング。 |
| web.webInteraction.URL | pev1 | AppMeasurement クエリパラメーター PAGE_EVENT_VAR1 のマッピング。 |
| web.webInteraction.name | pev2 | AppMeasurement クエリパラメーター PAGE_EVENT_VAR2 のマッピング。 |
| web.webInteraction.type | pe | `web.webInteraction.type=other` から `pe=lnk_o`; `web.webInteraction.type=download` から `pe=lnk_d`; `web.webInteraction.type=exit` から `pe=lnk_e` |
| web.webPageDetails.URL | g | AppMeasurement クエリパラメーター PAGE_URL のマッピング。 |
| web.webPageDetails.errorPage | pageType | コンバージョン ERROR_PAGE_TYPE を使用した AppMeasurement クエリパラメーター PAGE_TYPE_FULL のマッピング。 |
| web.webPageDetails.homePage | hp | コンバージョン BOOLEAN_TO_YN を使用した AppMeasurement クエリパラメーター HOMEPAGE のマッピング。 |
| web.webPageDetails.name | gn | AppMeasurement クエリパラメーター PAGENAME のマッピング。 |
| web.webPageDetails.server | sv | AppMeasurement クエリパラメーター USER_SERVER のマッピング。 |
| web.webPageDetails.siteSection | ch | AppMeasurement クエリパラメーター CHANNEL のマッピング。 |
| web.webReferrer.URL | r | AppMeasurement クエリパラメーター REFERRER のマッピング。 |

{style="table-layout:auto"}
