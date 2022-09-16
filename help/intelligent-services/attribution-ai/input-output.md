---
keywords: Experience Platform；はじめに；Attribution Ai；人気のトピック；Attribution Ai 入力；Attribution Ai 出力；
feature: Attribution AI
title: Attribution AIの入出力
topic-legacy: Input and Output data for Attribution AI
description: 次のドキュメントでは、Attribution AIで使用される様々な入力と出力の概要を説明します。
exl-id: d6dbc9ee-0c1a-4a5f-b922-88c7a36a5380
source-git-commit: e33d59c4ac28f55ba6ae2fc073d02f8738159263
workflow-type: tm+mt
source-wordcount: '2504'
ht-degree: 14%

---

# での入力と出力 [!DNL Attribution AI]

次のドキュメントでは、 [!DNL Attribution AI].

## [!DNL Attribution AI] 入力データ

Attribution AIは、次のデータセットを分析してアルゴリズムスコアを計算することで機能します。

- Adobe Analyticsデータセット（を使用） [Analytics ソースコネクタ](../../sources/tutorials/ui/create/adobe-applications/analytics.md)
- Adobe Experience Platformスキーマの一般的なエクスペリエンスイベント (EE) データセット
- 消費者エクスペリエンスイベント (CEE) データセット

これで、 **id マップ** （フィールド）。各データセットが ECID などの同じ ID タイプ（名前空間）を共有する場合に使用します。 ID と名前空間を選択すると、ID 列の完全性指標が表示され、結び付けられるデータの量が示されます。 複数のデータセットの追加について詳しくは、 [Attribution AIユーザーガイド](./user-guide.md#identity).

チャネル情報は、デフォルトではマッピングされない場合があります。 場合によっては、 mediaChannel （フィールド）が空白の場合、必須の列なので、フィールドを mediaChannel にマッピングするまで「続行」できません。 データセットでチャネルが検出された場合、デフォルトで mediaChannel にマッピングされます。 その他の列は、 **メディアタイプ** および **メディアアクション** はオプションです。

チャネルフィールドのマッピング後、「イベントの定義」の手順に進み、コンバージョンイベントとタッチポイントイベントを選択して、個々のデータセットから特定のフィールドを選択できます。

>[!IMPORTANT]
>
>Adobe Analyticsソースコネクタでは、データのバックフィルに最大 4 週間かかる場合があります。 コネクタを最近設定した場合は、Attribution AIセットに必要な最小長のデータが含まれていることを確認する必要があります。 次を確認してください： [履歴データ](#data-requirements) 」セクションを参照して、正確なアルゴリズムスコアを計算するのに十分なデータがあることを確認してください。

の設定の詳細については、 [!DNL Consumer Experience Event] (CEE) スキーマについては、 [インテリジェントサービスのデータ準備](../data-preparation.md) ガイド。 Adobe Analyticsデータのマッピングについて詳しくは、 [Analytics フィールドのマッピング](../../sources/connectors/adobe-applications/analytics.md) ドキュメント。

すべての列が [!DNL Consumer Experience Event] (CEE) スキーマはAttribution AIに必須です。

タッチポイントは、スキーマ内または選択したデータセット内で以下に推奨されるフィールドを使用して設定できます。

| 推奨列 | 必要な条件： |
| --- | --- |
| プライマリID フィールド | タッチポイント/コンバージョン |
| タイムスタンプ | タッチポイント/コンバージョン |
| チャネル._タイプ | タッチポイント |
| Channel.mediaAction | タッチポイント |
| Channel.mediaType | タッチポイント |
| Marketing.trackingCode | タッチポイント |
| Marketing.campaignname | タッチポイント |
| Marketing.campaigngroup | タッチポイント |
| コマース | 変換 |

通常、アトリビューションは、「コマース」下の注文、購入、チェックアウトなどのコンバージョン列で実行されます。 「チャネル」および「マーケティング」の列は、Attribution AIのタッチポイント ( 例： `channel._type = 'https://ns.adobe.com/xdm/channel-types/email'`) をクリックします。 最適な結果とインサイトを得るには、できる限り多くのコンバージョン列とタッチポイント列を含めることを強くお勧めします。 また、上記の列に限定されるわけではありません。 その他の推奨列またはカスタム列をコンバージョンまたはタッチポイント定義として含めることができます。

タッチポイントの設定に関連するチャネルまたはキャンペーン情報がいずれかの Mixin に存在するか、フィールドを通過する場合、エクスペリエンスイベント (EE) データセットには、チャネルとマーケティングの Mixin を明示的に含める必要はありません。

>[!TIP]
>
>CEE スキーマでAdobe Analyticsデータを使用している場合、通常、Analytics のタッチポイント情報は `channel.typeAtSource` ( 例： `channel.typeAtSource = 'email'`) をクリックします。

## 履歴データ {#data-requirements}

>[!IMPORTANT]
>
> Attribution AIの機能に最低限必要なデータ量は、次のとおりです。
> - 適切なモデルを実行するには、少なくとも 3 か月（90 日）のデータを提供する必要があります。
> - 少なくとも 1,000 個のコンバージョンが必要です。


Attribution AIには、モデルトレーニングの入力として履歴データが必要です。 必要なデータの期間は、主に次の 2 つの主要な要因によって決まります。トレーニングウィンドウとルックバックウィンドウ。 トレーニング期間が短い入力は、最近のトレンドに対する感度が高くなります。トレーニング期間が長いと、より安定した正確なモデルを作成できます。 ビジネス目標を最も適切に表す履歴データを使用して目標をモデル化することが重要です。

この [トレーニングウィンドウ設定](./user-guide.md#training-window) 発生時間に基づいて、モデルトレーニングに含めるように設定されたコンバージョンイベントをフィルタリングします。 現在、トレーニング期間は最小 1 四半期（90 日）です。 この [ルックバックウィンドウ](./user-guide.md#lookback-window) は、このコンバージョンイベントに関連するコンバージョンイベントの何日前からのタッチポイントを含めるかを示す時間枠を提供します。 これら 2 つの概念を組み合わせて、アプリケーションに必要な入力データの量（日数で測定）を決定します。

デフォルトでは、Attribution AIはトレーニング期間を最新の 2 四半期（6 ヶ月）として定義し、ルックバック期間を 56 日として定義します。 つまり、モデルでは、過去 2 四半期に発生した定義済みのコンバージョンイベントをすべて考慮し、関連するコンバージョンイベントの 56 日前に発生したすべてのタッチポイントを探します。

**数式**:

必要なデータの最小長=トレーニングウィンドウ+ルックバックウィンドウ

>[!TIP]
>
> デフォルト設定を使用するアプリケーションで必要なデータの最小長は次のとおりです。2 四半期（180 日）+ 56 日= 236 日。

例：

- 過去 90 日（3 ヶ月）以内に発生したコンバージョンイベントを属性に設定し、コンバージョンイベントの 4 週間前に発生したすべてのタッチポイントを追跡する必要があります。 入力データの期間は、過去 90 日+ 28 日（4 週間）に及びます。 トレーニング期間は 90 日で、ルックバック期間は 28 日で、合計 118 日です。

## Attribution AI出力データ

Attribution AIは次の出力を行います。

- [未加工の精度スコア](#raw-granular-scores)
- [集計スコア](#aggregated-attribution-scores)

**出力スキーマの例：**

![](./images/input-output/schema_output.gif)

### 未加工の精度スコア {#raw-granular-scores}

Attribution AIは、任意のスコア列でスコアをスライスしてディスコアリングできるように、アトリビューションスコアをできる限り詳細なレベルで出力します。 UI でこれらのスコアを表示するには、 [raw スコアパスの表示](#raw-score-path). API を使用してスコアをダウンロードするには、 [スコアのダウンロード，Attribution AI](./download-scores.md) 文書。

>[!NOTE]
>
> 次のいずれかに該当する場合にのみ、スコア出力データセットの入力データセットから目的のレポート列を確認できます。
> - レポート列は、タッチポイントまたはコンバージョン定義設定の一部として、設定ページに含まれます。
> - レポート列は、追加のスコアデータセット列に含まれます。


次の表に、生のスコアの例の出力のスキーマフィールドの概要を示します。

| 列名（データタイプ） | Nullable | 説明 |
| --- | --- | --- |
| timestamp (DateTime) | False | コンバージョンイベントまたは観測が発生した時間。 <br> **例：** 2020-06-09T00:01:51.000Z |
| identityMap （マップ） | True | CEE XDM 形式に類似したユーザーの identityMap。 |
| eventType（文字列） | True | この時系列レコードの主なイベントタイプ。 <br> **例：** &quot;注文&quot;, &quot;購入&quot;, &quot;訪問&quot; |
| eventMergeId （文字列） | True | 複数のクロス集計または結合する ID [!DNL Experience Events] を結合します。 これは、取り込み前にデータプロデューサーによって設定されることを目的としています。 <br> **例：** 575525617716-0-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _id （文字列） | False | 時系列イベントの一意の ID。 <br> **例：** 4461-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _tenantId （オブジェクト） | False | テンタント ID に対応する最上位のオブジェクトコンテナ。 <br> **例：** _atsdsnrmmsv2 |
| your_schema_name (Object) | False | コンバージョンイベントに関連付けられているすべてのタッチポイントイベントとそのメタデータを含むスコア行。 <br> **例：** Attribution AIスコア — モデル名__2020 |
| セグメント化（文字列） | True | モデルが構築される地理特性などのコンバージョンセグメント。 セグメントが存在しない場合、セグメントは conversionName と同じになります。 <br> **例：** ORDER_US |
| conversionName (String) | True | セットアップ中に設定された変換の名前。 <br> **例：** 注文、リード、訪問 |
| conversion (Object) | False | コンバージョンメタデータ列。 |
| dataSource (String) | True | データソースのグローバルに一意の ID。 <br> **例：** Adobe Analytics |
| eventSource （文字列） | True | 実際のイベントが発生したソース。 <br> **例：** Adobe.com |
| eventType（文字列） | True | この時系列レコードの主なイベントタイプ。 <br> **例：** 注文 |
| geo (String) | True | コンバージョンが配信された地域 `placeContext.geo.countryCode`. <br> **例：** US |
| priceTotal (Double) | True | コンバージョンによって得られた売上高 <br> **例：** 99.9 |
| product (String) | True | 製品自体の XDM 識別子。 <br> **例：** RX 1080 ti |
| productType (String) | True | この製品表示のユーザーに提示された製品の表示名。 <br> **例：** グプス |
| 数量（整数） | True | コンバージョン中に購入された数量。 <br> **例：** 1 1080 分の 1 |
| receivedTimestamp (DateTime) | True | コンバージョンのタイムスタンプを受け取りました。 <br> **例：** 2020-06-09T00:01:51.000Z |
| skuId（文字列） | True | 在庫管理単位 (SKU)。ベンダーによって定義された製品の一意の ID。 <br> **例：** MJ-03-XS-Black |
| timestamp (DateTime) | True | 変換のタイムスタンプ。 <br> **例：** 2020-06-09T00:01:51.000Z |
| passThrough （オブジェクト） | True | モデルの設定時にユーザーが指定した追加のスコアデータセット列。 |
| commerce_order_purchaseCity （文字列） | True | 追加のスコアデータセット列。 <br> **例：** 市区町村：サンノゼ |
| customerProfile (Object) | False | モデルの構築に使用されたユーザーの ID 詳細。 |
| id (Object) | False | モデルの構築に使用されるユーザーの詳細が含まれます。例： `id` および `namespace`. |
| id （文字列） | True | ユーザーの ID ID(Cookie ID、Adobe Analytics ID(AAID)、Experience CloudID（ECID、MCID、訪問者 ID とも呼ばれます）など。 <br> **例：** 17348762725408656344688320891369597404 |
| namespace (String) | True | パスを構築し、それによってモデルを構築するために使用される ID 名前空間。 <br> **例：** aaid |
| touchpointsDetail (Object Array) | True | 次の順で並べ替えられたコンバージョンにつながるタッチポイントの詳細のリスト | タッチポイントの発生またはタイムスタンプ。 |
| touchpointName (String) | True | セットアップ中に設定されたタッチポイントの名前。 <br> **例：** PAID_SEARCH_CLICK |
| scores (Object) | True | スコアとしてのこのコンバージョンへのタッチポイントの貢献度。 このオブジェクト内で生成されるスコアについて詳しくは、 [集計されたアトリビューションスコア](#aggregated-attribution-scores) 」セクションに入力します。 |
| touchPoint （オブジェクト） | True | タッチポイントメタデータ。 このオブジェクト内で生成されるスコアについて詳しくは、 [集計スコア](#aggregated-scores) 」セクションに入力します。 |

### 生のスコアパスの表示 (UI) {#raw-score-path}

生のスコアへのパスは、UI で表示できます。 最初に選択 **[!UICONTROL スキーマ]** 次に、Platform UI で、 **[!UICONTROL 参照]** タブをクリックします。

![スキーマを選択](./images/input-output/schemas_browse.png)

次に、 **[!UICONTROL 構造]** UI のウィンドウ、 **[!UICONTROL フィールドプロパティ]** タブが開きます。 内 **[!UICONTROL フィールドプロパティ]** は、未加工のスコアにマッピングされるパスフィールドです。

![スキーマを選択](./images/input-output/field_properties.png)

### 集計されたアトリビューションスコア {#aggregated-attribution-scores}

日付範囲が 30 日未満の場合は、集計スコアを CSV 形式で Platform UI からダウンロードできます。

Attribution AI は、アトリビューションスコアの 2 つのカテゴリ（アルゴリズムスコアとルールベーススコア）をサポートしています。

Attribution AI では、増分スコアと影響スコアの 2 種類のアルゴリズムスコアを生成します。影響スコアは、各マーケティングタッチポイントがコンバージョンに寄与している割合です。増分スコアは、マーケティングタッチポイントで直接引き起こされたわずかな影響の量です。増分スコアと影響スコアの主な違いは、増分スコアがベースライン効果を考慮に入れていることです。コンバージョンが、先行するマーケティングタッチポイントによってのみ生じるとは考えられません。

Adobe Experience Platform UI からのAttribution AIスキーマ出力例を簡単に見てみましょう。

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

**未加工スコアリファレンス（アトリビューションスコア）**

次の表に、アトリビューションのスコアと生のスコアのマッピングを示します。 生のスコアをダウンロードする場合は、 [スコアのダウンロード，Attribution AI](./download-scores.md) ドキュメント。

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

日付範囲が 30 日未満の場合は、集計スコアを CSV 形式で Platform UI からダウンロードできます。 これらの各集計列の詳細については、以下の表を参照してください。

| 列名 | 制約 | Nullable | 説明 |
| --- | --- | --- | --- |
| customerevents_date (DateTime) | ユーザー定義および固定形式 | False | YYYY-MM-DD 形式の顧客イベント日。 <br> **例**:2016-05-02 |
| mediatouchpoints_date (DateTime) | ユーザー定義および固定形式 | True | メディアタッチポイント日（YYYY-MM-DD 形式） <br> **例**:2017-04-21 |
| セグメント（文字列） | 計算済み | False | コンバージョンセグメント（モデルが構築される地理特性など）。 セグメントが存在しない場合、セグメントは conversion_scope と同じです。 <br> **例**:ORDER_AMER |
| conversion_scope (String) | ユーザー定義 | False | ユーザーが設定したコンバージョンの名前。 <br> **例**:注文 |
| touchpoint_scope（文字列） | ユーザー定義 | True | ユーザーが設定したタッチポイントの名前 <br> **例**:PAID_SEARCH_CLICK |
| product (String) | ユーザー定義 | True | 製品の XDM 識別子。 <br> **例**:CC |
| product_type（文字列） | ユーザー定義 | True | この製品表示のユーザーに提示された製品の表示名。 <br> **例**:gpu、ノートパソコン |
| geo (String) | ユーザー定義 | True | コンバージョンが配信された地理的な場所 (placeContext.geo.countryCode) <br> **例**:US |
| event_type(String) | ユーザー定義 | True | この時系列レコードの主なイベントタイプ <br> **例**:有料コンバージョン |
| media_type (String) | ENUM | False | メディアタイプが有料か、所有か、獲得かを表します。 <br> **例**:有料、所有 |
| channel （文字列） | ENUM | False | この `channel._type` プロパティを使用して、 [!DNL Consumer Experience Event] XDM。 <br> **例**:検索 |
| action (String) | ENUM | False | この `mediaAction` プロパティは、エクスペリエンスイベントのメディアアクションのタイプを提供するために使用されます。 <br> **例**:クリック |
| campaign_group（文字列） | ユーザー定義 | True | 複数のキャンペーンがグループ化されたキャンペーングループの名前（「50%_DISCOUNT」など）。 <br> **例**:商用 |
| campaign_name（文字列） | ユーザー定義 | True | マーケティングキャンペーンの識別に使用するキャンペーンの名前（「50%_DISCOUNT_USA」や「50%_DISCOUNT_ASIA」など）。 <br> **例**:感謝祭の販売 |

**生のスコアの参照（集計）**

次の表に、集計されたスコアを生のスコアにマッピングします。 生のスコアをダウンロードする場合は、 [スコアのダウンロード，Attribution AI](./download-scores.md) ドキュメント。 UI 内から生のスコアパスを表示するには、 [raw スコアパスの表示](#raw-score-path) 」と入力します。

| 列名 | 生のスコアの参照列 |
| --- | --- |
| customrevents_date | timestamp |
| mediatouchpoints_date | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.timestamp |
| セグメント | _tenantID.your_schema_name.segmentation |
| conversion_scope | _tenantID.your_schema_name.conversion.conversionName |
| touchpoint_scope | _tenantID.your_schema_name.touchpointsDetail.element.touchpointName |
| product | _tenantID.your_schema_name.conversion.product |
| product_type | _tenantID.your_schema_name.conversion.product_type |
| 地域 | _tenantID.your_schema_name.conversion.geo |
| event_type | eventType |
| media_type | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.mediaType |
| チャネル | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.mediaChannel |
| アクション | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.mediaAction |
| campaign_group | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.campaignGroup |
| campaign_name | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.campaignName |

>[!IMPORTANT]
>
> - Attribution AIは、更なるトレーニングとスコアリングに、更新されたデータのみを使用します。 同様に、データの削除をリクエストする場合、顧客 AI は削除されたデータを使用しないようにします。
> - アトリビューション AI では Platform データセットを活用します。 ブランドが受け取る可能性のある消費者の権利リクエストをサポートするには、Platform Privacy Service を使用して、アクセスおよび削除に対する消費者のリクエストを送信し、データレイク、ID サービス、リアルタイム顧客プロファイルをまたいでデータを削除する必要があります。
> - モデルの入出力に使用するすべてのデータセットは、Platform のガイドラインに従います。 Platform データ暗号化は、保存中および送信中のデータに適用されます。詳しくは、ドキュメントを参照してください。 [データ暗号化](../../../help/landing/governance-privacy-security/encryption.md)


## 次の手順 {#next-steps}

データを準備し、すべての資格情報とスキーマを設定したら、次の手順に従って開始します。 [Attribution AIユーザーガイド](./user-guide.md). このガイドでは、Attribution AIのインスタンスの作成手順を説明します。
