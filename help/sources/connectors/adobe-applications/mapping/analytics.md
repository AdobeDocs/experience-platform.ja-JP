---
keywords: Experience Platform；ホーム；人気の高いトピック；Analytics マッピングフィールド；分析マッピング
solution: Experience Platform
title: Adobe Analytics Source Connector のフィールドのマッピング
topic-legacy: overview
description: Adobe Experience Platform を使用すると、Analytics Data Connector（ADC）を介して Adobe Analytics データを取り込むことができます。ADC を通じて取り込まれるデータには、Analytics フィールドからエクスペリエンスデータモデル（XDM）フィールドに直接マッピングできるものや、正しくマッピングされるために変換や特定の関数が必要になるものがあります。
exl-id: 15dc1368-5cf1-42e1-9683-d5158f8aa2db
source-git-commit: af5564a07577a0123e1a45043d5479f6ad45d73e
workflow-type: tm+mt
source-wordcount: '3405'
ht-degree: 99%

---

# Analytics フィールドのマッピング

Adobe Experience Platform を使用すると、Analytics Data Connector（ADC）を介して Adobe Analytics データを取り込むことができます。ADC を通じて取り込まれるデータには、Analytics フィールドからエクスペリエンスデータモデル（XDM）フィールドに直接マッピングできるものや、正しくマッピングされるために変換や特定の関数が必要になるものがあります。

![](../images/analytics-data-experience-platform.png)

## 直接マッピングフィールド

選択したフィールドは、Adobe Analytics からエクスペリエンスデータモデル（XDM）に直接マッピングされます。

