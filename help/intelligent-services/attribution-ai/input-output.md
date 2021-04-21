---
keywords: Experience Platform；はじめに；アトリビューションai；人気のあるトピック；アトリビューションai入力；アトリビューションai出力；
solution: Experience Platform, Intelligent Services
title: Attribution AIの入出力
topic-legacy: Input and Output data for Attribution AI
description: 次のドキュメントでは、Attribution AIで使用される様々な入出力の概要を説明します。
exl-id: d6dbc9ee-0c1a-4a5f-b922-88c7a36a5380
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 14%

---

# [!DNL Attribution AI]の入出力

次のドキュメントは、[!DNL Attribution AI]で使用される様々な入力と出力の概要を示しています。

## [!DNL Attribution AI] 入力データ

Attribution AIは、次のデータセットのいずれかを分析してアルゴリズムスコアを計算することで機能します。

- コンシューマーエクスペリエンスイベント(CEE)データセット
- [Analyticsソースコネクタ](../../sources/tutorials/ui/create/adobe-applications/analytics.md)を使用したAdobe Analyticsデータセット

>[!IMPORTANT]
>
>Adobe Analyticsのソースコネクタは、データをバックフィルするのに最大4週間かかる場合があります。 コネクターを最近設定した場合は、Attribution AIに必要な最小長のデータがデータセットに含まれていることを確認する必要があります。 [履歴データ](#data-requirements)のセクションを確認し、正確なアルゴリズムスコアを計算するのに十分なデータがあることを確認してください。

[!DNL Consumer Experience Event] (CEE)スキーマの設定の詳細については、『[インテリジェントサービスデータ準備](../data-preparation.md)』ガイドを参照してください。 Adobe Analyticsデータのマッピングについて詳しくは、[Analytics field mappings](../../sources/connectors/adobe-applications/analytics.md)のドキュメントを参照してください。

Attribution AIの場合、[!DNL Consumer Experience Event] (CEE)スキーマのすべての列が必須ではありません。

>[!NOTE]
>
> 次の9列は必須です。追加の列はオプションですが、[!DNL Customer AI]や[!DNL Journey AI]などの他のAdobeソリューションで同じデータを使用する場合は、列を追加することをお勧めします。

| 必須列 | 必要な対象 |
| --- | --- |
| プライマリIDフィールド | タッチポイント/コンバージョン |
| タイムスタンプ | タッチポイント/コンバージョン |
| チャネル._type | タッチポイント |
| Channel.mediaAction | タッチポイント |
| Channel.mediaType | タッチポイント |
| Marketing.trackingCode | タッチポイント |
| Marketing.campaignname | タッチポイント |
| Marketing.campaigngroup | タッチポイント |
| コマース | コンバージョン |

通常、アトリビューションは、「コマース」下の注文、購入、チェックアウトなどのコンバージョン列で実行されます。 「チャネル」列と「マーケティング」列は、良いインサイトのためのタッチポイントを定義する場合に強くお勧めします。 ただし、上記の列と共に他の列も含めて、コンバージョンまたはタッチポイントの定義として設定できます。

以下の列は必須ではありませんが、情報があればCEEスキーマに列を含めることをお勧めします。

**その他の推奨列：**
- web.webReferer
- web.webInteraction
- web.webPageDetails
- xdm:productListItems

### 履歴データ {#data-requirements}

>[!IMPORTANT]
>
> Attribution AIが機能するために必要な最小データ量は次のとおりです。
> - 良好なモデルを実行するには、3か月（90日）以上のデータを提供する必要があります。
> - 少なくとも1000のコンバージョンが必要です。


Attribution AIには、モデルトレーニングの入力として履歴データが必要です。 必要なデータの期間は、主に次の2つの主な要因によって決まります。トレーニングウィンドウとルックバックウィンドウ トレーニング期間が短い入力は、最近の傾向に対してより敏感になり、トレーニング期間が長い入力は、より安定した正確なモデルを生成するのに役立ちます。 ビジネス目標を最も効果的に表す履歴データを使用して目標をモデル化することが重要です。

[トレーニングウィンドウ設定](./user-guide.md#training-window)フィルターコンバージョンイベントは、発生時間に基づいてモデルトレーニングに含めるように設定されています。 現在、最小トレーニング期間は1四半期（90日）です。 [ルックバックウィンドウ](./user-guide.md#lookback-window)には、このコンバージョンイベントに関連するコンバージョンイベントのタッチポイントを含める日数を示す時間枠が表示されます。 これら2つの概念を組み合わせて、1つのアプリケーションに必要な入力データの量（日数で測定）を決定します。

デフォルトでは、Attribution AIはトレーニング期間を最新の2四半期（6か月）として定義し、56日間を参照します。 つまり、モデルは、過去2四半期に発生した定義済みのすべてのコンバージョンイベントを考慮し、関連するコンバージョンイベントの56日前に発生したすべてのタッチポイントを探します。

**数式**:

必要なデータの最小長=トレーニングウィンドウ+ルックバックウィンドウ

>[!TIP]
>
> デフォルト設定のアプリケーションに必要なデータの最小長は次のとおりです。2四半期（180日）+ 56日= 236日。

例 :

- 過去90日間（3か月）以内に発生したコンバージョンイベントを属性化し、コンバージョンイベントの4週間前に発生したすべてのタッチポイントを追跡する必要があります。 入力データ期間は、過去90日間+ 28日間（4週間）に及びます。 トレーニング期間は90日で、ルックバック期間は28日間の合計118日です。

## Attribution AI出力データ

Attribution AIは次のように出力します。

- [生の詳細なスコア](#raw-granular-scores)
- [集計スコア](#aggregated-attribution-scores)

**出力スキーマの例：**

![](./images/input-output/schema_output.gif)

### 生の詳細なスコア{#raw-granular-scores}

Attribution AI出力のアトリビューションスコアは、可能な限り細かいレベルで出力され、任意のスコア列でスコアを分類できます。 これらのスコアをUIで表示するには、[生のスコアパス](#raw-score-path)を表示するセクションを読んでください。 APIを使用してスコアをダウンロードするには、[Attribution AI](./download-scores.md)ドキュメントのスコアのダウンロードを参照してください。

>[!NOTE]
>
> 次のいずれかが真の場合にのみ、スコア出力データセットの入力データセットから目的のレポート列を見ることができます。
> - 「レポート」列は、タッチポイント設定またはコンバージョン定義設定の一部として、設定ページに含まれます。
> - レポート列は、追加のスコアデータセット列に含まれます。


次の表に、生のスコアの例の出力に含まれるスキーマフィールドの概要を示します。

| 列名(DataType) | Null許容 | 説明 |
| --- | --- | --- |
| timestamp (DateTime) | False | コンバージョンイベントまたは観察が発生した時間。<br> **例：** 2020-06-09T00:01:51.000Z |
| identityMap(Map) | True | CEE XDM形式に類似したユーザーのidentityMap。 |
| eventType（文字列型） | True | この時系列レコードの主なイベントタイプです。<br> **例：** &quot;Order&quot;, &quot;Purchase&quot;, &quot;Visit&quot; |
| eventMergeId (String) | True | 基本的に同じイベントであるか、結合する必要がある複数の[!DNL Experience Events]を相互に関連付けまたは結合するID。 これは、取り込みの前にData Producerによって入力されることを意図しています。<br> **例：** 575525617716-0-edc2ed37-1ab-4750-a820-1c2b3844b8c4 |
| _id（文字列） | False | 時系列イベントの一意の識別子。<br> **例：** 4461-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _tenantId （オブジェクト） | False | テンタントIDに対する修正を行う最上位レベルのオブジェクトコンテナです。<br> **例：** _atsdsnrmmsv2 |
| your_スキーマ_name （オブジェクト） | False | スコア行とコンバージョンイベントに関連付けられているすべてのタッチポイントイベントとそのメタデータ。<br> **例：** Attribution AIスコア — モデル名__2020 |
| segmentation（文字列） | True | モデルが作成する地域分類などのコンバージョンセグメント。 セグメントが存在しない場合、セグメントはconversionNameと同じです。<br> **例：** ORDER_US |
| conversionName (String) | True | セットアップ中に設定された変換の名前。<br> **例：** 注文、リード、訪問 |
| conversion （オブジェクト）#conversionオブジェクト# | False | 変換メタデータ列 |
| dataSource (String) | True | データソースのグローバル一意の識別。<br> **例：** Adobe Analytics |
| eventSource（文字列型） | True | 実際のイベントが発生した時のソース。<br> **例：** Adobe.com |
| eventType（文字列型） | True | この時系列レコードの主なイベントタイプです。<br> **例：** 順序 |
| geo（文字列） | True | コンバージョンが配信された地理的な場所`placeContext.geo.countryCode`。<br> **例：** US |
| priceTotal (重複) | True | コンバージョン<br>を通じて得られた売上高 **例：** 99.9 |
| product（文字列） | True | 製品自体のXDM識別子。<br> **例：** RX 1080 ti |
| productType (String) | True | この製品表示に対してユーザーに提示される、製品の表示名。<br> **例：** Gpu |
| quantity（整数） | True | 変換中に購入された数量。<br> **例：** 1 1080 ti |
| receivedTimestamp (DateTime) | True | 受け取った変換のタイムスタンプ。<br> **例：** 2020-06-09T00:01:51.000Z |
| skuId（文字列） | True | 在庫保管単位(SKU)は、仕入先によって定義された製品の固有の識別子です。<br> **例：** MJ-03-XS-Black |
| timestamp (DateTime) | True | 変換のタイムスタンプ。<br> **例：** 2020-06-09T00:01:51.000Z |
| passThrough （オブジェクト）#passThroughオブジェクト# | True | モデルの設定時にユーザーが指定した追加のスコアデータセット列。 |
| commerce_order_purchaseCity （文字列） | True | 追加のスコアデータセット列。<br> **例：** city :サンノゼ |
| customerProfile （オブジェクト）#customerProfileオブジェクト# | False | モデルの構築に使用されるユーザーのIDの詳細。 |
| identity（オブジェクト） | False | `id`や`namespace`など、モデルの構築に使用するユーザーの詳細が含まれます。 |
| id（文字列） | True | cookie IDやAID、MCIDなどのユーザーのID。<br> **例：** 1734876272540865634468320891369597404 |
| 名前空間（文字列） | True | パスを構築し、それによってモデルを構築するために使用されるID名前空間。<br> **例：** aid |
| touchpointsDetail（オブジェクト配列） | True | タッチポイントの発生またはタイムスタンプに基づいて並べ替えられた変換につながるタッチポイントの詳細のリスト。 |
| touchpointName（文字列） | True | セットアップ中に設定されたタッチポイントの名前。<br> **例：** PAID_SEARCH_CLICK |
| scores （オブジェクト）#scoresオブジェクト# | True | スコアとしてのこのコンバージョンへのタッチポイントの貢献度。 このオブジェクト内で生成されるスコアについて詳しくは、[集計されたアトリビューションスコア](#aggregated-attribution-scores)の節を参照してください。 |
| touchPoint （オブジェクト） | True | タッチポイントメタデータ このオブジェクト内で生成されるスコアの詳細については、[集計スコア](#aggregated-scores)の節を参照してください。 |

### 生のスコアパス(UI)を表示{#raw-score-path}

生のスコアへのパスをUIで表示できます。 開始するには、プラットフォームUIで「**[!UICONTROL スキーマ]**」を選択し、「**[!UICONTROL 参照]**」タブからアトリビューションAIスコアスキーマを検索して選択します。

![スキーマの選択](./images/input-output/schemas_browse.png)

次に、UIの&#x200B;**[!UICONTROL 構造]**&#x200B;ウィンドウ内のフィールドを選択し、**[!UICONTROL フィールドのプロパティ]**&#x200B;タブが開きます。 **[!UICONTROL Field properties]**&#x200B;内には、生のスコアに対応するパスフィールドがあります。

![スキーマを選択](./images/input-output/field_properties.png)


### 集計されたアトリビューションスコア{#aggregated-attribution-scores}

集計スコアは、日付範囲が30日未満の場合は、プラットフォームUIからCSV形式でダウンロードできます。

Attribution AI は、アトリビューションスコアの 2 つのカテゴリ（アルゴリズムスコアとルールベーススコア）をサポートしています。

Attribution AI では、増分スコアと影響スコアの 2 種類のアルゴリズムスコアを生成します。影響スコアは、各マーケティングタッチポイントがコンバージョンに寄与している割合です。増分スコアは、マーケティングタッチポイントで直接引き起こされたわずかな影響の量です。増分スコアと影響スコアの主な違いは、増分スコアがベースライン効果を考慮に入れていることです。コンバージョンが、先行するマーケティングタッチポイントによってのみ生じるとは考えられません。

Adobe Experience PlatformのUIからAttribution AIスキーマ出力例を簡単に見てみましょう。

![](./images/input-output/schema_screenshot.png)

これらの各アトリビューションスコアについて詳しくは、次の表を参照してください。

| アトリビューションスコア | 説明 |
| ----- | ----------- |
| 影響（アルゴリズム） | 影響スコアは、各マーケティングタッチポイントがコンバージョンに寄与している割合です。 |
| 増分（アルゴリズム） | 増分スコアは、マーケティングタッチポイントで直接引き起こされたわずかな影響の量です。 |
| ファーストタッチ | コンバージョンパスの最初のタッチポイントにすべてのクレジットを割り当てるルールベースのアトリビューションスコアです。 |
| ラストタッチ | コンバージョンに最も近いタッチポイントにすべてのクレジットを割り当てるルールベースのアトリビューションスコアです。 |
| 線形 | コンバージョンパスの各タッチポイントに同等のクレジットを割り当てるルールベースのアトリビューションスコアです。 |
| U 字形 | ルールベースのアトリビューションスコアで、クレジットの 40% を最初のタッチポイントに、40% を最後のタッチポイントに割り当て、残りの 20% を他のタッチポイントに均等に分配します。 |
| タイムディケイ | コンバージョンに近いタッチポイントが、コンバージョンから遠いタッチポイントよりも多くのクレジットを受け取るルールベースのアトリビューションスコアです。 |

**生のスコア参照（アトリビューションスコア）**

以下の表に、アトリビューションスコアと生のスコアの対応関係を示します。 生のスコアをダウンロードする場合は、[Attribution AI](./download-scores.md)のドキュメントのダウンロード中のスコアを参照してください。

| アトリビューションスコア | 生のスコア参照列 |
| --- | --- |
| 影響（アルゴリズム） | _tenantID.your_tenant_name.element.touchpoint.algorithmicInfluced |
| 増分（アルゴリズム） | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.algorithmicInfluenced |
| ファーストタッチ | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.firstTouch |
| ラストタッチ | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.lastTouch |
| 線形 | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.linear |
| U 字形 | _tenantID.your_element_name.touchpointsDetail.element.touchpoint.uShape |
| タイムディケイ | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.decayUnits |

### 集計スコア{#aggregated-scores}

集計スコアは、日付範囲が30日未満の場合は、プラットフォームUIからCSV形式でダウンロードできます。 これらの各集計列の詳細については、次の表を参照してください。

| 列名 | 制約 | Null許容 | 説明 |
| --- | --- | --- | --- |
| customerevents_date (DateTime) | ユーザー定義および固定形式 | False | YYYY-MM-DD形式の顧客イベント日。<br> **例**:2016-05-02 |
| mediatouchpoints_date (DateTime) | ユーザー定義および固定形式 | True | YYYY-MM-DD形式のメディアタッチポイント日付<br> **例**:2017-04-21 |
| segment（文字列） | 計算済み | False | モデルが構築される地域分類などのコンバージョンセグメント。 セグメントがない場合、セグメントはconversion_scopeと同じです。<br> **例**:ORDER_AMER |
| conversion_scope （文字列） | ユーザー定義 | False | ユーザーが設定した変換の名前。<br> **例**:注文 |
| touchpoint_scope（文字列） | ユーザー定義 | True | ユーザー<br>が設定したタッチポイントの名前 **例**:PAID_SEARCH_CLICK |
| product（文字列） | ユーザー定義 | True | 製品のXDM識別子。<br> **例**:CC |
| product_type（文字列） | ユーザー定義 | True | この製品表示に対してユーザーに提示される、製品の表示名。<br> **例**:gpu、ラップトップ |
| geo（文字列） | ユーザー定義 | True | 変換が配信された地理的な場所(placeContext.geo.countryCode) <br> **例**:US |
| イベントタイプ（文字列） | ユーザー定義 | True | この時系列レコードの主なイベントタイプ<br> **例**:有料コンバージョン |
| media_type（文字列） | ENUM | False | メディアの種類が有料、所有、または獲得のいずれであるかを示します。<br> **例**:有料、所有 |
| チャネル（文字列） | ENUM | False | [!DNL Consumer Experience Event] XDMで類似のプロパティを持つチャネルの大まかな分類を提供するために使用される`channel._type`プロパティ。 <br> **例**:検索 |
| action（文字列） | ENUM | False | `mediaAction`プロパティは、エクスペリエンスイベントメディアアクションのタイプを提供するために使用されます。<br> **例**:CLICK |
| キャンペーングループ（文字列） | ユーザー定義 | True | 複数のキャンペーンが&#39;50%_DISCOUNT&#39;のようにグループ化されているキャンペーングループの名前です。<br> **例**:商業 |
| キャンペーン名（文字列） | ユーザー定義 | True | &#39;50%_DISCOUNT_USA&#39;や&#39;50%_DISCOUNT_ASIA&#39;のようなマーケティングキャンペーンの識別に使用するキャンペーンの名前です。<br> **例**:感謝祭セール |

**生のスコア参照（集計）**

次の表に、集計されたスコアと生のスコアとを示します。 生のスコアをダウンロードする場合は、[Attribution AI](./download-scores.md)のドキュメントのダウンロード中のスコアを参照してください。 UI内から生のスコアパスを表示するには、[このドキュメント内の生のスコアパス](#raw-score-path)を表示するセクションにアクセスします。

| 列名 | 生のスコア参照列 |
| --- | --- |
| customrevents_date | timestamp |
| mediatouchpoints_date | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.timestamp |
| セグメント | _tenantID.your_tenant_name.segmentation |
| conversion_scope | _tenantID.your_tenant_name.conversion.conversionName |
| touchpoint_scope | _tenantID.your_tenant_name.touchpointsDetail.element.touchpointName |
| product | _tenantID.your_tenant_name.conversion.product |
| product_type | _tenantID.your_product_name.conversion.product_type |
| 地域 | _tenantID.your_tenant_name.conversion.geo |
| イベントタイプ | eventType |
| media_type | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.mediaType |
| チャネル | _tenantID.your_スキーマ_名.touchpointsDetail.element.touchpoint.mediaChannel |
| action | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.mediaAction |
| キャンペーン_グループ | _tenantID.your_tenant_name.touchpointsDetail.element.touchpoint.campaignGroup |
| キャンペーン名 | _tenantID.your_スキーマ_名.touchpointsDetail.element.touchpoint.campaignName |


## 次の手順 {#next-steps}

データを準備し、すべての資格情報とスキーマを準備したら、[Attribution AIユーザーガイド](./user-guide.md)に従って開始します。 このガイドでは、Attribution AIのインスタンスの作成手順を説明します。
