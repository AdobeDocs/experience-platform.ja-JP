---
keywords: インサイト;Attribution AI;アトリビューション AI インサイト;AAI クエリサービス;アトリビューションクエリ;アトリビューションスコア
feature: Attribution AI
title: クエリサービスを使用したアトリビューションスコアの分析
description: Adobe Experience Platform クエリサービスを使用してアトリビューション AI スコアを分析する方法について説明します。
exl-id: 35d7f6f2-a118-4093-8dbc-cb020ec35e90
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 2%

---

# クエリサービスを使用したアトリビューションスコアの分析

データの各行はコンバージョンを表し、関連するタッチポイントの情報が `touchpointsDetail` 列の下に構造体の配列として格納されます。

| タッチポイント情報 | 列 |
| ---------------------- | ------ |
| タッチポイント名 | `touchpointsDetail. touchpointName` |
| タッチポイントチャネル | `touchpointsDetail.touchPoint.mediaChannel` |
| タッチポイントアトリビューション AI アルゴリズムスコア | <li>`touchpointsDetail.scores.algorithmicSourced`</li> <li> `touchpointsDetail.scores.algorithmicInfluenced` </li> |

## データパスの検索

Adobe Experience Platform UI の左側のナビゲーションで **[!UICONTROL データセット]** を選択します。 **[!UICONTROL データセット]** ページが表示されます。 次に、「**[!UICONTROL 参照]**」タブを選択し、アトリビューション AI スコアの出力データセットを見つけます。

![&#x200B; モデルへのアクセス &#x200B;](./images/aai-query/datasets_browse.png)

出力データセットを選択します。 データセットアクティビティ ページが表示されます。

![&#x200B; データセットアクティビティページ &#x200B;](./images/aai-query/select_preview.png)

データセットアクティビティページで、右上隅の **[!UICONTROL データセットをプレビュー]** を選択し、データが期待どおりに取り込まれていることを確認します。

![&#x200B; データセットをプレビュー &#x200B;](./images/aai-query/preview_dataset.JPG)

データのプレビュー後、右側のパネルでスキーマを選択します。 ポップオーバーが開き、スキーマ名と説明が表示されます。 スコアリングスキーマにリダイレクトするには、スキーマ名のハイパーリンクを選択します。

![&#x200B; スキーマを選択 &#x200B;](./images/aai-query/select_schema.png)

スコアリングスキーマを使用すると、値を選択または検索できます。 選択すると、**[!UICONTROL フィールドプロパティ]** サイドパネルが開き、クエリの作成に使用するパスをコピーできます。

![&#x200B; パスをコピー &#x200B;](./images/aai-query/copy_path.png)

## クエリサービスにアクセス

Experience Platform UI 内からクエリサービスにアクセスするには、左側のナビゲーションで **[!UICONTROL クエリ]** を選択してから、「**[!UICONTROL 参照]** タブを選択します。 以前に保存したクエリのリストが読み込まれます。

![&#x200B; クエリサービスの参照 &#x200B;](./images/aai-query/query_tab.png)

次に、右上隅の「**[!UICONTROL クエリを作成]**」を選択します。 クエリエディターが読み込まれます。 クエリエディターを使用すると、スコアリングデータを使用したクエリの作成を開始できます。

![&#x200B; クエリエディター &#x200B;](./images/aai-query/query_example.png)

クエリエディターについて詳しくは、[&#x200B; クエリエディターユーザーガイド &#x200B;](../../query-service/ui/user-guide.md) を参照してください。

## アトリビューションスコア分析用のクエリテンプレート

以下のクエリは、様々なスコア分析シナリオのテンプレートとして使用できます。 `_tenantId` と `your_score_output_dataset` を、スコアリング出力スキーマにある適切な値に置き換える必要があります。

>[!NOTE]
>
> データの取り込み方法に応じて、次で使用する値（`timestamp` など）が異なる形式になる場合があります。

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

**（コンバージョンウィンドウ内の）コンバージョン専用イベントの合計数**

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

### 配分分析の例

**定義済みのタイプ別（コンバージョンウィンドウ内）のコンバージョンパス上のタッチポイントの量**

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

### Insightの生成例

**タッチポイントおよびコンバージョン日（コンバージョンウィンドウ内）による増分単位の分類**

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

**タッチポイントおよびタッチポイント日（コンバージョンウィンドウ内）による増分単位の分類**

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

**すべてのスコアリングモデルの特定タイプのタッチポイントの集計スコア（コンバージョンウィンドウ内）**

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

**詳細 – パスの長さの分析**

各コンバージョンイベントタイプのパスの長さの分布を取得します。

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

**詳細 – コンバージョンパス分析に関する個別のタッチポイント数**

各コンバージョンイベントタイプについて、コンバージョンパス上の個別タッチポイント数の分布を取得します。

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

このクエリでは、構造体列を複数の単一列に統合し、配列を複数の行に分解します。 これは、アトリビューションスコアを CSV 形式に変換する際に役立ちます。 このクエリの出力には、各行に 1 つのコンバージョンと、そのコンバージョンに対応する 1 つのタッチポイントがあります。

>[!TIP]
>
> この例では、`_tenantId` と `your_score_output_dataset` に加えて `{COLUMN_NAME}` も置き換える必要があります。 `COLUMN_NAME` 変数は、アトリビューション AI モデルの設定時に追加されたオプションのパススルー列名（レポート列）の値を取ることができます。 スコアリング出力スキーマを確認して、このクエリを完了するために必要な `{COLUMN_NAME}` の値を見つけてください。

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
