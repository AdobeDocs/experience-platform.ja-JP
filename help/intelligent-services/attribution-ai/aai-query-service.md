---
keywords: インサイト；attribution ai;attribution ai インサイト；AAI クエリサービス；アトリビューションクエリ；アトリビューションスコア
feature: Attribution AI
title: クエリサービスを使用したアトリビューションスコアの分析
topic-legacy: Attribution AI queries
description: Adobe Experience Platformクエリサービスを使用してAttribution AIスコアを分析する方法を説明します。
exl-id: 35d7f6f2-a118-4093-8dbc-cb020ec35e90
source-git-commit: c3320f040383980448135371ad9fae583cfca344
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 0%

---

# クエリサービスを使用したアトリビューションスコアの分析

データの各行はコンバージョンを表し、コンバージョン内の関連するタッチポイントの情報は、`touchpointsDetail` 列の下に構造体の配列として保存されます。

| タッチポイント情報 | 列 |
| ---------------------- | ------ |
| タッチポイント名 | `touchpointsDetail. touchpointName` |
| タッチポイントチャネル | `touchpointsDetail.touchPoint.mediaChannel` |
| タッチポイントAttribution AIのアルゴリズムスコア | <li>`touchpointsDetail.scores.algorithmicSourced`</li> <li> `touchpointsDetail.scores.algorithmicInfluenced` </li> |

## データパスの検索

Adobe Experience Platform UI で、左側のナビゲーションで「**[!UICONTROL データセット]**」を選択します。 **[!UICONTROL Datasets]** ページが表示されます。 次に、「**[!UICONTROL 参照]**」タブを選択し、Attribution AIスコアの出力データセットを見つけます。

![インスタンスへのアクセス](./images/aai-query/datasets_browse.png)

出力データセットを選択します。 「データセットアクティビティ」ページが表示されます。

![データセットアクティビティページ](./images/aai-query/select_preview.png)

データセットアクティビティページで、右上隅の「**[!UICONTROL データセットのプレビュー]**」を選択してデータをプレビューし、期待どおりに取り込まれたことを確認します。

![プレビューデータセット](./images/aai-query/preview_dataset.JPG)

データをプレビューした後、右側のパネルでスキーマを選択します。 スキーマ名と説明がポップオーバーに表示されます。 スキーマ名のハイパーリンクを選択して、スコアリングスキーマにリダイレクトします。

![スキーマの選択](./images/aai-query/select_schema.png)

スコア付けスキーマを使用して、値を選択または検索できます。 選択すると、**[!UICONTROL フィールドのプロパティ]** サイドレールが開き、クエリの作成に使用するパスをコピーできます。

![パスをコピー](./images/aai-query/copy_path.png)

## クエリサービスへのアクセス

Platform UI 内からクエリサービスにアクセスするには、まず左のナビゲーションで「**[!UICONTROL クエリ]**」を選択し、次に「**[!UICONTROL 参照]**」タブを選択します。 以前に保存したクエリのリストが読み込まれます。

![クエリサービスの参照](./images/aai-query/query_tab.png)

次に、右上隅の「**[!UICONTROL クエリを作成]**」を選択します。 クエリエディターが読み込まれます。 クエリエディターを使用して、スコアリングデータを使用してクエリの作成を開始できます。

![クエリエディター](./images/aai-query/query_example.png)

クエリエディターの詳細については、[ クエリエディターのユーザーガイド ](../../query-service/ui/user-guide.md) を参照してください。

## アトリビューションスコア分析用のクエリテンプレート

以下のクエリは、様々なスコア分析シナリオのテンプレートとして使用できます。 `_tenantId` と `your_score_output_dataset` を、スコアリング出力スキーマにある適切な値に置き換える必要があります。

>[!NOTE]
>
> データの取り込み方法に応じて、`timestamp` など以下で使用される値は異なる形式になる場合があります。

### 検証の例

**（コンバージョンウィンドウ内の）コンバージョンイベント別のコンバージョンの合計数**

```sql
    SELECT conversionName,
           SUM(scores.firstTouch) as total_conversions,
           SUM(scores.algorithmicSourced) as total_attributed_conversions
    FROM
        (SELECT
                _tenantId.your_score_output_dataset.conversionName
                    as conversionName,
                inline(_tenantId.your_score_output_dataset.touchpointsDetail),
                timestamp as conversion_timestamp
         FROM
                your_score_output_dataset
        )
    WHERE
        conversion_timestamp >= '2020-07-16'
      AND
        conversion_timestamp <  '2020-10-14'
    GROUP BY
        conversionName
```

**（コンバージョンウィンドウ内の）コンバージョンのみのイベントの合計数**