次のテーブルには、Analytics フィールドの名前（*Analytics フィールド*）、対応する XDM フィールド（*XDM フィールド*）およびそのタイプ（*XDM タイプ*）、フィールドの説明（*説明*）を示す列が含まれています。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| Analytics フィールド | XDM フィールド | XDM タイプ | 説明 |
| --------------- | --------- | -------- | ---------- |
| M_evar1 ～ m_evar250 | _experience.analytics.customDimensions.eVars.eVar1 ～ _experience.analytics.customDimensions.eVars.eVar250 | 文字列 | カスタム変数。1 ～ 250 の範囲で指定できます。各組織では、これらのカスタム eVar の使用方法が異なります。 |
| m_prop1 ～ m_prop75 | _experience.analytics.customDimensions.props.prop1 ～ experience.analytics.customDimensions.props.prop75 | 文字列 | カスタムトラフィック変数。1 ～ 75 の範囲で指定できます。 |
| m_browser | _experience.analytics.environment.browserID | 整数 | ブラウザーの番号 ID。 |
| m_browser_height | environment.browserDetails.viewportHeight | 整数 | ブラウザーの高さ（ピクセル単位）。 |
| m_browser_width | environment.browserDetails.viewportWidth | 整数 | ブラウザーの幅（ピクセル単位）。 |
| m_campaign | marketing.trackingCode | 文字列 | トラッキングコードディメンションで使用される変数。 |
| m_channel | web.webPageDetails.siteSection | 文字列 | 「サイトセクション」ディメンションで使用される変数。 |
| m_domain | environment.domain | 文字列 | 「ドメイン」ディメンションで使用される変数。ユーザーのインターネットサービスプロバイダー（ISP）に基づきます。 |
| m_geo_city | placeContext.geo.city | 文字列 | ヒットの市区町村の名前。ヒットの IP アドレスに基づきます。 |
| m_geo_dma | placeContext.geo.dmaID | 整数 | ヒットの人口統計領域の数値 ID。ヒットの IP アドレスに基づきます。 |
| m_geo_region | placeContext.geo.stateProvince | 文字列 | ヒットの都道府県または地域の名前。ヒットの IP アドレスに基づきます。 |
| m_geo_zip | placeContext.geo.postalCode | 文字列 | ヒットの郵便番号。ヒットの IP アドレスに基づきます。 |
| m_keywords | search.keywords | 文字列 | 「キーワード」ディメンションで使用される変数。 |
| m_os | _experience.analytics.environment.operatingSystemID | 整数 | 訪問者のオペレーティングシステムを表す数値 ID。user_agent 列に基づきます。 |
| m_page_url | web.webPageDetails.URL | 文字列 | ページヒットの URL。 |
| m_pagename_no_url | web.webPageDetails.</span>name | 文字列 | 「ページ」ディメンションの入力に使用される変数。 |
| m_referrer | web.webReferrer.URL | 文字列 | 前のページのページ URL。 |
| m_search_page_num | search.pageDepth | 整数 | 「すべての検索ページのランク」ディメンションで使用されます。ユーザーがクリックスルーしてサイトに到達する前にサイトが表示された検索結果ページを示します。 |
| m_state | _experience.analytics.customDimensions.stateProvince | 文字列 | 状態変数。 |
| m_user_server | web.webPageDetails.server | 文字列 | 「サーバー」ディメンションで使用される変数。 |
| m_zip | _experience.analytics.customDimensions.postalCode | 文字列 | 「郵便番号」ディメンションの生成に使用される変数。 |
| accept_language | environment.browserDetails.acceptLanguage | 文字列 | Accept-Language HTTP ヘッダーで指定されている受け入れ可能なすべての言語のリスト。 |
| homepage | web.webPageDetails.isHomePage | ブール値 | 廃止。現在の URL がブラウザーのホームページかどうかを示します。 |
| ipv6 | environment.ipV6 | 文字列 |
| j_jscript | environment.browserDetails.javaScriptVersion | 文字列 | ブラウザーでサポートされている JavaScript のバージョン。 |
| user_agent | environment.browserDetails.userAgent | 文字列 | HTTP ヘッダーで送信されるユーザーエージェント文字列。 |
| mobileappid | application.</span>name | 文字列 | モバイルアプリ ID。次の形式で保存されます。`[AppName][BundleVersion]` |
| mobiledevice | device.model | 文字列 | モバイルデバイスの名前。iOS の場合は、コンマ区切りの 2 桁の文字列として格納されます。最初の数字はデバイスの世代を表し、2 番目の数字はデバイスファミリーを表します。 |
| pointofinterest | placeContext.POIinteraction.POIDetail.</span>name | 文字列 | モバイルサービスで使用されます。目標地点を表します。 |
| pointofinterestdistance | placeContext.POIinteraction.POIDetail.geoInteractionDetails.distanceToCenter | 数値 | モバイルサービスで使用されます。目標地点の距離を表します。 |
| mobileplaceaccuracy | placeContext.POIinteraction.POIDetail.geoInteractionDetails.deviceGeoAccuracy | 数値 | コンテキストデータ変数 a.loc.acc から収集します。収集時の GPS の精度をメートル単位で示します。 |
| mobileplacecategory | placeContext.POIinteraction.POIDetail.category | 文字列 | コンテキストデータ変数 a.loc.category から収集します。特定の場所のカテゴリを示します。 |
| mobileplaceid | placeContext.POIinteraction.POIDetail.POIID | 文字列 | コンテキストデータ変数 a.loc.id から収集します。特定の目標地点の識別子。 |
| video | media.mediaTimed.primaryAssetReference._id | 文字列 |  ビデオの名前。 |
| videoad | advertising.adAssetReference._id | 文字列 | 広告アセットの識別子。 |
| videocontenttype | media.mediaTimed.primaryAssetViewDetails.broadcastContentType | 文字列 | ビデオの Content-type。すべてのビデオ視聴について、自動的に「ビデオ」に設定されます。 |
| videoadpod | advertising.adAssetViewDetails.adBreak._id | 文字列 | ビデオ広告が含まれるポッド。 |
| videoadinpod | advertising.adAssetViewDetails.index | 整数 | ビデオ広告がポッド内の位置にあります。 |
| videoplayername | media.mediaTimed.primaryAssetViewDetails.playerName | 文字列 | ビデオプレイヤーの名前。 |
| videochannel | media.mediaTimed.primaryAssetViewDetails.broadcastChannel | 文字列 | ビデオチャネル。 |
| videoadplayername | advertising.adAssetViewDetails.playerName | 文字列 | ビデオ広告プレイヤーの名前。 |
| videochapter | media.mediaTimed.mediaChapter.chapterAssetReference._id | 文字列 | ビデオチャプターの名前 |
| videoname | media.mediaTimed.primaryAssetReference._dc.title | 文字列 | ビデオ名。 |
| videoadname | advertising.adAssetReference._dc.title | 文字列 | ビデオ広告の名前。 |
| videoshow | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Series._iptc4xmpExt.Name | 文字列 | ビデオショー。 |
| videoseason | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Season._iptc4xmpExt.Name | 文字列 | ビデオシーズン。 |
| videoepisode | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Episode._iptc4xmpExt.Name | 文字列 | ビデオのエピソード。 |
| videonetwork | media.mediaTimed.primaryAssetViewDetails.broadcastNetwork | 文字列 | ビデオネットワーク。 |
| videoshowtype | media.mediaTimed.primaryAssetReference.showType | 文字列 | ビデオショーのタイプ。 |
| videoadload | media.mediaTimed.primaryAssetViewDetails.adLoadType | 文字列 | ビデオ広告の読み込み。 |
| videofeedtype | media.mediaTimed.primaryAssetViewDetails.sourceFeed | 文字列 | ビデオフィードのタイプ。 |
| mobilebeaconmajor | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.beaconMajor | 数値 | Mobile Services ビーコンのメジャー番号。 |
| mobilebeaconminor | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.beaconMinor | 数値 | Mobile Services ビーコンのマイナー番号。 |
| mobilebeaconuuid | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.proximityUUID | 文字列 | Mobile Services ビーコンの UUID。 |
| videosessionid | media.mediaTimed.primaryAssetViewDetails._id | 文字列 | ビデオセッション ID。 |
| videogenre | media.mediaTimed.primaryAssetReference._iptc4xmpExt.Genre | 配列 | ビデオのジャンル。 | {title (オブジェクト), description (オブジェクト), type (オブジェクト), meta:xdmType (オブジェクト), items (文字列), meta:xdmField (オブジェクト)} |
| mobileinstalls | application.firstLaunches | オブジェクト | インストール後または再インストール後の最初の実行時にトリガーされます。 | {id (文字列), value (数値)} |
| mobileupgrades | application.upgrades | オブジェクト | アプリのアップグレード番号を報告します。アップグレード後の最初の起動時またはバージョン番号の変更時に常にトリガーされます。 | {id (文字列), value (数値)} |
| mobilelaunches | application.launches | オブジェクト | アプリが起動された回数。 | {id (文字列), value (数値)} |
| mobilecrashes | application.crashes | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| mobilemessageclicks | directMarketing.clicks | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| mobileplaceentry | placeContext.POIinteraction.poiEntries | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| mobileplaceexit | placeContext.POIinteraction.poiExits | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videotime | media.mediaTimed.timePlayed | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videostart | media.mediaTimed.impressions | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videocomplete | media.mediaTimed.completes | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videosegmentviews | media.mediaTimed.mediaSegmentViews | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoadstart | advertising.impressions | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoadcomplete | advertising.completes | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoadtime | advertising.timePlayed | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videochapterstart | media.mediaTimed.mediaChapter.impressions | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videochaptercomplete | media.mediaTimed.mediaChapter.completes | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videochaptertime | media.mediaTimed.mediaChapter.timePlayed | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoplay | media.mediaTimed.starts | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videototaltime | media.mediaTimed.totalTimePlayed | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoqoetimetostart | media.mediaTimed.primaryAssetViewDetails.qoe.timeToStart | オブジェクト | ビデオ画質の開始時間。 | {id (文字列), value (数値)} |
| videoqoedropbeforestart | media.mediaTimed.dropBeforeStarts | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoqoebuffercount | media.mediaTimed.primaryAssetViewDetails.qoe.buffers | オブジェクト | ビデオ画質バッファ数 | {id (文字列), value (数値)} |
| videoqoebuffertime | media.mediaTimed.primaryAssetViewDetails.qoe.bufferTime | オブジェクト | ビデオ画質バッファ時間 | {id (文字列), value (数値)} |
| videoqoebitratechangecount | media.mediaTimed.primaryAssetViewDetails.qoe.bitrateChanges | オブジェクト | ビデオ画質変更回数 | {id (文字列), value (数値)} |
| videoqoebitrateaverage | media.mediaTimed.primaryAssetViewDetails.qoe.bitrateAverage | オブジェクト | ビデオ画質の平均ビットレート | {id (文字列), value (数値)} |
| videoqoeerrorcount | media.mediaTimed.primaryAssetViewDetails.qoe.errors | オブジェクト | ビデオ画質エラー回数 | {id (文字列), value (数値)} |
| videoqoedroppedframecount | media.mediaTimed.primaryAssetViewDetails.qoe.droppedFrames | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoprogress10 | media.mediaTimed.progress10 | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoprogress25 | media.mediaTimed.progress25 | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoprogress50 | media.mediaTimed.progress50 | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoprogress75 | media.mediaTimed.progress75 | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoprogress95 | media.mediaTimed.progress95 | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videoresume | media.mediaTimed.resumes | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videopausecount | media.mediaTimed.pauses | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videopausetime | media.mediaTimed.pauseTime | オブジェクト | <!-- MISSING --> | {id (文字列), value (数値)} |
| videosecondssincelastcall | media.mediaTimed.primaryAssetViewDetails.sessionTimeout | 整数 |

