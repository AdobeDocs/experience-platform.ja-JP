---
keywords: Experience Platform；はじめに；Attribution Ai；人気のあるトピック；Attribution Ai入力；Attribution Ai出力；
solution: Experience Platform, Intelligent Services
title: Attribution AIの入出力
topic-legacy: Input and Output data for Attribution AI
description: 次のドキュメントでは、データで使用される様々な入力と出力の概要を説明します。
exl-id: d6dbc9ee-0c1a-4a5f-b922-88c7a36a5380
source-git-commit: a49218103669758404a4ddf3f9833b8b2d9b7fc6
workflow-type: tm+mt
source-wordcount: '2230'
ht-degree: 14%

---

# [!DNL Attribution AI]の入出力

以下のドキュメントでは、[!DNL Attribution AI]で使用される様々な入力と出力の概要を説明します。

## [!DNL Attribution AI] 入力データ

Attribution AIは、次のデータセットのいずれかを分析してアルゴリズムスコアを計算することで機能します。

- コンシューマーエクスペリエンスイベント(CEE)データセット
- Adobe Analyticsデータセット（[Analyticsソースコネクタ](../../sources/tutorials/ui/create/adobe-applications/analytics.md)を使用）

>[!IMPORTANT]
>
>Adobe Analyticsソースコネクタでは、データのバックフィルに最大4週間かかる場合があります。 コネクタを最近設定した場合は、Attribution AIに必要なデータの長さが最小限であることを確認する必要があります。 [履歴データ](#data-requirements)の節を参照して、正確なアルゴリズムスコアを計算するのに十分なデータがあることを確認してください。

[!DNL Consumer Experience Event](CEE)スキーマの設定について詳しくは、『[インテリジェントサービスデータ準備](../data-preparation.md)』ガイドを参照してください。 Adobe Analyticsデータのマッピングについて詳しくは、 [Analytics field mappings](../../sources/connectors/adobe-applications/analytics.md)のドキュメントを参照してください。

Attribution AIの場合、[!DNL Consumer Experience Event](CEE)スキーマのすべての列が必須とは限りません。

>[!NOTE]
>
> 次の9列は必須です。追加の列はオプションですが、[!DNL Customer AI]や[!DNL Journey AI]などの他のAdobeソリューションで同じデータを使用する場合は、追加の列を推奨/必要にします。

| 必須列 | 必要 |
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

通常、アトリビューションは「コマース」の下の注文、購入、チェックアウトなどのコンバージョン列で実行されます。 「チャネル」と「マーケティング」の列は、Attribution AIのタッチポイントを定義するために使用されます（例：`channel._type = 'https://ns.adobe.com/xdm/channel-types/email'`）。 最適な結果とインサイトを得るには、できる限り多くのコンバージョン列とタッチポイント列を含めることを強くお勧めします。 また、上記の列に制限されるわけではありません。 その他の推奨列またはカスタム列をコンバージョンまたはタッチポイント定義として含めることができます。

>[!TIP]
>
>CEEスキーマでAdobe Analyticsデータを使用している場合、Analyticsのタッチポイント情報は通常`channel.typeAtSource`に格納されます（例：`channel.typeAtSource = 'email'`）。

以下の列は必須ではありませんが、情報がある場合はCEEスキーマに含めることをお勧めします。

**その他の推奨列：**
- web.webReferer
- web.webInteraction
- web.webPageDetails
- xdm:productListItems

## 履歴データ {#data-requirements}

>[!IMPORTANT]
>
> Attribution AIの機能に必要な最小量は、次のとおりです。
> - 適切なモデルを実行するには、少なくとも3ヶ月（90日）のデータを提供する必要があります。
> - 少なくとも1,000個のコンバージョンが必要です。


Attribution AIには、モデルトレーニングの入力として履歴データが必要です。 必要なデータの期間は、主に次の2つの重要な要因によって決まります。トレーニング期間およびルックバック期間。 トレーニング期間が短い入力は、最近のトレンドに対する感度が高くなり、トレーニング期間が長いと、より安定した正確なモデルを作成できます。 ビジネス目標を最も適切に表す履歴データを使用して目標をモデル化することが重要です。

[トレーニング期間設定](./user-guide.md#training-window)は、発生時間に基づいて、モデルトレーニングに含めるように設定されたコンバージョンイベントをフィルタリングします。 現在、最小トレーニング期間は1四半期（90日）です。 [ルックバックウィンドウ](./user-guide.md#lookback-window)には、このコンバージョンイベントに関連するコンバージョンイベントのタッチポイントの何日前に含めるかを示す時間枠が表示されます。 これら2つの概念を組み合わせて、1つのアプリケーションに必要な入力データの量（日単位）を決定します。

デフォルトでは、Attribution AIはトレーニング期間を最新の2四半期（6ヶ月）として定義し、ルックバック期間を56日として定義します。 つまり、モデルは、過去2四半期に発生した定義済みのコンバージョンイベントをすべて考慮し、関連するコンバージョンイベントの56日前に発生したすべてのタッチポイントを探します。

**数式**:

必要なデータの最小長=トレーニングウィンドウ+ルックバックウィンドウ

>[!TIP]
>
> デフォルト設定を使用するアプリケーションに必要なデータの最小長は次のとおりです。2四半期（180日）+ 56日= 236日。

例：

- 過去90日（3ヶ月）以内に発生したコンバージョンイベントを属性に含め、コンバージョンイベントの4週間前に発生したすべてのタッチポイントを追跡します。 入力データの期間は、過去90日間+ 28日間（4週間）です。 トレーニング期間は90日、ルックバック期間は28日で、合計118日です。

## Attribution AI出力データ

Attribution AIは次のように出力します。

- [未加工の詳細スコア](#raw-granular-scores)
- [集計スコア](#aggregated-attribution-scores)

**出力スキーマの例：**

![](./images/input-output/schema_output.gif)

### 未加工の詳細スコア {#raw-granular-scores}

Attribution AIは、任意のスコア列でスコアをスライスして多角的に分析できるよう、可能な限り詳細なレベルでアトリビューションスコアを出力します。 UIでこれらのスコアを表示するには、[生のスコアパス](#raw-score-path)の表示の節をお読みください。 APIを使用してスコアをダウンロードするには、Attribution AI](./download-scores.md)ドキュメントの[スコアのダウンロードを参照してください。

>[!NOTE]
>
> 次のいずれかに該当する場合にのみ、スコア出力データセットの入力データセットから目的のレポート列を確認できます。
> - レポート列は、タッチポイントまたはコンバージョン定義設定の一部として、設定ページに含まれます。
> - レポート列は、追加のスコアデータセット列に含まれます。


次の表に、生のスコアの例の出力のスキーマフィールドの概要を示します。

| 列名（データ型） | Nullable | 説明 |
| --- | --- | --- |
| timestamp (DateTime) | False | コンバージョンイベントまたは監視が発生した時間。<br> **例：** 2020-06-09T00:01:51.000Z |
| identityMap（マップ） | True | CEE XDM形式に類似したユーザーのidentityMap。 |
| eventType（文字列） | True | この時系列レコードのプライマリイベントタイプ。<br> **例：** &quot;Order&quot;, &quot;Purchase&quot;, &quot;Visit&quot; |
| eventMergeId （文字列） | True | 基本的に同じイベントであるか結合する必要がある複数の[!DNL Experience Events]を相互に関連付けるまたは結合するID。 これは、取り込みの前にデータプロデューサーによって設定されることを意図しています。<br> **例：** 575525617716-0-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _id（文字列） | False | 時系列イベントの一意の識別子。<br> **例：** 4461-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _tenantId （オブジェクト） | False | テンタントIDに対応する最上位のオブジェクトコンテナです。 <br> **例：**  _atsdsnrmmsv2 |
| your_schema_name (Object) | False | コンバージョンイベントに関連付けられているすべてのタッチポイントイベントとそのメタデータを含むスコア行。<br> **例：** Attribution AIスコア — モデル名__2020 |
| セグメント化（文字列） | True | モデルが構築される地域特性などのコンバージョンセグメント。 セグメントがない場合、セグメントはconversionNameと同じになります。<br> **例：** ORDER_US |
| conversionName (String) | True | セットアップ中に設定された変換の名前。<br> **例：** 注文、リード、訪問 |
| conversion （オブジェクト） | False | コンバージョンメタデータ列。 |
| dataSource （文字列） | True | データソースのグローバル一意の識別。<br> **例：** Adobe Analytics |
| eventSource（文字列） | True | 実際のイベントが発生したソース。<br> **例：** Adobe.com |
| eventType（文字列） | True | この時系列レコードのプライマリイベントタイプ。<br> **例：** Order |
| geo （文字列） | True | コンバージョンが配信された地理的な場所`placeContext.geo.countryCode`。<br> **例：** US |
| priceTotal (Double) | True | コンバージョン<br>で得られた売上高 **例：** 99.9 |
| product （文字列） | True | 製品自体のXDM識別子。<br> **例：** RX 1080 ti |
| productType （文字列） | True | この製品表示のユーザーに表示される、製品の表示名。<br> **例：** Gpus |
| quantity (integer) | True | 変換中に購入された数量。<br> **例：** 1 1080 ti |
| receivedTimestamp (DateTime) | True | 変換のタイムスタンプを受け取った。<br> **例：** 2020-06-09T00:01:51.000Z |
| skuId（文字列） | True | 在庫管理単位(SKU)：ベンダーによって定義された製品の一意の識別子。<br> **例：** MJ-03-XS-Black |
| timestamp (DateTime) | True | 変換のタイムスタンプ。<br> **例：** 2020-06-09T00:01:51.000Z |
| passThrough （オブジェクト）#passThroughオブジェクト# | True | ユーザーがモデルの設定時に指定した追加のスコアデータセット列。 |
| commerce_order_purchaseCity （文字列） | True | 追加のスコアデータセット列。<br> **例：** city :サンノゼ |
| customerProfile （オブジェクト） | False | モデルの構築に使用されるユーザーのID詳細。 |
| identity （オブジェクト） | False | `id`や`namespace`など、モデルの構築に使用するユーザーの詳細が含まれます。 |
| id （文字列） | True | Cookie IDやAAID、MCIDなどのユーザーのID。 <br> **例：** 17348762725408656344688320891369597404 |
| namespace (String) | True | パスを構築し、それによってモデルを作成するために使用されるID名前空間。<br> **例：** aaid |
| touchpointsDetail（オブジェクト配列） | True | コンバージョンの発注元となるタッチポイントの詳細のリスト | タッチポイントの発生またはタイムスタンプ。 |
| touchpointName（文字列） | True | セットアップ中に設定されたタッチポイントの名前。<br> **例：** PAID_SEARCH_CLICK |
| scores （オブジェクト） | True | このコンバージョンへのタッチポイントのスコア貢献度。 このオブジェクト内で生成されるスコアについて詳しくは、[集計されたアトリビューションスコア](#aggregated-attribution-scores)の節を参照してください。 |
| touchPoint （オブジェクト） | True | タッチポイントのメタデータ。 このオブジェクト内で生成されるスコアについて詳しくは、[集計されたスコア](#aggregated-scores)の節を参照してください。 |

### 生のスコアパスの表示(UI) {#raw-score-path}

UIで生のスコアへのパスを表示できます。 まず、Platform UIで「**[!UICONTROL スキーマ]**」を選択し、「**[!UICONTROL 参照]**」タブからアトリビューションAIスコアスキーマを検索して選択します。

![スキーマの選択](./images/input-output/schemas_browse.png)

次に、UIの&#x200B;**[!UICONTROL 構造]**&#x200B;ウィンドウ内のフィールドを選択し、「**[!UICONTROL フィールドのプロパティ]**」タブが開きます。 **[!UICONTROL フィールドプロパティ]**&#x200B;内には、生のスコアにマッピングされるパスフィールドが入ります。

![スキーマの選択](./images/input-output/field_properties.png)


### 集計されたアトリビューションスコア {#aggregated-attribution-scores}

日付範囲が30日未満の場合は、集計スコアをPlatform UIからCSV形式でダウンロードできます。

Attribution AI は、アトリビューションスコアの 2 つのカテゴリ（アルゴリズムスコアとルールベーススコア）をサポートしています。

Attribution AI では、増分スコアと影響スコアの 2 種類のアルゴリズムスコアを生成します。影響スコアは、各マーケティングタッチポイントがコンバージョンに寄与している割合です。増分スコアは、マーケティングタッチポイントで直接引き起こされたわずかな影響の量です。増分スコアと影響スコアの主な違いは、増分スコアがベースライン効果を考慮に入れていることです。コンバージョンが、先行するマーケティングタッチポイントによってのみ生じるとは考えられません。

Adobe Experience Platform UIからのAttribution AIスキーマ出力の例を簡単に示します。

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

**未加工スコアの参照（アトリビューションスコア）**

次の表に、アトリビューションのスコアと未加工のスコアのマッピングを示します。 生のスコアをダウンロードする場合は、Attribution AI](./download-scores.md)のドキュメントの[スコアのダウンロードを参照してください。

| アトリビューションスコア | 生のスコア参照列 |
| --- | --- |
| 影響（アルゴリズム） | _tenantID.your_schema_name.element.touchpoint.algorithmicInfluced |
| 増分（アルゴリズム） | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.algorithmicInfluced |
| ファーストタッチ | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.firstTouch |
| ラストタッチ | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.lastTouch |
| 線形 | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.linear |
| U 字形 | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.uShape |
| タイムディケイ | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.decayUnits |

### 集計スコア {#aggregated-scores}

日付範囲が30日未満の場合は、集計スコアをPlatform UIからCSV形式でダウンロードできます。 これらの各集計列の詳細については、以下の表を参照してください。

| 列名 | 制約 | Nullable | 説明 |
| --- | --- | --- | --- |
| customerevents_date (DateTime) | ユーザー定義および固定形式 | False | YYYY-MM-DD形式の顧客イベント日。<br> **例**:2016-05-02 |
| mediatouchpoints_date (DateTime) | ユーザー定義および固定形式 | True | メディアタッチポイントの日付（YYYY-MM-DD形式） <br> **例**:2017-04-21 |
| segment (String) | 計算 | False | コンバージョンセグメント（モデルが構築される地域特性など）。 セグメントがない場合、セグメントはconversion_scopeと同じです。<br> **例**:ORDER_AMER |
| conversion_scope (String) | ユーザー定義 | False | ユーザーが設定したコンバージョンの名前。<br> **例**:ORDER |
| touchpoint_scope（文字列） | ユーザー定義 | True | ユーザーが設定したタッチポイントの名前<br> **例**:PAID_SEARCH_CLICK |
| product （文字列） | ユーザー定義 | True | 製品のXDM識別子。<br> **例**:CC |
| product_type（文字列） | ユーザー定義 | True | この製品表示のユーザーに表示される、製品の表示名。<br> **例**:gpu、ラップトップ |
| geo （文字列） | ユーザー定義 | True | コンバージョンが配信された地理的な場所(placeContext.geo.countryCode) <br> **例**:米国 |
| event_type（文字列） | ユーザー定義 | True | この時系列レコードのプライマリイベントタイプ<br> **例**:有料コンバージョン |
| media_type（文字列） | ENUM | False | メディアタイプが有料、所有、有料かを示します。<br> **例**:有料、所有 |
| channel （文字列） | ENUM | False | [!DNL Consumer Experience Event] XDMで類似のプロパティを持つチャネルの大まかな分類を提供するために使用される`channel._type`プロパティ。 <br> **例**:検索 |
| action (String) | ENUM | False | `mediaAction`プロパティは、エクスペリエンスイベントメディアアクションのタイプを提供するために使用されます。<br> **例**:クリック |
| campaign_group（文字列） | ユーザー定義 | True | 複数のキャンペーンが「50%_DISCOUNT」のようにグループ化されているキャンペーングループの名前。<br> **例**:商用 |
| campaign_name（文字列） | ユーザー定義 | True | マーケティングキャンペーンの識別に使用するキャンペーンの名前（「50%_DISCOUNT_USA」または「50%_DISCOUNT_ASIA」など）。 <br> **例**:感謝祭の売り |

**生のスコアの参照（集計）**

次の表に、集計スコアと生のスコアのマッピングを示します。 生のスコアをダウンロードする場合は、Attribution AI](./download-scores.md)のドキュメントの[スコアのダウンロードを参照してください。 UI内から生のスコアパスを表示するには、このドキュメント内の[生のスコアパス](#raw-score-path)の表示に関する節を参照してください。

| 列名 | 生のスコアの参照列 |
| --- | --- |
| customrevents_date | timestamp |
| mediatouchpoints_date | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.timestamp |
| セグメント | _tenantID.your_schema_name.segmentation |
| conversion_scope | _tenantID.your_schema_name.conversion.conversionName |
| touchpoint_scope | _tenantID.your_schema_name.touchpointsDetail.element.touchpointName |
| product | _tenantID.your_schema_name.conversion.product |
| product_type | _tenantID.your_schema_name.conversion.product_type |
| geo | _tenantID.your_schema_name.conversion.geo |
| event_type | eventType |
| media_type | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.mediaType |
| チャネル | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.mediaChannel |
| action | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.mediaAction |
| campaign_group | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.campaignGroup |
| campaign_name | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.campaignName |


## 次の手順 {#next-steps}

データを準備し、すべての資格情報とスキーマを設定したら、まず『[Attribution AIユーザーガイド](./user-guide.md)』に従います。 このガイドでは、Attribution AIのインスタンスの作成手順を説明します。