```sql
    SELECT
        _tenantId.your_score_output_dataset.conversionName as conversionName,
        COUNT(1) as convOnly_cnt
    FROM
        your_score_output_dataset
    WHERE
        _tenantId.your_score_output_dataset.touchpointsDetail.touchpointName[0] IS NULL AND
        timestamp >= '2020-07-16' AND
        timestamp <  '2020-10-14'
    GROUP BY
        conversionName
```

### トレンド分析の例

**1 日あたりのコンバージョン数**

```sql
    SELECT conversionName,
           DATE(conversion_timestamp) as conversion_date,
           SUM(scores.firstTouch) as convertion_cnt
    FROM
        (SELECT
                _tenantId.your_score_output_dataset.conversionName as conversionName,
                inline(_tenantId.your_score_output_dataset.touchpointsDetail),
                timestamp as conversion_timestamp
         FROM
                your_score_output_dataset
        )
    GROUP BY
        conversionName, DATE(conversion_timestamp)
    ORDER BY
        conversionName, DATE(conversion_timestamp)
    LIMIT 20
```

### 分布分析の例

**（コンバージョンウィンドウ内での）定義されたタイプ別のコンバージョンパスのタッチポイントの数**

```sql
    SELECT conversionName,
           touchpointName,
           COUNT(1) as tp_count
    FROM
        (SELECT
                _tenantId.your_score_output_dataset.conversionName as conversionName,
                inline(_tenantId.your_score_output_dataset.touchpointsDetail),
                timestamp as conversion_timestamp
         FROM
                your_score_output_dataset
        )
    WHERE
        conversion_timestamp >= '2020-07-16' AND
        conversion_timestamp < '2020-10-14' AND
        touchpointName IS NOT NULL
    GROUP BY
        conversionName, touchpointName
    ORDER BY
        conversionName, tp_count DESC
```

### Insight の生成例

**タッチポイントおよびコンバージョン日別の増分単位の分類（コンバージョンウィンドウ内）**

```sql
    SELECT conversionName,
           touchpointName,
           DATE(conversion_timestamp) as conversion_date,
           SUM(scores.algorithmicSourced) as incremental_units
    FROM
        (SELECT
                _tenantId.your_score_output_dataset.conversionName as conversionName,
                inline(_tenantId.your_score_output_dataset.touchpointsDetail),
                timestamp as conversion_timestamp
         FROM
                your_score_output_dataset
        )
    WHERE
        conversion_timestamp >= '2020-07-16' AND
        conversion_timestamp < '2020-10-14'  AND
        touchpointName IS NOT NULL
    GROUP BY
        conversionName, touchpointName, DATE(conversion_timestamp)
    ORDER BY
        conversionName, touchpointName, DATE(conversion_timestamp)
```

**タッチポイントおよびタッチポイントの日付別の増分単位の分類（コンバージョンウィンドウ内）**

```sql
    SELECT conversionName,
           touchpointName,
           DATE(touchpoint.timestamp) as touchpoint_date,
           SUM(scores.algorithmicSourced) as incremental_units
    FROM
        (SELECT
                _tenantId.your_score_output_dataset.conversionName as conversionName,
                inline(_tenantId.your_score_output_dataset.touchpointsDetail),
                timestamp as conversion_timestamp
         FROM
                your_score_output_dataset
        )
    WHERE
        conversion_timestamp >= '2020-07-16' AND
        conversion_timestamp < '2020-10-14'  AND
        touchpointName IS NOT NULL
    GROUP BY
        conversionName, touchpointName, DATE(touchpoint.timestamp)
    ORDER BY
        conversionName, touchpointName, DATE(touchpoint.timestamp)
    LIMIT 20
```

**（コンバージョンウィンドウ内の）すべてのスコアリングモデルに対する特定のタイプのタッチポイントの集計スコア**

```sql
    SELECT
           conversionName,
           touchpointName,
           SUM(scores.algorithmicSourced) as total_incremental_units,
           SUM(scores.algorithmicInfluenced) as total_influenced_units,
           SUM(scores.uShape) as total_uShape_units,
           SUM(scores.decayUnits) as total_decay_units,
           SUM(scores.linear) as total_linear_units,
           SUM(scores.lastTouch) as total_lastTouch_units,
           SUM(scores.firstTouch) as total_firstTouch_units
    FROM
        (SELECT
                _tenantId.your_score_output_dataset.conversionName as conversionName,
                inline(_tenantId.your_score_output_dataset.touchpointsDetail),
                timestamp as conversion_timestamp
         FROM
                your_score_output_dataset
        )
    WHERE
        conversion_timestamp >= '2020-07-16' AND
        conversion_timestamp < '2020-10-14'  AND
        touchpointName = 'display'
    GROUP BY
        conversionName, touchpointName
    ORDER BY
        conversionName, touchpointName
```