{style=&quot;table-layout:auto&quot;}

## マッピングフィールドの分割

これらのフィールドには 1 つのソースがありますが、**複数の** XDM の場所にマッピングされます。

| Analytics フィールド | XDM フィールド | XDM タイプ | 説明 |
| --------------- | --------- | -------- | ---------- |
| s_resolution | device.screenWidth、device.screenHeight | 整数 | モニターの解像度を表す数値 ID。 |
| mobileosversion | environment.operatingSystem、environment.operatingSystemVersion | 文字列 | モバイルオペレーティングシステムのバージョン。 |
| videoadlength | advertising.adAssetReference._xmpDM.duration | 整数 | ビデオ広告の長さ。 |

{style=&quot;table-layout:auto&quot;}

## 生成されたマッピングフィールド

ADC からのフィールドを変換する要選択します。XDM で生成するには、Adobe Analytics からの直接コピーを超えるロジックが必要です。

次のテーブルには、Analytics フィールドの名前（*Analytics フィールド*）、対応する XDM フィールド（*XDM フィールド*）およびそのタイプ（*XDM タイプ*）、フィールドの説明（*説明*）を示す列が含まれています。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| Analytics フィールド | XDM フィールド | XDM タイプ | 説明 |
| --------------- | --------- | -------- | ----------- |
| m_prop1 ～ m_prop75 | _experience.analytics.customDimensions.listprops.prop1 ～ _experience.analytics.customDimensions.listprops.prop75 | オブジェクト | カスタムトラフィック変数（1 ～ 75） | {} |
| m_hier1 ～ m_hier5 | _experience.analytics.customDimensions.hierarchies.hier1 ～ _experience.analytics.customDimensions.hierarchies.hier5 | オブジェクト | 階層変数で使用されます。これには、 | 区切り形式の値リストが含まれます。 | {values (配列), delimiter (文字列)} |
| m_mvvar1 ～ m_mvvar3 | _experience.analytics.customDimensions.lists.list1.list[] ～ _experience.analytics.customDimensions.lists.list3.list[] | 配列 | 変数値のリスト。実装に応じて、カスタム値の区切りリストが含まれます | {value (文字列), key (文字列)} |
| m_color | device.colorDepth | 整数 | c_color 列の値に基づく色深度 ID。 |
| m_cookies | environment.browserDetails.cookiesEnabled | ブール値 | Cookie サポートディメンションで使用される変数。 |
| m_event_list | commerce.purchases、commerce.productViews、commerce.productListOpens、commerce.checkouts、commerce.productListAdds、commerce.productListRemovals、commerce.productListViews | オブジェクト | 標準コマースイベントがヒット時にトリガーされました。 | {id (文字列), value (数値)} |
| m_event_list | _Experience.analytics.event1to100.event1 ～ _experience.analytics.event1to100.event100、_experience.analytics.event101to200.event101 ～ _experience.analytics.event101to200.event200、_experience.analytics.event201to300.event201 ～ _experience.analytics.event201to300.event300、_experience.analytics.event301to400.event301 ～ _experience.analytics.event301to400.event400、_experience.analytics.event401to500.event401 ～ _experience.analytics.event401to500.event500、_experience.analytics.event501to600.event501 ～ _experience.analytics.event501to600.event600、_experience.analytics.event601to700.event601 ～ _experience.analytics.event601to700.event700、_experience.analytics.event701to800.event701 ～ _experience.analytics.event701to800.event800、_experience.analytics.event801to900.event801 ～ _experience.analytics.event801to900.event900、_experience.analytics.event901to1000.event901 ～ _experience.analytics.event901to1000.event1000 | オブジェクト | カスタムイベントがヒット時にトリガーされました。 | {id (オブジェクト), value (オブジェクト)} |
| m_geo_country | placeContext.geo.countryCode | 文字列 | ヒット元の国（IP に基づく）の略。 |
| m_geo_latitude | placeContext.geo._schema.latitude | 数値 | <!-- MISSING --> |
| m_geo_longitude | placeContext.geo._schema.longitude | 数値 | <!-- MISSING --> |
| m_java_enabled | environment.browserDetails.javaEnabled | ブール値 | Java が有効かどうかを示すフラグ。 |
| m_latitude | placeContext.geo._schema.latitude | 数値 | <!-- MISSING --> |
| m_longitude | placeContext.geo._schema.longitude | 数値 | <!-- MISSING --> |
| m_page_event_var1 | web.webInteraction.URL | 文字列 | リンクトラッキングイメージリクエストでのみ使用される変数。この変数には、クリックされたダウンロードリンク、出口リンク、またはカスタムリンクの URL が含まれます。 |
| m_page_event_var2 | web.webInteraction.name | 文字列 | リンクトラッキングイメージリクエストでのみ使用される変数。このリストは、リンクのカスタム名をリスト表示します（指定されている場合）。 |
| m_page_type | web.webPageDetails.isErrorPage | ブール値 | 「ページが見つかりません」ディメンションの入力に使用される変数。この変数は、空にするか、「ErrorPage」を含む必要があります。 |
| m_pagename_no_url | web.webPageDetails.pageViews.value | 数値 | ページの名前（設定されている場合）。ページが指定されていない場合、この値は空のままです。 |
| m_paid_search | search.isPaid | ブール値 | ヒットが有料検索の検出に一致した場合に設定されるフラグ。 |
| m_product_list | productListItems[].items | 配列 | 製品リスト。products 変数を通じて渡されます。 | {SKU (文字列), quantity (整数), priceTotal (数値)} |
| m_ref_type | web.webReferrer.type | 文字列 | ヒットのリファラルのタイプを表す数値 ID。1 はサイト内、2 は他の Web サイト、3 は検索エンジン、4 はハードドライブ、5 は USENET、6 は手動入力／ブックマーク（リファラーなし）、7 は電子メール、8 は JavaScript なし、9 はソーシャルネットワークを意味します。 |
| m_search_engine | search.searchEngine | 文字列 | サイトに訪問者を誘導した検索エンジンを表す数値 ID。 |
| post_currency | commerce.order.currencyCode | 文字列 | トランザクションで使用された通貨のコード。 |
| post_cust_hit_time_gmt | timestamp | 文字列 | タイムスタンプが有効なデータセットでのみ使用されます。Unix 時間に基づいて送信されるタイムスタンプです。 |
| post_cust_visid | identityMap | オブジェクト | 顧客訪問者 ID。 |
| post_cust_visid | endUserIDs._experience.aacustomid.primary | ブール値 | 顧客訪問者 ID。 |
| post_cust_visid | endUserIDs_experience.aacustomid.namespace.code | 文字列 | 顧客訪問者 ID。 |
| post_visid_high + visid_low | identityMap | オブジェクト | 訪問の一意の ID。 |
| post_visid_high + visid_low | endUserIDs_experience.aaid.id | 文字列 | 訪問の一意の ID。 |
| post_visid_high | endUserIDs_experience.aaid.primary | ブール値 | visid_low と組み合わせて使用し、訪問を一意に識別します。 |
| post_visid_high | endUserIDs_experience.aaid.namespace.code | 文字列 | visid_low と組み合わせて使用し、訪問を一意に識別します。 |
| post_visid_low | identityMap | オブジェクト | visid_high と組み合わせて使用し、訪問を一意に識別します。 |
| hit_time_gmt | receivedTimestamp | 文字列 | ヒットのタイムスタンプ（UNIX 時間）。 |
| hitid_high + hitid_low | _id | 文字列 | ヒットを識別する一意の ID。 |
| hitid_low | _id | 文字列 | hitid_high と組み合わせて使用し、ヒットを一意に識別します。 |
| ip | environment.ipV4 | 文字列 | イメージリクエストの HTTP ヘッダーに基づく IP アドレス。 |
| j_jscript | environment.browserDetails.javaScriptEnabled | ブール値 | 使用する JavaScript のバージョン。 |
| mcvisid_high + mcvisid_low | identityMap | オブジェクト | Experience Cloud 訪問者 ID。 |
| mcvisid_high + mcvisid_low | endUserIDs_experience.mcid.id | 文字列 | Experience Cloud 訪問者 ID。 |
| mcvisid_high | endUserIDs_experience.mcid.primary | ブール値 | Experience Cloud 訪問者 ID。 |
| mcvisid_high | endUserIDs_experience.mcid.namespace.code | 文字列 | Experience Cloud 訪問者 ID。 |
| mcvisid_low | identityMap | オブジェクト | Experience Cloud 訪問者 ID。 |
| sdid_high + sdid_low | _experience.target.supplementalDataID | 文字列 | ヒットステッチ ID。解析フィールド sdid_high と sdid_low は、2 つ以上の受信ヒットを結合するために使用される補足的なデータ ID です。 |
| mobilebeaconproximity | placeContext.POIinteraction.POIDetail.beaconInteractionDetails.proximity | 文字列 | Mobile Services ビーコンの近接性. |
| videochapter | media.mediaTimed.mediaChapter.chapterAssetReference._xmpDM.duration | 整数 | ビデオの章の名前。 |
| videolength | media.mediaTimed.primaryAssetReference._xmpDM.duration | 整数 | ビデオの長さ。 |

