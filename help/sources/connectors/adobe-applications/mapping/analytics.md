---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Analyticsマッピングフィールド
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '3328'
ht-degree: 12%

---


# Analyticsマッピングフィールド

Adobe Experience Platformを使用すると、Analyticsデータコネクタ(ADC)を通じてアドビのAnalyticsデータを取り込むことができます。 ADCを通じて取り込まれるデータの一部は、Analyticsフィールドからエクスペリエンスデータモデル(XDM)フィールドに直接マップできますが、正常にマップされるためには、変換と特定の機能が必要です。

![](../images/analytics-data-experience-platform.png)

## 直接マッピングフィールド

選択したフィールドは、AdobeAnalyticsからExperience Data Model(XDM)に直接マッピングされます。

次の表に、Analyticsフィールド名(*Analyticsフィールド*)、対応するXDMフィールド名(*XDMフィールド*)、そのタイプ(*XDMタイプ*)を示す列と、フィールド&#x200B;*説明(*&#x200B;説明)の説明を示す列を示します。

>[!NOTE]
>
>左右にスクロールして、テーブルの全内容を表示してください。

| Analytics場 | XDM フィールド | XDMタイプ | 説明 |
| --------------- | --------- | -------- | ---------- |
| m_evar1 ～ m_evar250 | _experience.analytics.customDimensions.eVars.eVar1 - _experience.analytics.customDimensions.eVars.eVar250 | string | カスタム変数。1 ～ 250の値を指定できます。 各組織でこれらのカスタムeVarの使用方法が異なります。 |
| m_prop1 ～ m_prop75 | _experience.analytics.customDimensions.props.prop1 - _experience.analytics.customDimensions.props.prop75 | string | カスタムトラフィック変数。1 ～ 75の値を指定できます。 |
| m_browser | _experience.analytics.環境.browserID | integer | ブラウザーの番号ID。 |
| m_browser_height | environment.browserDetails.viewportHeight | integer | ブラウザーの高さ（ピクセル単位）。 |
| m_browser_width | environment.browserDetails.viewportWidth | integer | ブラウザーの幅（ピクセル単位）。 |
| m_キャンペーン | marketing.trackingCode | string | トラッキングコードディメンションで使用される変数。 |
| m_チャネル | web.webPageDetails.siteSection | string | サイトセクションディメンションで使用される変数。 |
| m_domain | environment.domain | string | Domainディメンションで使用される変数。 これは、ユーザーのインターネットサービスプロバイダー(ISP)に基づきます。 |
| m_geo_city | placeContext.geo.city | string | ヒットの市区町村の名前。 これは、ヒットのIPアドレスに基づいています。 |
| m_geo_dma | placeContext.geo.dmaID | integer | ヒットの人口統計領域の数値ID。 これは、ヒットのIPアドレスに基づいています。 |
| m_geo_region | placeContext.geo.stateProvince | string | ヒットの状態または地域の名前。 これは、ヒットのIPアドレスに基づいています。 |
| m_geo_zip | placeContext.geo.postalCode | string | ヒットの郵便番号。 これは、ヒットのIPアドレスに基づいています。 |
| m_keywords | search.keywords | string | Keywordディメンションで使用される変数。 |
| m_os | _experience.analytics.環境.operatingSystemID | integer | 訪問者のオペレーティングシステムを表す数値ID。 これは、user_agent列に基づきます。 |
| m_page_url | web.webPageDetails.URL | string | ページヒットのURL。 |
| m_pagename_no_url | web.webPageDetails.</span>name | string | Pagesディメンションの入力に使用される変数です。 |
| m_転送者 | web.webReferrer.URL | string | 前のページのページURL。 |
| m_search_page_num | search.pageDepth | integer | すべての検索ページのランクディメンションで使用されます。 ユーザーがサイトにクリックスルーする前にサイトが表示された検索結果のページを示します。 |
| m_state | _experience.analytics.customDimensions.stateProvince | string | 状態変数。 |
| m_user_server | web.webPageDetails.server | string | Serverディメンションで使用される変数。 |
| m_zip | _experience.analytics.customDimensions.postalCode | string | 郵便番号ディメンションの入力に使用される変数。 |
| accept_language | environment.browserDetails.acceptLanguage | string | Accept-Language HTTPヘッダーに示されているとおり、許可されたすべての言語をリストします。 |
| homepage | web.webPageDetails.isHomePage | ブール型 | 廃止。現在のURLがブラウザーのホームページであるかどうかを示します。 |
| ipv6 | environment.ipV6 | string |
| j_jscript | environment.browserDetails.javaScriptVersion | string | ブラウザーでサポートされているJavaScriptのバージョン。 |
| user_agent | environment.browserDetails.userAgent | string | HTTPヘッダーで送信されるユーザーエージェント文字列。 |
| mobileappid | application.</span>name | string | モバイルアプリIDは、次の形式で保存されます。 `[AppName][BundleVersion]`. |
| mobiledevice | device.model | string | モバイルデバイスの名前。 iOSでは、コンマ区切りの2桁の文字列として保存されます。 最初の番号はデバイスの生成を表し、2番目の番号はデバイスファミリを表します。 |
| pointofinterest | placeContext.POIinteraction.POIDetail.</span>name | string | モバイルサービスで使用されます。 目標地点を表します。 |
| pointofinterestdistance | placeContext.POIinteraction.POIDetail.geoInteractionDetails.distanceToCenter | 数値 | モバイルサービスで使用されます。 目標点の距離を表します。 |
| mobileplaceaccuracy | placeContext.POIinteraction.POIDetail.geoInteractionDetails.deviceGeoAccuracy | 数値 | コンテキストデータ変数 a.loc.acc から収集します。収集時の GPS の精度をメートル単位で示します。 |
| mobileplacecategory | placeContext.POIinteraction.POIDetail.category | string | コンテキストデータ変数 a.loc.category から収集します。特定の場所のカテゴリを示します。 |
| mobileplaceid | placeContext.POIinteraction.POIDetail.POIID | string | コンテキストデータ変数a.loc.idから収集されます。 特定の目標地点の識別子。 |
| video | media.mediaTimed.primaryAssetReference._id | string |  ビデオの名前。 |
| videoad | advertising.adAssetReference._id | string | 広告アセットの識別子。 |
| videocontenttype | media.mediaTimed.primaryAssetViewDetails.broadcastContentType | string | ビデオコンテンツタイプ。 これは、すべてのビデオ表示で自動的に「ビデオ」に設定されます。 |
| videoadpod | advertising.adAssetViewDetails.adBreak._id | string | ビデオ広告が含まれているポッド。 |
| videoadinpod | advertising.adAssetViewDetails.index | integer | ビデオ広告がポッド内の位置。 |
| videoplayername | media.mediaTimed.primaryAssetViewDetails.playerName | string | ビデオプレーヤーの名前。 |
| videochannel | media.mediaTimed.primaryAssetViewDetails.broadcastChannel | string | ビデオチャネル。 |
| videoadplayername | advertising.adAssetViewDetails.playerName | string | ビデオ広告プレイヤーの名前。 |
| videochapter | media.mediaTimed.mediaChapter.chapterAssetReference._id | string | ビデオチャプター名 |
| videoname | media.mediaTimed.primaryAssetReference._dc.title | string | ビデオ名。 |
| videoadname | advertising.adAssetReference._dc.title | string | ビデオ広告の名前。 |
| videoshow | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Series._iptc4xmpExt.Name | string | ビデオショー. |
| videoseason | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Season._iptc4xmpExt.Name | string | ビデオシーズン。 |
| videoepisode | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Episode._iptc4xmpExt.Name | string | ビデオのエピソード. |
| videonetwork | media.mediaTimed.primaryAssetViewDetails.broadcastNetwork | string | ビデオネットワーク. |
| videoshowtype | media.mediaTimed.primaryAssetReference.showType | string | ビデオショーのタイプ. |
| videoadload | media.mediaTimed.primaryAssetViewDetails.adLoadType | string | ビデオ広告の読み込み. |
| videofeedtype | media.mediaTimed.primaryAssetViewDetails.sourceFeed | string | ビデオフィードのタイプ. |
| mobilebeaconmajor | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.beaconMajor | 数値 | Mobile Services ビーコンのメジャー番号. |
| mobilebeaconminor | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.beaconMinor | 数値 | Mobile Services ビーコンのマイナー番号. |
| mobilebeaconuuid | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.proximityUUID | string | Mobile Services ビーコンの UUID. |
| videosessionid | media.mediaTimed.primaryAssetViewDetails._id | string | ビデオセッションID。 |
| videogenre | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Genre | array | ビデオのジャンル. | {title （オブジェクト）、description （オブジェクト）、type （オブジェクト）、meta:xdmType （オブジェクト）、items （文字列）、meta:xdmField （オブジェクト）} |
| mobileinstalls | application.firstLaunches | オブジェクト | これは、インストールまたは再インストール後の最初の実行時にトリガーされます | {id （文字列），値（数値）} |
| mobileupgrades | application.upgrades | オブジェクト | アプリのアップグレード数を報告します。 アップグレード後、またはバージョン番号が変更された場合に、最初の実行時にトリガーされます。 | {id （文字列），値（数値）} |
| mobilelaunches | application.launches | オブジェクト | アプリが起動された回数。 | {id （文字列），値（数値）} |
| mobilecrashes | application.crashes | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| mobilemessageclicks | directMarketing.clicks | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| mobileplaceentry | placeContext.POIinteraction.poiEntries | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| mobileplaceexit | placeContext.POIinteraction.poiExits | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videotime | media.mediaTimed.timePlayed | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videostart | media.mediaTimed.impressions | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videocomplete | media.mediaTimed.completes | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videosegmentviews | media.mediaTimed.mediaSegmentViews | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoadstart | advertising.impressions | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoadcomplete | advertising.completes | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoadtime | advertising.timePlayed | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videochapterstart | media.mediaTimed.mediaChapter.impressions | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videochaptercomplete | media.mediaTimed.mediaChapter.completes | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videochaptertime | media.mediaTimed.mediaChapter.timePlayed | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoplay | media.mediaTimed.starts | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videototaltime | media.mediaTimed.totalTimePlayed | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoqoetimetostart | media.mediaTimed.primaryAssetViewDetails.qoe.timeToStart | オブジェクト | ビデオ画質の開始までの時間。 | {id （文字列），値（数値）} |
| videoqoedropbeforestart | media.mediaTimed.dropBeforeStarts | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoqoebuffercount | media.mediaTimed.primaryAssetViewDetails.qoe.buffers | オブジェクト | ビデオ画質バッファ数 | {id （文字列），値（数値）} |
| videoqoebuffertime | media.mediaTimed.primaryAssetViewDetails.qoe.bufferTime | オブジェクト | ビデオ画質バッファ時間 | {id （文字列），値（数値）} |
| videoqoebitratechangecount | media.mediaTimed.primaryAssetViewDetails.qoe.bitrateChanges | オブジェクト | ビデオ画質変更回数 | {id （文字列），値（数値）} |
| videoqoebitrateaverage | media.mediaTimed.primaryAssetViewDetails.qoe.bitrateAverage | オブジェクト | ビデオ画質平均ビットレート | {id （文字列），値（数値）} |
| videoqoeerrorcount | media.mediaTimed.primaryAssetViewDetails.qoe.errors | オブジェクト | ビデオ画質エラー数 | {id （文字列），値（数値）} |
| videoqoedroppedframecount | media.mediaTimed.primaryAssetViewDetails.qoe.droppedFrames | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoprogress10 | media.mediaTimed.progress10 | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoprogress25 | media.mediaTimed.progress25 | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoprogress50 | media.mediaTimed.progress50 | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoprogress75 | media.mediaTimed.progress75 | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoprogress95 | media.mediaTimed.progress95 | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videoresume | media.mediaTimed.resumes | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videopausecount | media.mediaTimed.pauses | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videopausetime | media.mediaTimed.pauseTime | オブジェクト | <!-- MISSING --> | {id （文字列），値（数値）} |
| videosecondsincelastcall | media.mediaTimed.primaryAssetViewDetails.sessionTimeout | integer |

