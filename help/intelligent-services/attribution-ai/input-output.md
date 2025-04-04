---
keywords: Experience Platform；はじめに；Attribution AI；人気のトピック；Attribution AI の入力；Attribution AI の出力；
feature: Attribution AI
title: アトリビューション AI の入出力
description: 次のドキュメントでは、アトリビューション AI で使用される様々な入力および出力の概要を説明します。
exl-id: d6dbc9ee-0c1a-4a5f-b922-88c7a36a5380
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2474'
ht-degree: 13%

---

# [!DNL Attribution AI] の入出力

次のドキュメントでは、[!DNL Attribution AI] で使用される様々な入力および出力の概要を説明します。

## [!DNL Attribution AI] 入力データ

アトリビューション AI は、次のデータセットを分析して、アルゴリズムスコアを計算することで機能します。

- [Analytics ソースコネクタ ](../../sources/tutorials/ui/create/adobe-applications/analytics.md) を使用したAdobe Analytics データセット
- Adobe Experience Platform スキーマからのエクスペリエンスイベント（EE）データセット一般
- 消費者エクスペリエンスイベント（CEE）データセット

各データセットが ECID など同じ ID タイプ（名前空間）を共有している場合、**ID マップ** （フィールド）に基づいて異なるソースから複数のデータセットを追加できるようになりました。 ID と名前空間を選択すると、ステッチされるデータの量を示す ID 列の完了度指標が表示されます。 複数のデータセットの追加について詳しくは、[ アトリビューション AI ユーザーガイド ](./user-guide.md#identity) を参照してください。

チャネル情報は、デフォルトでは常にマッピングされるわけではありません。 場合によっては、mediaChannel （フィールド）が空白の場合、フィールドを mediaChannel にマッピングしないと、必須の列ではないため、「続行」できません。 データセットでチャネルが検出されると、デフォルトで mediaChannel にマッピングされます。 **メディアタイプ** や **メディアアクション** など、その他の列は引き続きオプションです。

チャネルフィールドをマッピングしたら、「イベントを定義」の手順に進みます。この手順で、コンバージョンイベント、タッチポイントイベントを選択し、個々のデータセットから特定のフィールドを選択できます。

>[!IMPORTANT]
>
>Adobe Analytics ソースコネクタでは、データのバックフィルに最大 4 週間かかる場合があります。 最近コネクタを設定した場合は、データセットにアトリビューション AI に必要な最小データ長があることを確認する必要があります。 [ 履歴データ ](#data-requirements) の節を確認して、正確なアルゴリズムスコアを計算するのに十分なデータがあることを確認してください。

[!DNL Consumer Experience Event] （CEE）スキーマの設定について詳しくは、「[ インテリジェントサービスデータ準備 ](../data-preparation.md) ガイドを参照してください。 Analytics データのマッピングについて詳しくは、[Adobe Analytics フィールドのマッピング ](../../sources/connectors/adobe-applications/analytics.md) に関するドキュメントを参照してください。

[!DNL Consumer Experience Event] （CEE）スキーマ内の一部の列が、アトリビューション AI に必須ではありません。

スキーマまたは選択したデータセットで、以下で推奨する任意のフィールドを使用して、タッチポイントを設定できます。

| 推奨される列 | に必要 |
| --- | --- |
| プライマリID フィールド | タッチポイント/コンバージョン |
| タイムスタンプ | タッチポイント/コンバージョン |
| チャネル。_type | タッチポイント |
| Channel.mediaAction | タッチポイント |
| Channel.mediaType | タッチポイント |
| Marketing.trackingCode | タッチポイント |
| Marketing.campaignname | タッチポイント |
| Marketing.campaigngroup | タッチポイント |
| Commerce | 変換 |

通常、アトリビューションは、「コマース」の注文、購入、チェックアウトなどのコンバージョン列で実行されます。 「チャネル」と「マーケティング」の列は、Attribution AI のタッチポイントの定義に使用されます（例：`channel._type = 'https://ns.adobe.com/xdm/channel-types/email'`）。 最適な結果とインサイトを得るには、できるだけ多くのコンバージョン列とタッチポイント列を含めることを強くお勧めします。 また、上記の列に限定されません。 その他の推奨列やカスタム列をコンバージョンまたはタッチポイントの定義として含めることができます。

タッチポイントの設定に関連するチャネルまたはキャンペーン情報が Mixin フィールドまたはパススルーフィールドのいずれかに存在する限り、エクスペリエンスイベント（EE）データセットにチャネルおよびマーケティング Mixin を明示的に指定する必要はありません。

>[!TIP]
>
>CEE スキーマでAdobe Analytics データを使用している場合、Analytics のタッチポイント情報は通常、`channel.typeAtSource` （例：`channel.typeAtSource = 'email'`）に保存されます。

## 履歴データ {#data-requirements}

>[!IMPORTANT]
>
> アトリビューション AI が機能するために必要な最小データ量は、次のとおりです。
> - 適切なモデルを実行するには、少なくとも 3 か月（90 日）のデータを指定する必要があります。
> - 1,000 以上のコンバージョンが必要です。

アトリビューション AI では、モデルトレーニングの入力として履歴データが必要です。 必要なデータ期間は、主にトレーニングウィンドウとルックバックウィンドウの 2 つの主要な要因によって決定されます。 トレーニングウィンドウが短い入力は最近のトレンドの影響を受けやすくなりますが、トレーニングウィンドウが長いほど、より安定した正確なモデルを作成するのに役立ちます。 ビジネス目標を最も的確に表す履歴データを使用して目標をモデル化することが重要です。

[ トレーニングウィンドウ設定 ](./user-guide.md#training-window) は、モデルトレーニングに含める設定コンバージョンイベントを、発生時間に基づいてフィルタリングします。 現在、最小トレーニングウィンドウは 1 四半期（90 日）です。 [ ルックバックウィンドウ ](./user-guide.md#lookback-window) は、このコンバージョンイベントに関連するコンバージョンイベントタッチポイントまでの日数を示す時間枠を提供します。 これら 2 つの概念の組み合わせによって、アプリケーションに必要な入力データの量（日数）が決まります。

デフォルトでは、アトリビューション AI はトレーニングウィンドウを最新の 2 四半期（6 か月）として定義し、ルックバックウィンドウを 56 日として定義します。 つまり、モデルでは、過去 2 四半期に発生した、定義済みのコンバージョンイベントをすべて考慮し、関連するコンバージョンイベントの前 56 日以内に発生したすべてのタッチポイントを探します。

**数式**:

必要なデータの最小長= トレーニングウィンドウ + ルックバックウィンドウ

>[!TIP]
>
> デフォルト設定のアプリケーションに必要なデータの最小長は、2 四半期（180 日）+ 56 日= 236 日です。

例：

- 過去 90 日間（3 か月）に発生したコンバージョンイベントを属性化し、コンバージョンイベント前の 4 週間以内に発生したすべてのタッチポイントを追跡する必要があります。 入力データ期間は、過去 90 日+ 28 日（4 週間）に及ぶ必要があります。 トレーニングウィンドウは 90 日で、ルックバックウィンドウは合計 118 日で 28 日です。

## アトリビューション AI 出力データ

アトリビューション AI は、次の情報を出力します。

- [生の詳細なスコア](#raw-granular-scores)
- [集計スコア](#aggregated-attribution-scores)

**出力スキーマの例：**

![](./images/input-output/schema_output.gif)

### 生の詳細なスコア {#raw-granular-scores}

アトリビューション AI は、アトリビューションスコアをできる限り詳細なレベルで出力するので、任意のスコア列でスコアをスライスし、サイコロ分けできます。 これらのスコアを UI に表示するには、[ 生のスコアパスの表示 ](#raw-score-path) に関する節を参照してください。 API を使用してスコアをダウンロードするには、[ アトリビューション AI でのスコアのダウンロード ](./download-scores.md) ドキュメントにアクセスしてください。

>[!NOTE]
>
> 次のいずれかが当てはまる場合にのみ、スコア出力データセットの入力データセットから目的のレポート列を確認できます。
> - レポート列は、タッチポイントまたはコンバージョン定義設定の一部として、設定ページに含まれています。
> - レポート列は、追加のスコアデータセット列に含まれています。

次の表に、生のスコアのサンプル出力のスキーマフィールドの概要を示します。

| 列名（データタイプ） | 無効可能 | 説明 |
| --- | --- | --- |
| タイムスタンプ （日時） | False | コンバージョンイベントまたは観測が発生した時間。<br> **例：** 2020-06-09T00:01:51.000Z |
| identityMap （マップ） | True | cee XDM 形式に類似したユーザーの identityMap。 |
| eventType （String） | True | この時系列レコードの主なイベントタイプ。<br> **例：** &quot;Order&quot;, &quot;Purchase&quot;, &quot;Visit&quot; |
| eventMergeId （String） | True | 本質的に同じイベントである、または結合する必要がある複数の [!DNL Experience Events] を相互に関連付けたり結合したりするための ID。 これは、取り込み前に、データプロデューサーによって入力されることを目的としています。<br> **例：** 575525617716-0-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _id （文字列） | False | 時系列イベントの一意の ID。<br> **例：** 4461-edc2ed37-1aab-4750-a820-1c2b3844b8c4 |
| _tenantId （オブジェクト） | False | テナント ID に対応する最上位のオブジェクトコンテナ。<br> **例：** _atsdsnrmmsv2 |
| your_schema_name （オブジェクト） | False | コンバージョンイベントに関連するすべてのタッチポイントイベントとそのメタデータを含む行をスコアに付けます。<br> **例：** アトリビューション AI スコア – モデル名__2020 |
| セグメント化（文字列） | True | コンバージョンセグメント（地理セグメントなど） – モデルの構築対象。 セグメントがない場合、segment は conversionName と同じです。<br> **例：** ORDER_US |
| conversionName （String） | True | セットアップ時に設定されたコンバージョンの名前。<br> **例：** 注文、リード、訪問 |
| conversion （オブジェクト） | False | 変換メタデータ列。 |
| dataSource （String） | True | データソースのグローバルに一意の ID。<br> **例：** Adobe Analytics |
| eventSource （String） | True | 実際のイベントが発生したソース。<br> **例：** Adobe.com |
| eventType （String） | True | この時系列レコードの主なイベントタイプ。<br> **例：** 順序 |
| geo （String） | True | コンバージョンが配信された地理的 `placeContext.geo.countryCode` 場所。<br> **例：** US |
| priceTotal （Double） | True | コンバージョン <br> を通じて得られた収益 **例：** 99.9 |
| product （String） | True | 製品自体の XDM 識別子。<br> **例：** RX 1080 ti |
| productType （String） | True | この商品表示でユーザーに提示される商品の表示名。<br> **例：** Gpu |
| 数量（整数） | True | コンバージョン中に購入された数量。<br> **例：** 1 1080 ti |
| receivedTimestamp （DateTime） | True | コンバージョンのタイムスタンプを受信しました。<br> **例：** 2020-06-09T00:01:51.000Z |
| skuId （String） | True | 最小在庫管理単位（SKU）。ベンダーによって定義された商品の一意の ID。<br> **例：** MJ-03-XS-Black |
| タイムスタンプ （日時） | True | コンバージョンのタイムスタンプ。<br> **例：** 2020-06-09T00:01:51.000Z |
| passThrough （オブジェクト） | True | モデルの設定時にユーザーによって指定された、追加のスコアデータセットの列。 |
| commerce_order_purchaseCity （String） | True | 追加のスコアデータセット列。<br> **例：** city: サンノゼ |
| customerProfile （オブジェクト） | False | モデルの構築に使用されるユーザーの ID の詳細。 |
| identity （オブジェクト） | False | `id` や `namespace` など、モデルの構築に使用されるユーザーの詳細が含まれます。 |
| id （文字列） | True | Cookie ID、Adobe Analytics ID （AAID）、Experience Cloud ID （ECID、MCID または訪問者 ID とも呼ばれます）など、ユーザーの ID <br> **例：** 17348762725408656344688320891369597404 |
| 名前空間（文字列） | True | パスを構築し、それによってモデルを構築するために使用される ID 名前空間。<br> **例：** aaid |
| touchpointsDetail （オブジェクト配列） | True | 並べ替え順のコンバージョンにつながるタッチポイント詳細のリスト | タッチポイント発生またはタイムスタンプ。 |
| touchpointName （String） | True | 設定時に設定されたタッチポイントの名前。<br> **例：** PAID_SEARCH_CLICK |
| scores （オブジェクト） | True | このコンバージョンに対するタッチポイントの貢献度（スコア）。 このオブジェクト内で生成されるスコアについて詳しくは、[ 集計アトリビューションスコア ](#aggregated-attribution-scores) の節を参照してください。 |
| touchPoint （オブジェクト） | True | タッチポイントメタデータ。 このオブジェクト内で生成されるスコアについて詳しくは、[ 集計スコア ](#aggregated-scores) の節を参照してください。 |

### 生のスコアパスの表示（UI） {#raw-score-path}

生のスコアへのパスを UI に表示できます。 Experience Platform UI で「**[!UICONTROL スキーマ]**」を選択してから、「参照 **[!UICONTROL タブ内でアトリビューション AI スコアスキーマを検索して選択し]** す。

![ スキーマを選択 ](./images/input-output/schemas_browse.png)

次に、UI の **[!UICONTROL 構造]** ウィンドウでフィールドを選択すると、「**[!UICONTROL フィールドプロパティ]**」タブが開きます。 **[!UICONTROL フィールドプロパティ]** 内には、生のスコアにマッピングされるパスフィールドがあります。

![ スキーマを選択 ](./images/input-output/field_properties.png)

### 集計アトリビューションスコア {#aggregated-attribution-scores}

集計スコアは、日付範囲が 30 日未満の場合、Experience Platform UI から CSV 形式でダウンロードできます。

Attribution AI は、アトリビューションスコアの 2 つのカテゴリ（アルゴリズムスコアとルールベーススコア）をサポートしています。

Attribution AI では、増分スコアと影響スコアの 2 種類のアルゴリズムスコアを生成します。影響スコアは、各マーケティングタッチポイントがコンバージョンに寄与している割合です。増分スコアは、マーケティングタッチポイントで直接引き起こされたわずかな影響の量です。増分スコアと影響スコアの主な違いは、増分スコアがベースライン効果を考慮に入れていることです。コンバージョンが、先行するマーケティングタッチポイントによってのみ生じるとは考えられません。

以下は、Adobe Experience Platform UI からのアトリビューション AI スキーマ出力の例です。

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

次の表に、アトリビューションスコアを生のスコアにマッピングします。 生のスコアをダウンロードする場合は、[ アトリビューション AI でのスコアのダウンロード ](./download-scores.md) ドキュメントを参照してください。

| アトリビューションスコア | 生のスコア参照列 |
| --- | --- |
| 影響（アルゴリズム） | _tenantID.your_schema_name.element.touchpoint.algorithmicInfected |
| 増分（アルゴリズム） | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.algorithmicInfected |
| ファーストタッチ | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.firstTouch |
| ラストタッチ | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.lastTouch |
| 線形 | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.linear |
| U 字形 | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.uShape |
| タイムディケイ | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.decayUnits |

### 集計スコア {#aggregated-scores}

集計スコアは、日付範囲が 30 日未満の場合、Experience Platform UI から CSV 形式でダウンロードできます。 これらの各集計列の詳細については、次の表を参照してください。

| 列名 | 制約 | 無効可能 | 説明 |
| --- | --- | --- | --- |
| customerevents_date （DateTime） | ユーザー定義および固定フォーマット | False | 顧客イベント日付（YYYY-MM-DD 形式）。<br> **例**:2016-05-02 |
| mediatouchpoints_date （DateTime） | ユーザー定義および固定フォーマット | True | YYYY-MM-DD 形式 <br> のメディアタッチポイント日 **例**:2017-04-21 |
| セグメント（文字列） | 計算日時 | False | コンバージョンセグメント （モデルの構築対象となる地理セグメント化など）。 セグメントがない場合、セグメントは conversion_scope と同じです。<br> **例**: ORDER_AMER |
| conversion_scope （String） | ユーザー定義 | False | ユーザーが設定したコンバージョンの名前。<br> **例**:ORDER |
| touchpoint_scope （String） | ユーザー定義 | True | ユーザー <br> で設定されたタッチポイントの名前 **例**:PAID_SEARCH_CLICK |
| product （String） | ユーザー定義 | True | 商品の XDM 識別子。<br> **例**: CC |
| product_type （String） | ユーザー定義 | True | この商品表示でユーザーに提示される商品の表示名。<br> **例**:gpu、ラップトップ |
| geo （String） | ユーザー定義 | True | コンバージョンが配信された地理的な場所（placeContext.geo.countryCode） <br> **例**:US |
| event_type （String） | ユーザー定義 | True | この時系列レコード <br> の主なイベントタイプ **例**：有料コンバージョン |
| media_type （String） | 列挙 | False | メディアタイプがペイド、オウンドまたはアーンドのいずれかを表します。<br> **例**：有料、所有 |
| チャネル （文字列） | 列挙 | False | XDM で類似のプロパティを持つチャネルの大まかな分類を提供するために使用される `channel._type` プロパティ [!DNL Consumer Experience Event] す。<br> **例**：検索 |
| アクション（String） | 列挙 | False | `mediaAction` プロパティは、エクスペリエンスイベントメディアアクションのタイプを提供するために使用されます。<br> **例**：クリックする |
| campaign_group （String） | ユーザー定義 | True | 複数のキャンペーンがグループ化されたキャンペーングループの名前（「50%_DISCOUNT」など）。<br> **例**：商用 |
| campaign_name （String） | ユーザー定義 | True | マーケティングキャンペーンを識別するために使用されるキャンペーンの名前（「50%_DISCOUNT_USA」や「50%_DISCOUNT_ASIA」など）。<br> **例**：感謝祭の販売 |

**生のスコア参照（集計）**

次の表は、集計されたスコアを生のスコアにマッピングします。 生のスコアをダウンロードする場合は、[ アトリビューション AI でのスコアのダウンロード ](./download-scores.md) ドキュメントを参照してください。 UI 内から生のスコアパスを表示するには、このドキュメント内の [ 生のスコアパスの表示 ](#raw-score-path) の節を参照してください。

| 列名 | 生のスコア参照列 |
| --- | --- |
| customerevents_date | タイムスタンプ |
| mediatouchpoints_date | _tenantID.your_schema_name.touchpointsDetail.element.touchpoint.timestamp |
| セグメント | _tenantID.your_schema_name.segmentation |
| conversion_scope | _tenantID.your_schema_name.conversion.conversionName |
| touchpoint_scope | _tenantID.your_schema_name.タッチポイントの詳細.element.タッチポイント名 |
| 製品 | _tenantID.your_schema_name.conversion.product |
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
> - アトリビューション AI では、追加のトレーニングとスコアリングに、更新されたデータのみを使用します。 同様に、データの削除をリクエストすると、顧客 AI は削除されたデータの使用を控えます。
> - Attribution AI ではExperience Platform データセットを活用します。 ブランドが受け取る可能性のある消費者の権利リクエストをサポートするには、Experience Platform Privacy Serviceを使用して、アクセスおよび削除に対する消費者のリクエストを送信し、データレイク、ID サービス、リアルタイム顧客プロファイルをまたいでデータを削除する必要があります。
> - モデルの入出力に使用するすべてのデータセットは、Experience Platformのガイドラインに従います。 Experience Platform データ暗号化は、保存中および送信中のデータに適用されます。 [ データ暗号化 ](../../../help/landing/governance-privacy-security/encryption.md) について詳しくは、ドキュメントを参照してください

## 次の手順 {#next-steps}

データを準備し、すべての資格情報とスキーマを設定したら、[ アトリビューション AI ユーザーガイド ](./user-guide.md) に従って開始します。 このガイドでは、アトリビューション AI のインスタンスを作成する手順について説明します。
