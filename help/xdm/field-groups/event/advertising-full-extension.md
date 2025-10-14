---
title: Adobe Advertising Cloud ExperienceEvent 完全拡張スキーマフィールドグループ
description: Adobe Advertising Cloud ExperienceEvent 完全拡張スキーマフィールドグループについて説明します。
badgeBeta: label="ベータ版" type="Informative"
source-git-commit: adfd0220b8bc53c44abc76a711b148a7e03edb7a
workflow-type: tm+mt
source-wordcount: '1581'
ht-degree: 11%

---

# [!UICONTROL Adobe Advertising Cloud ExperienceEvent 完全拡張 &#x200B;] スキーマフィールドグループ

>[!AVAILABILITY]
>
>[!UICONTROL Adobe Advertising Cloud ExperienceEvent フル拡張機能 &#x200B;] フィールドグループは、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL Adobe Advertising Cloud ExperienceEvent 完全拡張 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループで、Adobe Advertising（以前の「[!DNL Advertising Cloud]」）が収集する共通のメトリクスをキャプチャするものです。

このドキュメントでは、[!DNL Advertising Cloud] 拡張フィールドグループの構造と使用例について説明します。

>[!NOTE]
>
>また、このフィールドグループを [Experience Platform UI で &#x200B;](../../ui/explore.md) 検索したり、[&#x200B; パブリック XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/analytics/experienceevent-all.schema.json) で完全なスキーマを表示したりすることもできます。

## フィールドグループ構造

フィールドグループは、スキーマに単一の `_experience` オブジェクトを提供し、スキーマ自身には単一の `adcloud` オブジェクトが含まれます。

![[!DNL Advertising Cloud] フィールドグループの最上位フィールド &#x200B;](../../images/field-groups/advertising-full-extension/full-schema.png " フィールドグループの最上位フィ  [!DNL Advertising Cloud]  ルド ")

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `adDeliveryDetails` | オブジェクト | 広告配信の詳細。 このオブジェクトの内容について詳しくは、[&#x200B; オブジェクトに関する次の `adDeliveryDetails` サブセクション &#x200B;](#adDeliveryDetails) を参照してください。 |
| `advertisement` | オブジェクト | デジタル広告の詳細。 このオブジェクトの内容について詳しくは、広告オブジェクトの次の [&#x200B; サブセクション &#x200B;](#advertisement) を参照してください。 |
| `campaign` | オブジェクト | キャンペーン階層の詳細。 このオブジェクトの内容について詳しくは、キャンペーンオブジェクトに関する以下の [&#x200B; サブセクション &#x200B;](#campaign-campaign) を参照してください。 |
| `conversionDetails` | オブジェクト | 広告のコンバージョンの詳細。 このオブジェクトの内容については詳しくは、[次のサブセクション](#conversionDetails)を参照してください。 |
| `eventType` | 文字列 | Adobe Advertising イベントタイプ。 |
| `fees` | オブジェクト | Advertising料金の詳細。 このオブジェクトの内容について詳しくは [&#x200B; 以下の fee オブジェクトのサブセクション &#x200B;](#fees) を参照してください。 |
| `inventory` | オブジェクト | 在庫の詳細。 このオブジェクトの内容については詳しくは、[次のサブセクション](#inventory)を参照してください。 |
| `productDetails` | オブジェクト | 製品広告の詳細。 このオブジェクトの内容について詳しくは [productDetails オブジェクトの次の &#x200B;](#productDetails) サブセクションを参照してください。 |
| `stitchId` | 文字列 | サードパーティ cookie をブロックするブラウザーでのクリックスルーコンバージョンをトラッキングするAdobe Advertising広告サーバーからの ID。 |

## `adDeliveryDetails` {#adDeliveryDetails}

adDeliveryDetails オブジェクトは、広告が配信された場所と方法に関する情報（プレースメント web サイトや場所の識別子など）を提供します。

![`adDeliveryDetails` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/adDeliveryDetails.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `placementWebsite` | 文字列 | 広告が表示された web サイト。 |
| `siteLinkText` | 文字列 | 広告で選択された実際のサイトリンク。 |
| `interestLocationID` | 文字列 | 検索クエリから推測される場所。 例えば、「Hotel in Goa」に対するクエリを実行すると、ユーザーの実際の閲覧場所に関係なく、Goa の ID が返されます。 |
| `physicalLocationID` | 文字列 | ユーザーのブラウジング場所。広告ネットワークからの参照 ID で表されます。 この ID は、読み取り可能な場所名（市区町村、国など）ではなく、広告ネットワークによって割り当てられる、地理的ターゲットを識別するコードです。 |

## `advertisement` {#advertisement}

広告オブジェクトは、識別子、タイプ、クリエイティブ、ターゲティング、関連するキーワードなど、デジタル広告に関する詳細を記述します。

![`advertisement` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/advertisement.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `adId` | 文字列 | このイベントに関連付けられている広告の識別子。 この ID は Ad-ID 業界標準とは関係がありません。 |
| `runtime` | 文字列 | 広告ユニットのランタイム。ブラウザーやオペレーティングシステムのランタイムとは異なります。 使用可能な値：`unknown` および `HTML5`。 |
| `adClass` | 文字列 | 運転イベントの広告クラス：`display`、`video` または `social`。 |
| `adUnitType` | 文字列 | ブラウザーまたはデバイスで広告をレンダリングするコードの識別子。 指定できる値には以下のものがあります。<ul><li>`linearVideo`：リニアビデオ</li><li>`interactiveVideo`：インタラクティブビデオ</li><li>`banner`：バナー、</li><li>`richMediaBanner`：リッチメディアバナー、</li><li>`newsFeedVideo`：ニュースフィードビデオ、</li><li>`newsFeedDisplay`：ニュースフィードの表示</li><li>`HTML5`: HTML5,</li><li>`inPageVideo`：ページビデオで、</li><li>`inPageDisplay`：ページディスプレイで、</li><li>`facebook`: Facebook,</li><li>`twitter`: Twitter,</li><li>`linearTv`: リニア TV</li><li>`vod`：ビデオオンデマンド</li></ul> |
| `promotedAssetId` | 文字列 | このイベントに関連付けられた広告で昇格されたアセットの識別子。 |
| `creativeID` | 文字列 | このイベントに関連付けられた広告クリエイティブの識別子（バナー、ビデオ、ソーシャル広告など）。 |
| `keywordID` | 文字列 | このイベントをトリガーした検索クエリでユーザーが入力したキーワードの識別子。 |
| `keyword` | 文字列 | 顧客が入札するリストキーワード。 |
| `isDynamicSearchAd` | ブール値 | イベントが動的検索広告から発生したかどうかを示します。 |
| `audienceID` | 文字列 | 広告のターゲットとなるオーディエンスセグメントの識別子。 |
| `adGroupID` | 文字列 | このイベントをトリガーした広告に関連付けられている広告グループの識別子。 |
| `campaignID` | 文字列 | このイベントをトリガーした広告に関連付けられたキャンペーンの識別子。 |
| `networkType` | 文字列 | イベントが発生したネットワークの種類。 指定できる値には以下のものがあります。 <ul><li>`search`：広告が検索ネットワークに表示されました。</li><li>`content`：提供情報がコンテンツ ネットワークに表示されました。</li></ul> |
| `matchType` | 文字列 | キーワード一致タイプ。 指定できる値には以下のものがあります。 <ul><li>`exact`：キーワードの完全一致</li><li>`broad`：キーワードの部分一致</li><li>`phrase`：キーワードのフレーズ一致</li></ul> |

## `campaign` {#campaign}

キャンペーンオブジェクトは、アカウント、広告主、プレースメント、パッケージ識別子、通貨の詳細など、広告キャンペーンの階層を定義します。

![`campaign` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/campaign.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountId` | 文字列 | アカウントの識別子。 |
| `dspId` | 文字列 | キャンペーンが定義されているDemand Side Platform（DSP）の識別子。 通常、この識別子は Adobe Advertising Cloud DSP の ID です。 |
| `campaignId` | 文字列 | キャンペーンの識別子。 |
| `placementId` | 文字列 | プレースメントの識別子。 |
| `packageId` | 文字列 | Advertising DSP パッケージの識別子。 |
| `advertiserId` | 文字列 | 広告主の識別子。 |
| `experimentId` | 文字列 | 実験の識別子。 |
| `sampleGroupId` | 文字列 | サンプルグループの識別子。 |
| `currency` | 文字列 | アカウントの ISO 4217 請求通貨コード。 正規表現パターン ^[A-Z]{3}$ （3 つの大文字）を使用します。 例：USD、EUR。 |

## `conversionDetails` {#conversionDetails}

conversionDetails オブジェクトは、トラッキングコード、ID、コンバージョンプロパティなど、広告コンバージョンのトラッキング情報をキャプチャします。

![`conversionDetails` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/conversionDetails.png "conversionDetails フィールド ")

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `trackingCode` | 文字列 | イベントに対するコンバージョントラッキングコード。 使用できる形式のリストについては、[AMO ID 形式 &#x200B;](https://experienceleague.adobe.com/ja/docs/advertising/integrations/customer-journey-analytics/ids#amo-id-formats) を参照してください。 |
| `trackingIdentities` | 文字列 | イベントの EF ID またはトラッキング ID の詳細。 使用可能な形式の一覧は、[EF ID 形式 &#x200B;](https://experienceleague.adobe.com/ja/docs/advertising/integrations/customer-journey-analytics/ids#ef-id-formats) を参照してください。 |
| `conversionProperties` | オブジェクト | 変換プロパティのマップで、（`subscriptions=253` などの）キーと値のペアの文字列の配列として表されます。 |

## `fees` {#fees}

料金オブジェクトには、メディア、データ、その他の広告コストがAdvertising DSP、アカウント、広告主ごとに分類されてキャプチャされます。

![`fees` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/fees.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `DSPMediaFees` | 数値 | Advertising DSPが請求できるメディア料金。 |
| `DSPDataFees` | 数値 | Advertising DSPが請求できるデータ料金。 |
| `DSPOtherFees` | 数値 | Advertising DSPが請求できるその他の料金。 |
| `accountMediaFees` | 数値 | アカウントのメディア料金ですが、Advertising DSPで請求することはできません。 |
| `accountDataFees` | 数値 | アカウントのデータ料金ですが、Advertising DSPで請求することはできません。 |
| `accountOtherFees` | 数値 | アカウントに対するその他の料金ですが、Advertising DSPで請求することはできません。 |
| `advertiserMediaFees` | 数値 | メディア広告主は、アカウントから広告主に請求される料金。 |
| `advertiserDataFees` | 数値 | アカウントから広告主に請求されるその他の広告主費用。 |
| `advertiserOtherFees` | 数値 | アカウントから広告主に請求されるその他の広告主費用。 |
| `billableMediaNetSpend` | 数値 | メディア広告の課金対象の純支出。 |
| `totalMediaNetSpend` | 数値 | メディア広告の純支出の合計。 |
| `billableDataNetSpend` | 数値 | データ広告の課金対象の正味支出。 |
| `billableOtherNetSpend` | 数値 | その他のタイプの広告に対して請求できる純支出。 |
| `totalBillableNetSpend` | 数値 | 請求可能な純支出の合計。 |
| `totalNonBillableNetSpend` | 数値 | 請求不可の純支出の合計。 |
| `totalNetSpend` | 数値 | 純支出の合計。 |

## `inventory` {#inventory}

在庫オブジェクトは、セッションデータ、パートナーコード、サイト ID、原価通貨、セグメント化ルールなど、広告在庫オポチュニティに関する詳細を記録します。

![`inventory` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/inventory.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sessionId` | 文字列 | エクスペリエンスイベントに関連付けられたセッション ID。同じセッションで発生した独立したイベントをリンクするために使用されます。 |
| `feedID` | 文字列 | パブリッシャー、広告交換、その他の機能の複合 ID。 |
| `sspPartnerCode` | 文字列 | Adobe Advertising Cloud が広告在庫を表示するパートナー（取引所）。 |
| `siteID` | 文字列 | 広告インプレッションが提供された web サイトの識別子。 |
| `costCurrency` | 文字列 | 広告機会に対するパートナーへの支払いに使用される ISO 4217 通貨コード。 値は正規表現パターン^[A-Z]{3}$ （3 つの大文字）に従う必要があります。 例：USD、EUR。 |
| `inventorySourceId` | 文字列 | このオポチュニティが配信された Adobe Advertising Cloud インベントリソースの ID。 |
| `segment` | オブジェクト | ユーザーのセグメント化ルールに関連付けられた詳細。 プロパティは次のとおりです。<ul><li>`attributablePartnerId` （String）:accumulativeSegmentId を所有するセグメントプロバイダーの識別子。</li><li>`attributableSegmentId` （String）：プレースメントのターゲティングルールでユーザーターゲティングのクレジットが付与されるセグメント。 これは、コストと支払いパートナーを追跡する目的で使用されます。</li><li>`segments` （String）：ユーザーが属していたユーザーセグメント a\）と、広告がターゲットとしていたユーザーセグメント b\）の積集合。 これは、オークション時にユーザーが属していたセグメントの完全なリストではありません。</li></ul> |
| `optimizationTag` | 文字列 | 最適化に関連するタグ。 |
| `attributableDeviceGraphId` | 文字列 | コンバージョンイベントに起因するデバイスグラフの識別子。 |

## `productDetails` {#productDetails}

`productDetails` オブジェクトには、商品の識別子、国、言語、パーティション、タイトル、広告タイプなど、[!DNL Adobe Advertising Search, Social, & Commerce] のショッピング広告で特集される商品に関する情報が含まれています。

![`productDetails` オブジェクトとそのフィールドを示すスキーマ図。](../../images/field-groups/advertising-full-extension/productDetails.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `productID` | 文字列 | このイベントに関連付けられた広告で取り上げられる製品の識別子。 |
| `country` | 文字列 | イベントに関連付けられた広告で取り上げられる製品の販売国。 |
| `language` | 文字列 | クライアントのマーチャントセンターデータフィードに示されている、商品情報の言語。 |
| `partitionID` | 文字列 | このイベントで広告に関連付けられた商品グループの識別子。 |
| `title` | 文字列 | 広告に表示される製品タイトル。 |
| `adType` | 文字列 | 買い物キャンペーンで使用される商品 [!DNL Google] 広告タイプ。 指定できる値には以下のものがあります。<ul><li>`pla_with_pog`：イベントがショッピング広告を通じて購入から発生した場合</li><li>`pla`：買い物広告からイベントが発生した場合。</li><li>`pla_multichannel`：ショッピング広告からのイベントに「オンライン」と「ローカル」の両方のショッピングチャネルのオプションが含まれていた場合。</li><li>`pla_with_promotion`：ショッピング広告のイベントでマーチャントプロモーションが表示された場合。</li></ul> |

## 次の手順

このドキュメントでは、[!DNL Advertising Cloud] 拡張機能フィールドグループの構造と使用例について説明しました。 フィールドグループ自体の詳細については、[公開 XDM リポジトリ](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/adcloud/experienceevent-all.schema.json)を参照してください。

Adobe Experience Platform Web SDKを使用した [!DNL Advertising] データの収集にこのフィールドグループを使用している場合は、[&#x200B; データストリームの設定 &#x200B;](../../../datastreams/overview.md) に関するガイドを参照し、サーバー側でデータを XDM にマッピングする方法を確認してください。