## マッピングフィールドの分割

これらのフィールドは1つのソースを持ちますが、 **複数の** XDMの場所にマップされます。

| Analytics場 | XDM フィールド | XDMタイプ | 説明 |
| --------------- | --------- | -------- | ---------- |
| s_resolution | device.screenWidth、device.screenHeight | integer | モニターの解像度を表す数値ID。 |
| mobileosversion | 環境.operatingSystem、環境.operatingSystemVersion | string | モバイルオペレーティングシステムのバージョン。 |
| videoadlength | advertising.adAssetReference._xmpDM.duration | integer | ビデオ広告の長さ。 |

## 生成されたマッピングフィールド

ADCから来るフィールドを変換する必要があります。XDMで生成するには、アドビAnalyticsからの直接コピーを超えるロジックが必要です。

次の表に、Analyticsフィールド名(*Analyticsフィールド*)、対応するXDMフィールド名(*XDMフィールド*)、そのタイプ(*XDMタイプ*)を示す列と、フィールド&#x200B;*説明(*&#x200B;説明)の説明を示す列を示します。

>[!NOTE]
>
>左右にスクロールして、テーブルの全内容を表示してください。

| Analytics場 | XDM フィールド | XDMタイプ | 説明 |
| --------------- | --------- | -------- | ----------- |
| m_prop1 ～ m_prop75 | _experience.analytics.customDimensions.listprops.prop1 - _experience.analytics.customDimensions.listprops.prop75 | オブジェクト | カスタムトラフィック変数(1 ～ 75) | {} |
| m_hier1 ～ m_hier5 | _experience.analytics.customDimensions.hier1 - _experience.analytics.customDimensions.hierarchies.hier5 | オブジェクト | 階層変数で使用されます。 これには、 | 区切り形式の値リスト。 | {値（配列），区切り文字（文字列）} |
| m_mvvar1 ～ m_mvvar3 | _experience.analytics.customDimensions.リスト.リスト1.リスト[] - _experience.analytics.customDimensions.リスト.リスト3.リスト[] | array | 変数値のリスト。 実装に応じて異なるカスタム値の区切りリストが含まれます | {値（文字列），キー（文字列）} |
| m_color | device.colorDepth | integer | 色深度ID。c_color列の値に基づきます。 |
| m_cookies | environment.browserDetails.cookiesEnabled | ブール型 | cookieサポートディメンションで使用される変数。 |
| m_イベント_リスト | commerce.purchases、commerce.productViews、commerce.productListOpens、commerce.checkouts、commerce.productListAdds、commerce.productListRemovals、commerce.productListViews | オブジェクト | ヒット時に標準コマースイベントがトリガーされた。 | {id （文字列），値（数値）} |
| m_イベント_リスト | _experience.analytics.イベント1to100.イベント1 - _experience.analytics.イベント1to100.イベント100, _experience.analytics.イベント101to200.イベント101 - _experience.analytics.イベント101to200.イベントイベント200, _experience.analytics.イベント201to300.イベント201 - _experience.analytics.イベント201to300.イベント300, _experience.analytics.イベント301to400.イベント301 — エクスペリエンス.analytics.イベント301to400.イベント400, _experience.analytics.イベント401to500.イベント401 - _experience.analytics.イベント401to500.イベント500, _experience.analytics.イベント5001to600.イベント501 - _experience.analytics.イベント501to600.イベント600, _experience.analytics.イベント601to700.イベント601 - _experience.analytics.イベント601to700.イベントイベント700, _experience.analytics.イベント701to800.イベント701 - _experience.analytics.イベント701to800.イベント800, _experience.analytics.イベント801to900.イベント801 — エクスペリエンス.analytics.イベント801to900.イベント900, _experience.analytics.イベント901to1000.イベント901 - _experience.analytics.901to1000.1000 | オブジェクト | ヒット時にトリガーされるカスタムイベント。 | {id （オブジェクト）, value （オブジェクト）} |
| m_geo_country | placeContext.geo.countryCode | string | ヒット元の国（IPに基づく）の省略形です。 |
| m_geo_latitude | placeContext.geo._スキーマ.latitude | 数値 | <!-- MISSING --> |
| m_geo_longitude | placeContext.geo._スキーマ.経度 | 数値 | <!-- MISSING --> |
| m_java_enabled | environment.browserDetails.javaEnabled | ブール型 | Javaが有効かどうかを示すフラグ。 |
| m_latitude | placeContext.geo._スキーマ.latitude | 数値 | <!-- MISSING --> |
| m_longitude | placeContext.geo._スキーマ.経度 | 数値 | <!-- MISSING --> |
| m_page_イベント_var1 | web.webInteraction.URL | string | リンクトラッキングイメージリクエストでのみ使用される変数。 この変数には、クリックされたダウンロードリンク、離脱リンクまたはカスタムリンクのURLが含まれます。 |
| m_page_イベント_var2 | web.webInteraction.name | string | リンクトラッキングイメージリクエストでのみ使用される変数。 このリストは、リンクのカスタム名を指定した場合にその名前を表します。 |
| m_page_type | web.webPageDetails.isErrorPage | ブール型 | エラーページディメンションの入力に使用される変数です。 この変数は、空にするか、「ErrorPage」を含める必要があります。 |
| m_pagename_no_url | web.webPageDetails.pageViews.value | 数値 | ページの名前（設定されている場合）。 ページが指定されていない場合、この値は空のままです。 |
| m_paid_search | search.isPaid | ブール型 | ヒットが有料検索検知と一致した場合に設定されるフラグ。 |
| m_product_リスト | productListItems[].items | array | products変数を通して渡される製品リスト。 | {SKU （文字列）、quantity （整数）、priceTotal （数値）} |
| m_ref_type | web.webReferrer.type | string | ヒットのリファラルのタイプを表す数値 ID。1はサイト内のWebサイトを意味し、2は他のWebサイトを意味し、3は検索エンジン、3は検索エンジン、4はハードドライブ、5は手動入力/ブックマーク(転送者なし)、7は電子メール、8はJavaScriptなし、9はソーシャルネットワークを意味します。 |
| m_search_engine | search.searchEngine | string | 訪問者をサイトに導いた検索エンジンを表す数値ID。 |
| post_currency | commerce.order.currencyCode | string | 取引で使用された通貨のコード。 |
| post_cust_hit_time_gmt | timestamp | string | これは、タイムスタンプが有効なデータセットでのみ使用されます。 これは、Unix時間に基づいて、それと共に送信されるタイムスタンプです。 |
| post_cust_visid | identityMap | object | 顧客訪問者ID。 |
| post_cust_visid | endUserIDs。_experience.asuctomid.primary | ブール型 | 顧客訪問者ID。 |
| post_cust_visid | endUserIDs。_experience.acustomid.名前空間.コード | string | 顧客訪問者ID。 |
| post_visid_high + visid_low | identityMap | object | 訪問の一意の識別子。 |
| post_visid_high + visid_low | endUserIDs。_experience.aid.id | string | 訪問の一意の識別子。 |
| post_visid_high | endUserIDs。_experience.aid.primary | ブール型 | visid_lowと組み合わせて使用し、訪問を一意に識別します。 |
| post_visid_high | endUserIDs。_experience.aid.名前空間.コード | string | visid_lowと組み合わせて使用し、訪問を一意に識別します。 |
| post_visid_low | identityMap | object | visid_highと組み合わせて使用し、訪問を一意に識別します。 |
| hit_time_gmt | receivedTimestamp | string | Unix時間に基づくヒットのタイムスタンプ。 |
| hitid_high + hitid_low | _id | string | ヒットを識別する一意の識別子。 |
| hitid_low | _id | string | hitid_highと組み合わせて使用し、ヒットを一意に識別します。 |
| ip | environment.ipV4 | string | イメージ要求のHTTPヘッダーに基づくIPアドレス。 |
| j_jscript | environment.browserDetails.javaScriptEnabled | ブール型 | 使用するJavaScriptのバージョン。 |
| mcvisid_high + mcvisid_low | identityMap | object | Experience Cloud訪問者ID。 |
| mcvisid_high + mcvisid_low | endUserIDs。_experience.mcid.id | string | Experience Cloud訪問者ID。 |
| mcvisid_high | endUserIDs。_experience.mcid.primary | ブール型 | Experience Cloud訪問者ID。 |
| mcvisid_high | endUserIDs。_experience.mcid.名前空間.コード | string | Experience Cloud訪問者ID。 |
| mcvisid_low | identityMap | object | Experience Cloud訪問者ID。 |
| sdid_high + sdid_low | _experience.ターゲット.supplementalDataID | string | ヒットのステッチID。 解析フィールドsdid_highとsdid_lowは、2つ以上の受信ヒットを結合するために使用される補助的なデータIDです。 |
| mobilebeaconproximity | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.proximity | string | Mobile Services ビーコンの近接性. |
| videochapter | media.mediaTimed.mediaChapter.chapterAssetReference._xmpDM.duration | integer | ビデオチャプターの名前。 |
| videolength | media.mediaTimed.primaryAssetReference._xmpDM.duration | integer | ビデオの長さ。 |