{style=&quot;table-layout:auto&quot;}

## 高度なマッピングフィールド

選択フィールド（「postvalues」と呼ばれる）は、Adobe Analytics フィールドからエクスペリエンスデータモデル（XDM）に正常にマッピングされる前に、より高度な変換が必要です。これらの高度な変換を実行するには、Adobe Experience Platform クエリサービスと、セッション化、アトリビューションおよび重複排除用として事前に作成された関数（アドビ定義関数）を使用します。

クエリサービスを使用したこの変換の実行について詳しくは、[アドビ定義関数](../../../../query-service/sql/adobe-defined-functions.md)のドキュメントを参照してください。

次のテーブルには、Analytics フィールドの名前（*Analytics フィールド*）、対応する XDM フィールド（*XDM フィールド*）およびそのタイプ（*XDM タイプ*）、フィールドの説明（*説明*）を示す列が含まれています。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| Analytics フィールド | XDM フィールド | XDM タイプ | 説明 |
| --------------- | --------- | -------- | ---------- |
| post_evar1 ～ post_evar250 | _experience.analytics.customDimensions.eVars.eVar1 ～ _experience.analytics.customDimensions.eVars.eVar250 | 文字列 | カスタム変数。1 ～ 250 の範囲で指定できます。各組織では、これらのカスタム eVar の使用方法が異なります。 |
| post_prop1 ～ post_prop75 | _experience.analytics.customDimensions.props.prop1 ～ experience.analytics.customDimensions.props.prop75 | 文字列 | カスタムトラフィック変数。1 ～ 75 の範囲で指定できます。 |
| post_browser_height | environment.browserDetails.viewportHeight | 整数 | ブラウザーの高さ（ピクセル単位）。 |
| post_browser_width | environment.browserDetails.viewportWidth | 整数 | ブラウザーの幅（ピクセル単位）。 |
| post_campaign | marketing.trackingCode | 文字列 | トラッキングコードディメンションで使用される変数。 |
| post_channel | web.webPageDetails.siteSection | 文字列 | 「サイトセクション」ディメンションで使用される変数。 |
| post_cust_visid | endUserIDs_experience.aacustomid.id | 文字列 | カスタム訪問者 ID（設定されている場合）。 |
| post_first_hit_page_url | _experience.analytics.endUser.firstWeb.webPageDetails.URL | 文字列 | 訪問者が最初に到達するページの URL。 |
| post_first_hit_pagename | _experience.analytics.endUser.firstWeb.webPageDetails.name | 文字列 | 「オリジナルの入口ページ」ディメンションで使用される変数。訪問者の入口ページのページ名。 |
| post_keywords | search.keywords | 文字列 | ヒット用に収集されたキーワード。 |
| post_page_url | web.webPageDetails.URL | 文字列 | ページヒットの URL。 |
| post_pagename_no_url | web.webPageDetails.name | 文字列 | 「ページ」ディメンションの入力に使用される変数。 |
| post_purchaseid | commerce.order.purchaseID | 文字列 | 購入を一意に識別するために使用される変数。 |
| post_referrer | web.webReferrer.URL | 文字列 | 前のページの URL。 |
| post_state | _experience.analytics.customDimensions.stateProvince | 文字列 | 状態変数。 |
| post_user_server | web.webPageDetails.server | 文字列 | 「サーバー」ディメンションで使用される変数。 |
| post_zip | _experience.analytics.customDimensions.postalCode | 文字列 | 「郵便番号」ディメンションの生成に使用される変数。 |
| ブラウザー | _experience.analytics.environment.browserID | 整数 | ブラウザーの数値 ID。 |
| ドメイン | environment.domain | 文字列 | 「ドメイン」ディメンションで使用される変数。ユーザーのインターネットサービスプロバイダー（ISP）に基づきます。 |
| first_hit_referrer | _experience.analytics.endUser.firstWeb.webReferrer.URL | 文字列 | 訪問者の最初の参照 URL。 |
| geo_city | placeContext.geo.city | 文字列 | ヒットの市区町村の名前。ヒットの IP アドレスに基づきます。 |
| geo_dma | placeContext.geo.dmaID | 整数 | ヒットの人口統計領域の数値 ID。ヒットの IP アドレスに基づきます。 |
| geo_region | placeContext.geo.stateProvince | 文字列 | ヒットの都道府県または地域の名前。ヒットの IP アドレスに基づきます。 |
| geo_zip | placeContext.geo.postalCode | 文字列 | ヒットの郵便番号。ヒットの IP アドレスに基づきます。 |
| os | _experience.analytics.environment.operatingSystemID | 整数 | 訪問者のオペレーティングシステムを表す数値 ID。user_agent 列に基づきます。 |
| search_page_num | search.pageDepth | 整数 | この変数は、「すべての検索ページのランク」ディメンションで使用され、ユーザーがクリックスルーしてサイトに到達する前に、お客様のサイトが表示された | 検索結果のページを示します。 |
| visit_keywords | _experience.analytics.session.search.keywords | 文字列 | 「検索キーワード」ディメンションで使用される変数。 |
| visit_num | _experience.analytics.session.num | 整数 | 「訪問回数」ディメンションで使用される変数。1 から始まり、（訪問者ごとに）新しい訪問が開始されるたびに増分されます。 |
| visit_page_num | _experience.analytics.session.depth | 整数 | 「ヒットの深さ」ディメンションで使用される変数。この値は、ユーザーが生成したヒットごとに 1 ずつ増加し、各訪問後にリセットされます。 |
| visit_referrer | _experience.analytics.session.web.webReferrer.URL | 文字列 | 訪問の最初のリファラー。 |
| visit_search_page_num | _experience.analytics.session.search.pageDepth | 整数 | 訪問の最初のページ名。 |
| post_prop1 ～ post_prop75 | _experience.analytics.customDimensions.listprops.prop1 ～ _experience.analytics.customDimensions.listprops.prop75 | オブジェクト | カスタムトラフィック変数 1 ～ 75。 |
| Post_hier1 ～ post_hier5 | _experience.analytics.customDimensions.hierarchies.hier1 ～ _experience.analytics.customDimensions.hierarchies.hier5 | オブジェクト | 階層変数で使用され、値の区切りリストが含まれます。 | {values (配列), delimiter (文字列)} |
| Post_mvvar1 ～ post_mvvar3 | _experience.analytics.customDimensions.lists.list1.list[] ～ _experience.analytics.customDimensions.lists.list3.list[] | 配列 | 変数値のリスト。実装に応じて、カスタム値の区切りリストが含まれます。 | {value (文字列), key (文字列)} |
| post_cookies | environment.browserDetails.cookiesEnabled | ブール値 | 「cookie サポート」ディメンションで使用される変数。 |
| post_event_list | commerce.purchases、commerce.productViews、commerce.productListOpens、commerce.checkouts、commerce.productListAdds、commerce.productListRemovals、commerce.productListViews | オブジェクト | 標準コマースイベントがヒット時にトリガーされました。 | {id (文字列), value (数値)} |
| post_event_list | _Experience.analytics.event1to100.event1 ～ _experience.analytics.event1to100.event100、_experience.analytics.event101to200.event101 ～ _experience.analytics.event101to200.event200、_experience.analytics.event201to300.event201 ～ _experience.analytics.event201to300.event300、_experience.analytics.event301to400.event301 ～ _experience.analytics.event301to400.event400、_experience.analytics.event401to500.event401 ～ _experience.analytics.event401to500.event500、_experience.analytics.event501to600.event501 ～ _experience.analytics.event501to600.event600、_experience.analytics.event601to700.event601 ～ _experience.analytics.event601to700.event700、_experience.analytics.event701to800.event701 ～ _experience.analytics.event701to800.event800、_experience.analytics.event801to900.event801 ～ _experience.analytics.event801to900.event900、_experience.analytics.event901to1000.event901 ～ _experience.analytics.event901to1000.event1000 | オブジェクト | カスタムイベントがヒット時にトリガーされました。 | {id (オブジェクト), value (オブジェクト)} |
| post_java_enabled | environment.browserDetails.javaEnabled | ブール値 | Java が有効かどうかを示すフラグ。 |
| post_latitude | placeContext.geo._schema.latitude | 数値 | <!-- MISSING --> |
| post_longitude | placeContext.geo._schema.longitude | 数値 | <!-- MISSING --> |
| post_page_event | web.webInteraction.type | 文字列 | イメージリクエストで送信されるヒットのタイプ（標準的なヒット、ダウンロードリンク、出口リンク、クリックされたカスタムリンク）。 |
| post_page_event | web.webInteraction.linkClicks.value | 数値 | イメージリクエストで送信されるヒットのタイプ（標準的なヒット、ダウンロードリンク、出口リンク、クリックされたカスタムリンク）。 |
| post_page_event_var1 | web.webInteraction.URL | 文字列 | この変数は、リンクトラッキングイメージリクエストでのみ使用されます。ダウンロードリンク、出口リンクまたはクリックされたカスタムリンクの URL です。 |
| post_page_event_var2 | web.webInteraction.name | 文字列 | この変数は、リンクトラッキングイメージリクエストでのみ使用されます。リンクのカスタム名になります。 |
| post_page_type | web.webPageDetails.isErrorPage | ブール値 | 「エラーページ」ディメンションの入力に使用されます。この変数の値は、空か「ErrorPage」である必要があります。 |
| post_pagename_no_url | web.webPageDetails.pageViews.value | 数値 | ページの名前（設定されている場合）。ページが指定されていない場合、この値は空のままです。 |
| post_product_list | productListItems[].items | 配列 | 製品リスト。products 変数を通じて渡されます。 | {SKU (文字列), quantity (整数), priceTotal (数値)} |
| post_search_engine | search.searchEngine | 文字列 | サイトに訪問者を誘導した検索エンジンを表す数値 ID。 |
| mvvar1_instances | .list.items[] | オブジェクト | 変数値のリスト。実装に応じて、カスタム値の区切りリストが含まれます。 |
| mvvar2_instances | .list.items[] | オブジェクト | 変数値のリスト。実装に応じて、カスタム値の区切りリストが含まれます。 |
|  | mvvar3_instances | .list.items[] | オブジェクト | 変数値のリスト。実装に応じて、カスタム値の区切りリストが含まれます。 |
| color | device.colorDepth | 整数 | c_color 列の値に基づく色深度 ID。 |
| first_hit_ref_type | _experience.analytics.endUser.firstWeb.webReferrer.type | 文字列 | 訪問者の最初のリファラーのリファラータイプを表す数値 ID。 |
| first_hit_time_gmt | _experience.analytics.endUser.firstTimestamp | 整数 | 訪問者の最初のヒットのタイムスタンプ（Unix 時間）。 |
| geo_country | placeContext.geo.countryCode | 文字列 | ヒットの発生元となった国の略称（IP アドレスに基づく）。 |
| geo_latitude | placeContext.geo._schema.latitude | 数値 | <!-- MISSING --> |
| geo_longitude | placeContext.geo._schema.longitude | 数値 | <!-- MISSING --> |
| paid_search | search.isPaid | ブール値 | ヒットが有料検索の検出に一致した場合に設定されるフラグ。 |
| ref_type | web.webReferrer.type | 文字列 | ヒットのリファラルのタイプを表す数値 ID。 |
| visit_paid_search | _experience.analytics.session.search.isPaid | ブール値 | 訪問の最初のヒットが有料検索ヒットからのヒットであったかどうかを示すフラグ（1 = 有料、0 = 無料）。 |
| visit_ref_type | _experience.analytics.session.web.webReferrer.type | 文字列 | 訪問の最初のリファラーのリファラータイプを表す数値 ID。 |
| visit_search_engine | _experience.analytics.session.search.searchEngine | 文字列 | 訪問の最初の検索エンジンを表す数値 ID。 |
| visit_start_time_gmt | _experience.analytics.session.timestamp | 整数 | 訪問者の最初のヒットのタイムスタンプ（Unix 時間）。 |

{style=&quot;table-layout:auto&quot;}