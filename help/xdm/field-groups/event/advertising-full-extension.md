---
title: Adobe Advertising ExperienceEvent Full Extension Schema フィールドグループ
description: Adobe Advertising ExperienceEvent Full Extension スキーマフィールドグループについて説明します。
badgeBeta: label="ベータ版" type="Informative"
exl-id: 4a9f6bff-6098-424a-b8f4-0f14ec52d906
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '1558'
ht-degree: 11%

---

# [!UICONTROL Adobe Advertising ExperienceEvent Full Extension] スキーマフィールドグループ

>[!AVAILABILITY]
>
>[!UICONTROL Adobe Advertising ExperienceEvent Full Extension] フィールドグループは現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL Adobe Advertising ExperienceEvent Full Extension]は、[[!DNL XDM ExperienceEvent] class](../../classes/experienceevent.md)の標準スキーマフィールドグループで、Adobe Advertising（旧称「[!DNL Advertising Cloud]」）によって収集される一般的な指標をキャプチャします。

このドキュメントでは、[!DNL Advertising]拡張機能フィールドグループの構造と使用例について説明します。

>[!NOTE]
>
>このフィールドグループ [をExperience Platform UI](../../ui/explore.md)で検索したり、[&#x200B; パブリック XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/analytics/experienceevent-all.schema.json)でスキーマ全体を表示したりすることもできます。

## フィールドグループ構造

フィールドグループは、スキーマに単一の `_experience` オブジェクトを提供し、スキーマ自身には単一の `adcloud` オブジェクトが含まれます。

![&#x200B; フィールドグループ [!DNL Advertising]の最上位フィールド &#x200B;](../../images/field-groups/advertising-full-extension/full-schema.png " フィールドグループ  [!DNL Advertising] の最上位フィールド ")

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `adDeliveryDetails` | オブジェクト | 広告配信の詳細： このオブジェクトの内容について詳しくは、[&#x200B; オブジェクト `adDeliveryDetails`の以下の](#adDeliveryDetails) サブセクションを参照してください。 |
| `advertisement` | オブジェクト | デジタル広告の詳細。 このオブジェクトの内容について詳しくは、広告オブジェクト [の以下の](#advertisement) サブセクションを参照してください。 |
| `campaign` | オブジェクト | キャンペーン階層の詳細： このオブジェクトの内容について詳しくは、キャンペーンオブジェクト [の以下の](#campaign-campaign) サブセクションを参照してください。 |
| `conversionDetails` | オブジェクト | 広告のコンバージョンの詳細。 このオブジェクトの内容については詳しくは、[次のサブセクション](#conversionDetails)を参照してください。 |
| `eventType` | 文字列 | Adobe Advertising イベントタイプ。 |
| `fees` | オブジェクト | Advertising料金の詳細。 このオブジェクトの内容について詳しくは、料金オブジェクト [の以下の](#fees) サブセクションを参照してください。 |
| `inventory` | オブジェクト | 在庫の詳細： このオブジェクトの内容については詳しくは、[次のサブセクション](#inventory)を参照してください。 |
| `productDetails` | オブジェクト | 商品の広告の詳細。 このオブジェクトの内容について詳しくは、productDetails オブジェクト [の以下の](#productDetails) サブセクションを参照してください。 |
| `stitchId` | 文字列 | サードパーティ Cookieをブロックするブラウザーでクリックスルーのコンバージョンを追跡するための、Adobe Advertising広告サーバーのID。 |

## `adDeliveryDetails` {#adDeliveryDetails}

adDeliveryDetails オブジェクトは、広告が配信された場所と方法に関する情報（プレースメント web サイトや位置識別子など）を提供します。

![&#x200B; オブジェクトとそのフィールドを示す`adDeliveryDetails` スキーマ図。](../../images/field-groups/advertising-full-extension/adDeliveryDetails.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `placementWebsite` | 文字列 | 広告が表示されたweb サイト。 |
| `siteLinkText` | 文字列 | 広告で選択された実際のサイトリンク。 |
| `interestLocationID` | 文字列 | 検索クエリから推測された場所。 例えば、「ゴアのホテル」のクエリは、ユーザーの実際の閲覧場所に関係なく、ゴアのIDを返します。 |
| `physicalLocationID` | 文字列 | ユーザーのブラウジング場所。広告ネットワークからの参照IDで表されます。 このIDは、読み取り可能な場所の名前（都市や国など）ではなく、広告ネットワークによって地理的なターゲットを識別するために割り当てられたコードです。 |

## `advertisement` {#advertisement}

Advertising オブジェクトは、デジタル広告に関する詳細（識別子、タイプ、クリエイティブ、ターゲティング、関連キーワードなど）を記述します。

![&#x200B; オブジェクトとそのフィールドを示す`advertisement` スキーマ図。](../../images/field-groups/advertising-full-extension/advertisement.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `adId` | 文字列 | このイベントに関連付けられた広告の識別子。 このIDは、Ad-ID業界標準とは関係ありません。 |
| `runtime` | 文字列 | 広告ユニットのランタイム。ブラウザーまたはオペレーティングシステムのランタイムとは異なります。 指定できる値は`unknown`と`HTML5`です。 |
| `adClass` | 文字列 | ドライビングイベントの広告クラス : `display`、`video`、または`social`。 |
| `adUnitType` | 文字列 | ブラウザーまたはデバイスで広告をレンダリングするコードの識別子。 指定できる値には以下のものがあります。<ul><li>`linearVideo`: リニア ビデオ</li><li>`interactiveVideo`: インタラクティブビデオ</li><li>`banner`: バナー，</li><li>`richMediaBanner`: リッチメディアバナー，</li><li>`newsFeedVideo`: ニュースフィードのビデオ，</li><li>`newsFeedDisplay`: ニュースフィード表示，</li><li>`HTML5`: HTML5,</li><li>`inPageVideo`: ページのビデオで，</li><li>`inPageDisplay`: ページの表示中</li><li>`facebook`: Facebook,</li><li>`twitter`: Twitter,</li><li>`linearTv`: リニア TV,</li><li>`vod`: ビデオ オンデマンド</li></ul> |
| `promotedAssetId` | 文字列 | このイベントに関連付けられた広告でプロモーションされるアセットの識別子。 |
| `creativeID` | 文字列 | このイベントに関連付けられた広告クリエイティブ（バナー、ビデオ、ソーシャル広告など）の識別子。 |
| `keywordID` | 文字列 | このイベントをトリガーした検索クエリでユーザーが入力したキーワードの識別子。 |
| `keyword` | 文字列 | 顧客が入札したリストキーワード。 |
| `isDynamicSearchAd` | ブール | イベントが動的検索広告から発生しているかどうかを示します。 |
| `audienceID` | 文字列 | 広告のターゲットとなるオーディエンスセグメントの識別子。 |
| `adGroupID` | 文字列 | このイベントをトリガーした広告に関連付けられた広告グループの識別子。 |
| `campaignID` | 文字列 | このイベントをトリガーした広告に関連付けられたキャンペーンの識別子。 |
| `networkType` | 文字列 | イベントが発生したネットワークタイプ。 指定できる値には以下のものがあります。 <ul><li>`search`：広告が検索ネットワークに表示されました。</li><li>`content`：広告がコンテンツ ネットワークに表示されました。</li></ul> |
| `matchType` | 文字列 | キーワード一致タイプ。 指定できる値には以下のものがあります。 <ul><li>`exact`: キーワードの完全一致</li><li>`broad`: キーワードの幅広い一致</li><li>`phrase`: キーワードの語句一致</li></ul> |

## `campaign` {#campaign}

キャンペーンオブジェクトは、通貨の詳細とともに、アカウント、広告主、プレースメント、パッケージ識別子などの広告キャンペーン階層を定義します。

![&#x200B; オブジェクトとそのフィールドを示す`campaign` スキーマ図。](../../images/field-groups/advertising-full-extension/campaign.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountId` | 文字列 | アカウントの識別子。 |
| `dspId` | 文字列 | キャンペーンが定義されるDemand Side Platform（DSP）のID。 通常、このIDはAdobe Advertising DSPのIDです。 |
| `campaignId` | 文字列 | キャンペーンの識別子。 |
| `placementId` | 文字列 | プレースメントの識別子。 |
| `packageId` | 文字列 | Advertising DSP パッケージの識別子。 |
| `advertiserId` | 文字列 | 広告主の識別子。 |
| `experimentId` | 文字列 | 実験の識別子。 |
| `sampleGroupId` | 文字列 | サンプルグループの識別子。 |
| `currency` | 文字列 | アカウントのISO 4217請求通貨コード。 正規表現パターン ^[A-Z]{3}$（大文字3文字）を使用します。 例：USD、EUR。 |

## `conversionDetails` {#conversionDetails}

conversionDetails オブジェクトは、トラッキングコード、ID、コンバージョンプロパティなど、広告コンバージョンのトラッキング情報を取得します。

![&#x200B; オブジェクトとそのフィールドを示す`conversionDetails` スキーマ図。](../../images/field-groups/advertising-full-extension/conversionDetails.png "conversionDetails フィールド ")

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `trackingCode` | 文字列 | イベントのコンバージョントラッキングコード。 使用可能な形式の一覧については、[AMO ID形式](https://experienceleague.adobe.com/en/docs/advertising/integrations/customer-journey-analytics/ids#amo-id-formats)を参照してください。 |
| `trackingIdentities` | 文字列 | イベントのEF ID、またはトラッキング IDの詳細。 使用可能な形式の一覧については、[EF ID形式](https://experienceleague.adobe.com/en/docs/advertising/integrations/customer-journey-analytics/ids#ef-id-formats)を参照してください。 |
| `conversionProperties` | オブジェクト | 変換プロパティのマップ。キーと値のペア文字列（`subscriptions=253`など）の配列として表されます。 |

## `fees` {#fees}

手数料オブジェクトは、Advertising DSP、アカウント、広告主ごとに分類された、メディア、データ、その他の広告コストをキャプチャします。

![&#x200B; オブジェクトとそのフィールドを示す`fees` スキーマ図。](../../images/field-groups/advertising-full-extension/fees.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `DSPMediaFees` | 数値 | Advertising DSPが請求する広告料。 |
| `DSPDataFees` | 数値 | Advertising DSPが請求するデータ料金。 |
| `DSPOtherFees` | 数値 | Advertising DSPが請求するその他の料金。 |
| `accountMediaFees` | 数値 | Advertising DSPはアカウントの広告料を請求できますが、請求することはできません。 |
| `accountDataFees` | 数値 | Advertising DSPでは請求できないが、アカウントのデータ料金はかかります。 |
| `accountOtherFees` | 数値 | その他の手数料はアカウントに対して発生しますが、Advertising DSPからは請求できません。 |
| `advertiserMediaFees` | 数値 | 広告主にアカウントから請求されるメディア広告主料金。 |
| `advertiserDataFees` | 数値 | アカウントから広告主に請求されるその他の広告主料金。 |
| `advertiserOtherFees` | 数値 | アカウントから広告主に請求されるその他の広告主料金。 |
| `billableMediaNetSpend` | 数値 | メディア広告の請求可能な純支出。 |
| `totalMediaNetSpend` | 数値 | メディア広告の総支出額。 |
| `billableDataNetSpend` | 数値 | データ広告の請求可能な正味費用。 |
| `billableOtherNetSpend` | 数値 | 他の種類の広告に対する請求可能な正味支出。 |
| `totalBillableNetSpend` | 数値 | 請求可能な正味支出の合計。 |
| `totalNonBillableNetSpend` | 数値 | 請求不可の正味支出の合計。 |
| `totalNetSpend` | 数値 | 純支出合計。 |

## `inventory` {#inventory}

在庫オブジェクトには、セッションデータ、パートナーコード、サイト ID、コスト通貨、セグメンテーションルールなど、広告在庫の商談に関する詳細が記録されます。

![&#x200B; オブジェクトとそのフィールドを示す`inventory` スキーマ図。](../../images/field-groups/advertising-full-extension/inventory.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sessionId` | 文字列 | エクスペリエンスイベントに関連付けられたセッション ID。同じセッションで発生した独立したイベントをリンクするために使用されます。 |
| `feedID` | 文字列 | パブリッシャー、広告交換、およびその他の機能の複合ID。 |
| `sspPartnerCode` | 文字列 | Adobe Advertisingがインベントリの商談を受け取るパートナー（交換）。 |
| `siteID` | 文字列 | 広告インプレッションが配信されたweb サイトの識別子。 |
| `costCurrency` | 文字列 | 広告機会に対するパートナーへの支払いに使用されるISO 4217通貨コード。 値は、正規表現パターン ^[A-Z]{3}$ （3つの大文字）に従う必要があります。 例：USD、EUR。 |
| `inventorySourceId` | 文字列 | このオポチュニティが配信されたAdobe Advertising インベントリ ソースのID。 |
| `segment` | オブジェクト | ユーザーセグメンテーションルールに関連する詳細。 そのプロパティは次のとおりです。<ul><li>`attributablePartnerId` （文字列）: attributableSegmentIdを所有するセグメントプロバイダーの識別子。</li><li>`attributableSegmentId` （文字列）: プレースメントのターゲティングルールでユーザーターゲティング用にクレジットされたセグメント。 これは、コストの追跡とパートナーの支払いの目的で使用されます。</li><li>`segments` （文字列）: ユーザーが属していたユーザーセグメント a\）と、広告がターゲットにしていたユーザーセグメント b\）の積集合。 これは、オークション時にユーザーが属していたセグメントの完全なリストではありません。</li></ul> |
| `optimizationTag` | 文字列 | 最適化に関連するタグ。 |
| `attributableDeviceGraphId` | 文字列 | コンバージョンイベントに関連付けられたデバイスグラフの識別子。 |

## `productDetails` {#productDetails}

`productDetails` オブジェクトには、製品識別子、国、言語、パーティション、タイトル、広告タイプなど、[!DNL Adobe Advertising Search, Social, & Commerce]のショッピング広告で取り上げられた製品に関する情報が含まれています。

![&#x200B; オブジェクトとそのフィールドを示す`productDetails` スキーマ図。](../../images/field-groups/advertising-full-extension/productDetails.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `productID` | 文字列 | このイベントに関連付けられた広告で取り上げられた製品の識別子。 |
| `country` | 文字列 | イベントに関連する広告に表示された製品の販売国。 |
| `language` | 文字列 | 顧客のMerchant Center データフィードに示されている製品情報の言語。 |
| `partitionID` | 文字列 | このイベントの広告に関連付けられている製品グループの識別子。 |
| `title` | 文字列 | 広告に表示されている製品タイトル。 |
| `adType` | 文字列 | [!DNL Google] ショッピング キャンペーンで使用される製品の広告タイプ。 指定できる値には以下のものがあります。<ul><li>`pla_with_pog`: イベントがショッピング広告を通じて購入された場合</li><li>`pla`: イベントがショッピング広告から発生した日時。</li><li>`pla_multichannel`: ショッピング広告のイベントに「オンライン」と「ローカル」の両方のショッピングチャネルのオプションが含まれている場合。</li><li>`pla_with_promotion`: ショッピング広告のイベントに加盟店のプロモーションが表示された場合。</li></ul> |

## 次の手順

このドキュメントでは、[!DNL Adobe Advertising]拡張機能フィールドグループの構造とユースケースについて説明しました。 フィールドグループ自体の詳細については、[公開 XDM リポジトリ](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/adcloud/experienceevent-all.schema.json)を参照してください。

このフィールドグループを使用してAdobe Experience Platform Web SDKを使用して[!DNL Advertising] データを収集する場合は、[&#x200B; データストリームの設定](../../../datastreams/overview.md)に関するガイドを参照して、サーバー側のXDMにデータをマッピングする方法を確認してください。