**詳細 — パス長解析**

各コンバージョンイベントタイプのパスの長さの配分を取得します。

```sql
    WITH agg_path AS (
          SELECT
            _tenantId.your_score_output_dataset.conversionName as conversionName,
            sum(size(_tenantId.your_score_output_dataset.touchpointsDetail)) as path_length
          FROM
            your_score_output_dataset
          WHERE
            _tenantId.your_score_output_dataset.touchpointsDetail.touchpointName[0] IS NOT NULL AND
            timestamp >= '2020-07-16' AND
            timestamp <  '2020-10-14'
          GROUP BY
            _tenantId.your_score_output_dataset.conversionName,
            eventMergeId
    )
    SELECT
        conversionName,
        path_length,
        count(1) as conversionPath_count
    FROM
        agg_path
    GROUP BY
        conversionName, path_length
    ORDER BY
        conversionName, path_length
```

**高度 — コンバージョンパス分析でのタッチポイントのユニーク数**

各コンバージョンイベントタイプのコンバージョンパスの個別タッチポイント数の配分を取得します。

```sql
    WITH agg_path AS (
      SELECT
        _tenantId.your_score_output_dataset.conversionName as conversionName,
        size(array_distinct(flatten(collect_list(_tenantId.your_score_output_dataset.touchpointsDetail.touchpointName)))) as num_dist_tp
      FROM
        your_score_output_dataset
      WHERE
        _tenantId.your_score_output_dataset.touchpointsDetail.touchpointName[0] IS NOT NULL AND
        timestamp >= '2020-07-16' AND
        timestamp <  '2020-10-14'
      GROUP BY
        _tenantId.your_score_output_dataset.conversionName,
        eventMergeId
    )
    SELECT
        conversionName,
        num_dist_tp,
        count(1) as conversionPath_count
    FROM
     agg_path
    GROUP BY
        conversionName, num_dist_tp
    ORDER BY
        conversionName, num_dist_tp
```

### スキーマの統合と展開の例

このクエリは、構造体列を複数の単一の列に統合し、配列を複数の行に分解します。 これは、アトリビューションスコアを CSV 形式に変換するのに役立ちます。 このクエリの出力には、1 つのコンバージョンと、各行のそのコンバージョンに対応するタッチポイントの 1 つが含まれます。

>[!TIP]
>
> この例では、`_tenantId` と `your_score_output_dataset` に加えて `{COLUMN_NAME}` を置き換える必要があります。 `COLUMN_NAME` 変数は、オプションのパススルー列名（レポート列）の値を取得できます。この値は、Attribution AIインスタンスの設定時に追加されました。 スコアリング出力スキーマを確認して、このクエリの完了に必要な `{COLUMN_NAME}` 値を見つけてください。

```sql
SELECT 
  segmentation,
  conversionName,
  scoreCreatedTime,
  aaid, _id, eventMergeId,
  conversion.eventType as conversion_eventType,
  conversion.quantity as conversion_quantity,
  conversion.eventSource as conversion_eventSource,
  conversion.priceTotal as conversion_priceTotal,
  conversion.timestamp as conversion_timestamp,
  conversion.geo as conversion_geo,
  conversion.receivedTimestamp as conversion_receivedTimestamp,
  conversion.dataSource as conversion_dataSource,
  conversion.productType as conversion_productType,
  conversion.passThrough.{COLUMN_NAME} as conversion_passThru_column,
  conversion.skuId as conversion_skuId,
  conversion.product as conversion_product,
  touchpointName,
  touchPoint.campaignGroup as tp_campaignGroup, 
  touchPoint.mediaType as tp_mediaType,
  touchPoint.campaignTag as tp_campaignTag,
  touchPoint.timestamp as tp_timestamp,
  touchPoint.geo as tp_geo,
  touchPoint.receivedTimestamp as tp_receivedTimestamp,
  touchPoint.passThrough.{COLUMN_NAME} as tp_passThru_column,
  touchPoint.campaignName as tp_campaignName,
  touchPoint.mediaAction as tp_mediaAction,
  touchPoint.mediaChannel as tp_mediaChannel,
  touchPoint.eventid as tp_eventid,
  scores.*
FROM (
  SELECT
        _tenantId.your_score_output_dataset.segmentation,
        _tenantId.your_score_output_dataset.conversionName,
        _tenantId.your_score_output_dataset.scoreCreatedTime,
        _tenantId.your_score_output_dataset.conversion,
        _id,
        eventMergeId,
        map_values(identityMap)[0][0].id as aaid,
        inline(_tenantId.your_score_output_dataset.touchpointsDetail)
  FROM
        your_score_output_dataset
)
```