## 詳細マッピングフィールド

選択フィールド（「postvalues」と呼ばれる）は、AdobeAnalyticsフィールドからエクスペリエンスデータモデル(XDM)に正常にマッピングできるように、より高度な変換が必要です。 これらの高度な変換を実行するには、Adobe Experience Platformクエリサービスと、セッション、アトリビューションおよび重複排除 - 重複のための事前ビルド関数（Adobe定義関数）を使用する必要があります。

クエリサービスを使用した変換の実行について詳しくは、 [Adobe定義の関数に関するドキュメントを参照してください](../../../../query-service/sql/adobe-defined-functions.md) 。

次の表に、Analyticsフィールド名(*Analyticsフィールド*)、対応するXDMフィールド名(*XDMフィールド*)、そのタイプ(*XDMタイプ*)を示す列と、フィールド&#x200B;*説明(*&#x200B;説明)の説明を示す列を示します。

>[!NOTE]
>
>左右にスクロールして、テーブルの全内容を表示してください。

| Analytics場 | XDM フィールド | XDMタイプ | 説明 |
| --------------- | --------- | -------- | ---------- |
| post_evar1 ～ post_evar250 | _experience.analytics.customDimensions.eVars.eVar1 - _experience.analytics.customDimensions.eVars.eVar250 | string | カスタム変数。1 ～ 250の値を指定できます。 各組織でこれらのカスタムeVarの使用方法が異なります。 |
| post_prop1 ～ post_prop75 | _experience.analytics.customDimensions.props.prop1 - _experience.analytics.customDimensions.props.prop75 | string | カスタムトラフィック変数。1 ～ 75の値を指定できます。 |
| post_browser_height | environment.browserDetails.viewportHeight | integer | ブラウザーの高さ（ピクセル単位）。 |
| post_browser_width | environment.browserDetails.viewportWidth | integer | ブラウザーの幅（ピクセル単位）。 |
| post_キャンペーン | marketing.trackingCode | string | トラッキングコードディメンションで使用される変数。 |
| post_チャネル | web.webPageDetails.siteSection | string | サイトセクションディメンションで使用される変数。 |
| post_cust_visid | endUserIDs。_experience.asuctomid.id | string | カスタム訪問者ID（設定されている場合）。 |
| post_first_hit_page_url | _experience.analytics.endUser.firstWeb.webPageDetails.URL | string | 訪問者が最初に到達するページのURL。 |
| post_first_hit_pagename | _experience.analytics.endUser.firstWeb.webPageDetails.name | string | 入口ページの元のディメンションで使用される変数です。 訪問者の入口ページのページ名。 |
| post_keywords | search.keywords | string | ヒット用に収集されたキーワード。 |
| post_page_url | web.webPageDetails.URL | string | ページヒットのURL。 |
| post_pagename_no_url | web.webPageDetails.name | string | Pagesディメンションの入力に使用される変数です。 |
| post_purchaseid | commerce.order.purchaseID | string | 購入を一意に識別するために使用される変数。 |
| post_転送者 | web.webReferrer.URL | string | 前のページのURL。 |
| post_state | _experience.analytics.customDimensions.stateProvince | string | 状態変数。 |
| post_user_server | web.webPageDetails.server | string | Serverディメンションで使用される変数。 |
| post_zip | _experience.analytics.customDimensions.postalCode | string | 郵便番号ディメンションの入力に使用される変数。 |
| browser | _experience.analytics.環境.browserID | integer | ブラウザーの数値ID。 |
| domain | environment.domain | string | Domainディメンションで使用される変数。 これは、ユーザーのインターネットサービスプロバイダー(ISP)に基づきます。 |
| first_hit_転送者 | _experience.analytics.endUser.firstWeb.webReferrer.URL | string | 訪問者の最初の参照URL。 |
| geo_city | placeContext.geo.city | string | ヒットの市区町村の名前。 これは、ヒットのIPアドレスに基づいています。 |
| geo_dma | placeContext.geo.dmaID | integer | ヒットの人口統計領域の数値ID。 これは、ヒットのIPアドレスに基づいています。 |
| geo_region | placeContext.geo.stateProvince | string | ヒットの状態または地域の名前。 これは、ヒットのIPアドレスに基づいています。 |
| geo_zip | placeContext.geo.postalCode | string | ヒットの郵便番号。 これは、ヒットのIPアドレスに基づいています。 |
| os | _experience.analytics.環境.operatingSystemID | integer | 訪問者のオペレーティングシステムを表す数値ID。 これは、user_agent列に基づきます。 |
| search_page_num | search.pageDepth | integer | この変数は、All Search Page Rankディメンションで使用され、サイトで検索結果のページを示します | 」が表示される問題を修正しました。 |
| visit_keywords | _experience.analytics.session.search.keywords | string | Search Keywordsディメンションで使用される変数。 |
| visit_num | _experience.analytics.session.num | integer | 訪問回数ディメンションで使用される変数。 この開始は1になり、新しい訪問開始が訪問されるたびに（ユーザごとに）増加します。 |
| visit_page_num | _experience.analytics.session.depth | integer | ヒットの深さディメンションで使用される変数。 この値は、ユーザーが生成するヒットごとに1ずつ増加し、訪問のたびにリセットされます。 |
| visit_転送者 | _experience.analytics.session.web.webReferrer.URL | string | 訪問の最初の転送者。 |
| visit_search_page_num | _experience.analytics.session.search.pageDepth | integer | 訪問の最初のページ名。 |
| post_prop1 ～ post_prop75 | _experience.analytics.customDimensions.listprops.prop1 - _experience.analytics.customDimensions.listprops.prop75 | オブジェクト | カスタムトラフィック変数1 ～ 75。 |
| post_hier1 ～ post_hier5 | _experience.analytics.customDimensions.hier1 - _experience.analytics.customDimensions.hierarchies.hier5 | オブジェクト | 階層変数で使用され、値の区切りリストが含まれます。 | {値（配列），区切り文字（文字列）} |
| post_mvvar1 ～ post_mvvar3 | _experience.analytics.customDimensions.リスト.リスト1.リスト[] - _experience.analytics.customDimensions.リスト.リスト3.リスト[] | array | 変数値のリスト。 実装に応じて、カスタム値の区切りリストが含まれます。 | {値（文字列），キー（文字列）} |
| post_cookies | environment.browserDetails.cookiesEnabled | ブール型 | 「cookie サポート」ディメンションで使用される変数。 |
| post_イベント_リスト | commerce.purchases、commerce.productViews、commerce.productListOpens、commerce.checkouts、commerce.productListAdds、commerce.productListRemovals、commerce.productListViews | オブジェクト | ヒット時に標準コマースイベントがトリガーされた。 | {id （文字列），値（数値）} |
| post_イベント_リスト | _experience.analytics.イベント1to100.イベント1 - _experience.analytics.イベント1to100.イベント100, _experience.analytics.イベント101to200.イベント101 - _experience.analytics.イベント101to200.イベントイベント200, _experience.analytics.イベント201to300.イベント201 - _experience.analytics.イベント201to300.イベント300, _experience.analytics.イベント301to400.イベント301 — エクスペリエンス.analytics.イベント301to400.イベント400, _experience.analytics.イベント401to500.イベント401 - _experience.analytics.イベント401to500.イベント500, _experience.analytics.イベント5001to600.イベント501 - _experience.analytics.イベント501to600.イベント600, _experience.analytics.イベント601to700.イベント601 - _experience.analytics.イベント601to700.イベントイベント700, _experience.analytics.イベント701to800.イベント701 - _experience.analytics.イベント701to800.イベント800, _experience.analytics.イベント801to900.イベント801 — エクスペリエンス.analytics.イベント801to900.イベント900, _experience.analytics.イベント901to1000.イベント901 - _experience.analytics.901to1000.1000 | オブジェクト | ヒット時にトリガーされるカスタムイベント。 | {id （オブジェクト）, value （オブジェクト）} |
| post_java_enabled | environment.browserDetails.javaEnabled | ブール型 | Javaが有効かどうかを示すフラグ。 |
| post_latitude | placeContext.geo._スキーマ.latitude | 数値 | <!-- MISSING --> |
| post_longitude | placeContext.geo._スキーマ.経度 | 数値 | <!-- MISSING --> |
| post_page_イベント | web.webInteraction.type | string | イメージリクエストで送信されるヒットのタイプ（標準ヒット、ダウンロードリンク、離脱リンクまたはクリックされたカスタムリンク）。 |
| post_page_イベント | web.webInteraction.linkClicks.value | 数値 | イメージリクエストで送信されるヒットのタイプ（標準ヒット、ダウンロードリンク、離脱リンクまたはクリックされたカスタムリンク）。 |
| post_page_イベント_var1 | web.webInteraction.URL | string | この変数は、リンクトラッキングイメージリクエストでのみ使用されます。 これは、クリックされたダウンロードリンク、離脱リンクまたはカスタムリンクのURLです。 |
| post_page_イベント_var2 | web.webInteraction.name | string | この変数は、リンクトラッキングイメージリクエストでのみ使用されます。 これはリンクのカスタム名になります。 |
| post_page_type | web.webPageDetails.isErrorPage | ブール型 | これは、エラーページディメンションの入力に使用されます。 この変数は、空にするか、「ErrorPage」を含める必要があります。 |
| post_pagename_no_url | web.webPageDetails.pageViews.value | 数値 | ページの名前（設定されている場合）。 ページが指定されていない場合、この値は空のままです。 |
| post_product_リスト | productListItems[].items | array | products変数を通して渡される製品リスト。 | {SKU （文字列）、quantity （整数）、priceTotal （数値）} |
| post_search_engine | search.searchEngine | string | 訪問者をサイトに導いた検索エンジンを表す数値ID。 |
| mvvar1_instances | .リスト.items[] | オブジェクト | 変数値のリスト。 実装に応じて、カスタム値の区切りリストが含まれます。 |
| mvvar2_instances | .リスト.items[] | オブジェクト | 変数値のリスト。 実装に応じて、カスタム値の区切りリストが含まれます。 |
|  | mvvar3_instances | .リスト.items[] | オブジェクト | 変数値のリスト。 実装に応じて、カスタム値の区切りリストが含まれます。 |
| color | device.colorDepth | integer | c_color列の値に基づく色深度ID。 |
| first_hit_ref_type | _experience.analytics.endUser.firstWeb.webReferrer.type | string | 訪問者の最初の転送者の転送者タイプを表す数値ID。 |
| first_hit_time_gmt | _experience.analytics.endUser.firstTimestamp | integer | 訪問者の本当に最初のヒットのタイムスタンプ（UNIX 時間）。 |
| geo_country | placeContext.geo.countryCode | string | IPに基づく、ヒット元の国の省略。 |
| geo_latitude | placeContext.geo._スキーマ.latitude | 数値 | <!-- MISSING --> |
| geo_longitude | placeContext.geo._スキーマ.経度 | 数値 | <!-- MISSING --> |
| paid_search | search.isPaid | ブール型 | ヒットが有料検索検知と一致した場合に設定されるフラグ。 |
| ref_type | web.webReferrer.type | string | ヒットのリファラルのタイプを表す数値 ID。 |
| visit_paid_search | _experience.analytics.session.search.isPaid | ブール型 | 訪問の最初のヒットが有料検索ヒットからのものかどうかを示すフラグ（1=有料、0=無料）。 |
| visit_ref_type | _experience.analytics.session.web.webReferrer.type | string | 訪問の最初の転送者の転送者タイプを表す数値ID。 |
| visit_search_engine | _experience.analytics.session.search.searchEngine | string | 訪問の最初の検索エンジンの数値ID。 |
| visit_開始_時間_gmt | _experience.analytics.session.timestamp | integer | UNIX時間での訪問の最初のヒットのタイムスタンプ。 |
