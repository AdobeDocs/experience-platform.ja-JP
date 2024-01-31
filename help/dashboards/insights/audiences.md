---
title: オーディエンスインサイト
description: オーディエンスのインサイトを強化する SQL を見つけ、これらのクエリを使用してカスタムインサイトを生成し、Adobe Experience Platformからオーディエンスデータをさらに調査します。
source-git-commit: ee9ef2cf777c72fbd19cfccd80a37ea66591216d
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 1%

---

# オーディエンスインサイト

データモデルの分析から得られたインサイトにより、Adobe Real-time Customer Data Platformのデータにアクセスしやすく、理解しやすく、意思決定に対する影響が大きくなります。

オーディエンスの洞察を強化する SQL にアクセスして理解し、独自のインサイトを生成して、オーディエンスを構成する ID とプロファイルをさらに調査します。 既存のReal-Time CDPデータモデル SQL をインスピレーションとして使用し、生データを新しい実用的なインサイトに変換して、独自のビジネスニーズに応えるクエリを作成します。

<!-- This link will go in during the January release.
See the [View SQL documentation]() for more information on how to adapt your insights' SQL directly through the PLatform UI.  -->

以下のインサイトを、 [オーディエンスダッシュボード](../guides/audiences.md) またはカスタム [ユーザー定義ダッシュボード](../user-defined-dashboards.md). 詳しくは、 [カスタマイズの概要](../customize/overview.md) を参照してください。 [新しいウィジェットを作成および編集します](../customize/custom-widgets.md) ウィジェットライブラリで、および [ユーザー定義ダッシュボード](../user-defined-dashboards.md#create-widget).

以下のインサイトを、 [オーディエンスダッシュボード](../guides/audiences.md) またはカスタムダッシュボード。

## オーディエンスの重複レポート {#audience-overlap-report}

このインサイトによって回答された質問：

- 特定のフィルター適用済みオーディエンスの上位 50 件のオーディエンスは何ですか？
- フィルターを適用した特定のオーディエンスの 50 最も重複しないオーディエンスは何ですか？
- フィルタリングされたオーディエンスごとに重複パターンはどのように変化しますか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT source_segment_name,
        source_segment_id,
        overlap_segment_name,
        overlap_segment_id,
        max(source_segment_audience_count) source_segment_audience_count,
        max(overlap_segment_audience_count) overlap_segment_audience_count,
        max(overlap_audience_count) overlap_audience_count,
        CASE
            WHEN (max(source_segment_audience_count) + max(overlap_segment_audience_count) - max(overlap_audience_count)) > 0 THEN (cast(max(overlap_audience_count) AS DECIMAL(18, 2)) / cast((max(source_segment_audience_count) + max(overlap_segment_audience_count) - max(overlap_audience_count)) AS DECIMAL(18, 2))) * 100::DECIMAL(9, 2)
            ELSE 100.00
        END overlapping_percentage
  FROM
    (SELECT adwh_fact_profile_overlap_of_segments.Segment1 source_segment_id,
            adwh_fact_profile_overlap_of_segments.Segment2 overlap_segment_id,
            Sum(count_of_overlap) overlap_audience_count
    FROM qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments
    WHERE qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.date_key = '2024-01-10'
    GROUP BY qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.Segment2 ,
              qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.Segment1) a
  INNER JOIN
    (SELECT sum(count_of_profiles) source_segment_audience_count,
            adwh_dim_segments.segment_name source_segment_name,
            adwh_fact_profile_by_segment_trendlines.merge_policy_id,
            adwh_fact_profile_by_segment_trendlines.segment_Id segment1
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
    JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_dim_segments.segment_id = qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_Id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
    GROUP BY qsaccel.profile_agg.adwh_dim_segments.segment_name,
              qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id,
              qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_id) b ON a.source_segment_id = b.segment1
  INNER JOIN
    (SELECT sum(count_of_profiles) overlap_segment_audience_count,
            adwh_dim_segments.segment_name overlap_segment_name,
            adwh_fact_profile_by_segment_trendlines.merge_policy_id,
            adwh_fact_profile_by_segment_trendlines.segment_Id segment2
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
    JOIN qsaccel.profile_agg.adwh_dim_segments ON adwh_dim_segments.segment_id = adwh_fact_profile_by_segment_trendlines.segment_Id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
    GROUP BY qsaccel.profile_agg.adwh_dim_segments.segment_name,
              qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id,
              qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_id) c ON a.overlap_segment_id = c.segment2
  GROUP BY source_segment_name,
          source_segment_id,
          overlap_segment_name,
          overlap_segment_id
  ORDER BY overlapping_percentage DESC
  LIMIT 5;
```

+++

詳しくは、 [オーディエンス重複レポートウィジェットドキュメント](../guides/audiences.md#audience-overlap-report) を参照してください。

## オーディエンスの重複 {#audience-overlap}

このインサイトによって回答された質問：

- 両方のオーディエンスに共通するプロファイルはどれですか。
- 重複はエンゲージメント率やコンバージョン率にどのような影響を与えますか？
- 重複するセグメントに合わせてマーケティング戦略をカスタマイズするにはどうすればよいですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT Sum(overlap_col1) overlap_col1,
        Sum(overlap_col2) overlap_col2,
        Sum(overlap_count) Overlap_count
  FROM
    (SELECT 0 overlap_col1,
            0 overlap_col2,
            sum(count_of_overlap)Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments
    WHERE qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.merge_policy_id = 1133248113
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.date_key = '2024-01-10'
      AND ((qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment1=1870062812
            AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment2=2080256533)
            OR (qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment1=2080256533
                AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment2=1870062812))
    UNION ALL SELECT sum(count_of_profiles) overlap_col1,
                      0 overlap_col2,
                      0 overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
    LEFT JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_Id = qsaccel.profile_agg.adwh_dim_segments.segment_Id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 1133248113
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_dim_segments.segment_Id = 1870062812
    UNION ALL SELECT 0 overlap_col1,
                      sum(count_of_profiles) overlap_col2,
                      0 Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
    JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_Id = qsaccel.profile_agg.adwh_dim_segments.segment_Id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 1133248113
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_dim_segments.segment_Id = 2080256533 ) a;
```

+++

詳しくは、 [オーディエンス重複ウィジェットのドキュメント](../guides/audiences.md#audience-overlap) を参照してください。

## オーディエンスサイズの変更の傾向 {#audience-size-change-trend}

このインサイトによって回答された質問：

- 過去 30 日間、90 日間、12 ヶ月間に、オーディエンスサイズに大きな急増や急減はありますか？
- 特定の日間でオーディエンスのサイズはどのように変化しますか？
- 過去 12 か月間に、スパイクや下降のパターンが何らかの異常値や繰り返し検出されたか。

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT date_key,
      Profiles_added
  FROM
    (SELECT rn_num,
            date_key,
            (count_of_profiles-lag(count_of_profiles, 1, 0) over(
                                                                ORDER BY date_key))Profiles_added
    FROM
      (SELECT date_key,
              sum(x.count_of_profiles)count_of_profiles,
              row_number() OVER (
                                  ORDER BY date_key) rn_num
        FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines x
        INNER JOIN
          (SELECT MAX(process_date) last_process_date,
                  merge_policy_id
          FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
          WHERE process_name = 'FACT_TABLES_PROCESSING'
            AND process_status = 'SUCCESSFUL'
          GROUP BY merge_policy_id) y ON x.merge_policy_id = y.merge_policy_id
        WHERE segment_id = 1333234510
          AND x.date_key >= dateadd(DAY, -30 -1, y.last_process_date)
        GROUP BY x.date_key) a)b
  WHERE rn_num > 1;
```

+++

詳しくは、 [オーディエンスサイズの変更トレンドウィジェットのドキュメント](../guides/audiences.md#audience-size-change-trend) を参照してください。

## ID 別のオーディエンスサイズのトレンド {#audience-size-trend-by-identity}

このインサイトによって回答された質問：

- オーディエンスは、常に増加、安定化、変動を経験しているか。
- 時間の経過と共にオーディエンスの増加にスパイクや減少が生じる特定の ID はありますか？
- 時間の経過と共に ID が増加する際に異常が発生しているか。

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT sum(count_of_profiles) AS identities,
        date_key
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines x
  INNER JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
    FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
    WHERE process_name = 'FACT_TABLES_PROCESSING'
      AND process_status = 'SUCCESSFUL'
    GROUP BY merge_policy_id) y ON x.merge_policy_id = y.merge_policy_id
  INNER JOIN qsaccel.profile_agg.adwh_dim_namespaces z ON x.namespace_id = z.namespace_id
  AND x.merge_policy_id = z.merge_policy_id
  WHERE x.date_key >= dateadd(DAY, -30, y.last_process_date)
    AND x.segment_id = 1333234510
    AND z.namespace_description = 'crmid'
  GROUP BY date_key;
```

+++

詳しくは、 [ID ウィジェットドキュメント別のオーディエンスサイズのトレンド](../guides/audiences.md#audience-size-trend-by-identity) を参照してください。

## オーディエンスサイズのトレンド {#audience-size-trend}

このインサイトによって回答された質問：

- 異常値を含め、オーディエンスサイズは時間の経過と共にどのように変化しましたか？
- 30 日、90 日、12 か月の期間で、オーディエンスサイズの全体的なトレンドを見つけるにはどうすればよいですか？
- サイズに貢献するオーディエンスの主な特徴は何ですか？ 例えば、電子メールマーケティングキャンペーンによるスパイクなどです。

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT date_key,
        sum(count_of_profiles) AS audience_size
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines x
  INNER JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
    FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
    WHERE process_name = 'FACT_TABLES_PROCESSING'
      AND process_status = 'SUCCESSFUL'
    GROUP BY merge_policy_id) y ON x.merge_policy_id = y.merge_policy_id
  WHERE date_key >= dateadd(DAY, -30, y.last_process_date)
    AND x.segment_id = 1333234510
  GROUP BY date_key,
          segment_id;
```

+++

詳しくは、 [オーディエンスサイズのトレンドウィジェットのドキュメント](../guides/audiences.md#audience-size-trend) を参照してください。

## オーディエンスサイズ {#audience-size}

このインサイトによって回答された質問：

- 現在の合計オーディエンスサイズはどれくらいですか？
- 現在のオーディエンスのサイズは、以前の期間や特定のオーディエンスと比較してどのように異なりますか？
- 最近のマーケティングキャンペーンは、オーディエンスサイズにどのような影響を与えますか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT
  sum(
    qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.count_of_profiles
  ) count_of_profiles
FROM
  qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_id = qsaccel.profile_agg.adwh_dim_segments.segment_id
WHERE
  qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_id = -1323307941
  AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 1914917902
  AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-12';
```

+++

詳しくは、 [オーディエンスサイズウィジェットドキュメント](../guides/audiences.md#audience-size) を参照してください。

## スコアの顧客 AI 配分 {#customer-ai-distribution-of-scores}

このインサイトによって回答された質問：

- 選択したオーディエンスでフィルタリングした、顧客 AI モデルの各バケットのスコア付け配分は何ですか。
- 特定のオーディエンスに対する高、中、低のスコア配分とは何ですか。
- 様々な関心のあるオーディエンスによるスコア付け分布の分類は何ですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT b.model_name,
      b.model_type,
      c.segment_name,
      c.segment_id,
      CASE
        WHEN score >= 0
          AND score < 25 THEN 'LOW'
        WHEN score >= 25
          AND score < 75 THEN 'MEDIUM'
        WHEN score >= 75
          AND score <= 100 THEN 'HIGH'
        END bucket_name,
      CASE
        WHEN score >= 0
          AND score < 5 THEN '02.50'
        WHEN score >= 5
          AND score < 10 THEN '07.50'
        WHEN score >= 10
          AND score < 15 THEN '12.50'
        WHEN score >= 15
          AND score < 20 THEN '17.50'
        WHEN score >= 20
          AND score < 25 THEN '22.50'
        WHEN score >= 25
          AND score < 30 THEN '27.50'
        WHEN score >= 30
          AND score < 35 THEN '32.50'
        WHEN score >= 35
          AND score < 40 THEN '37.50'
        WHEN score >= 40
          AND score < 45 THEN '42.50'
        WHEN score >= 45
          AND score < 50 THEN '47.50'
        WHEN score >= 50
          AND score < 55 THEN '52.50'
        WHEN score >= 55
          AND score < 60 THEN '57.50'
        WHEN score >= 60
          AND score < 65 THEN '62.50'
        WHEN score >= 65
          AND score < 70 THEN '67.50'
        WHEN score >= 70
          AND score < 75 THEN '72.50'
        WHEN score >= 75
          AND score < 80 THEN '77.50'
        WHEN score >= 80
          AND score < 85 THEN '82.50'
        WHEN score >= 85
          AND score < 90 THEN '87.50'
        WHEN score >= 90
          AND score < 95 THEN '92.50'
        WHEN score >= 95
          AND score <= 100 THEN '97.50'
        END score_bins,
      Sum(CASE
            WHEN score >= 0
              AND score < 25 THEN count_of_profiles
            WHEN score >= 25
              AND score < 75 THEN count_of_profiles
            WHEN score >= 75
              AND score <= 100 THEN count_of_profiles
        END) count_of_profiles
   FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_ai_models a
          JOIN qsaccel.profile_agg.adwh_dim_ai_models b ON a.merge_policy_id = b.merge_policy_id
     AND a.model_id = b.model_id
          JOIN qsaccel.profile_agg.adwh_dim_segments c ON a.segment_id = c.segment_id
   WHERE a.merge_policy_id = 1133248113
     AND a.model_id = 1829081696
     AND a.segment_id = 1870062812
     AND score_date =
         (SELECT MAX(score_date)
          FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_ai_models d
          WHERE d.model_id = a.model_id) GROUP  BY b.model_name,
             b.model_type,
             c.segment_name,
             c.segment_id,
             CASE
               WHEN score >= 0
                 AND score < 25 THEN 'LOW'
               WHEN score >= 25
                 AND score < 75 THEN 'MEDIUM'
               WHEN score >= 75
                 AND score <= 100 THEN 'HIGH'
               END,
             CASE
               WHEN score >= 0
                 AND score < 5 THEN '02.50'
               WHEN score >= 5
                 AND score < 10 THEN '07.50'
               WHEN score >= 10
                 AND score < 15 THEN '12.50'
               WHEN score >= 15
                 AND score < 20 THEN '17.50'
               WHEN score >= 20
                 AND score < 25 THEN '22.50'
               WHEN score >= 25
                 AND score < 30 THEN '27.50'
               WHEN score >= 30
                 AND score < 35 THEN '32.50'
               WHEN score >= 35
                 AND score < 40 THEN '37.50'
               WHEN score >= 40
                 AND score < 45 THEN '42.50'
               WHEN score >= 45
                 AND score < 50 THEN '47.50'
               WHEN score >= 50
                 AND score < 55 THEN '52.50'
               WHEN score >= 55
                 AND score < 60 THEN '57.50'
               WHEN score >= 60
                 AND score < 65 THEN '62.50'
               WHEN score >= 65
                 AND score < 70 THEN '67.50'
               WHEN score >= 70
                 AND score < 75 THEN '72.50'
               WHEN score >= 75
                 AND score < 80 THEN '77.50'
               WHEN score >= 80
                 AND score < 85 THEN '82.50'
               WHEN score >= 85
                 AND score < 90 THEN '87.50'
               WHEN score >= 90
                 AND score < 95 THEN '92.50'
               WHEN score >= 95
                 AND score <= 100 THEN '97.50'
               END;
```

+++

詳しくは、 [スコアウィジェットのドキュメントの顧客 AI 配布](../guides/audiences.md#customer-ai-distribution-of-scores) を参照してください。

## 顧客 AI スコア付けの概要 {#customer-ai-scoring-summary}

このインサイトによって回答された質問：

- 特定のオーディエンスに関する顧客 AI モデルごとのスコア付けの概要を教えてください。
- 様々なオーディエンスの顧客 AI 傾向スコアは、どのように変化しますか。
- スコア付けの概要とオーディエンスの概要の他の KPI を比較すると、どのように異なりますか。

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT model_name,
         model_type,
         segment_name,
         CASE
             WHEN score BETWEEN 0 AND 24 THEN 'LOW'
             WHEN score BETWEEN 25 AND 74 THEN 'MEDIUM'
             WHEN score BETWEEN 75 AND 100 THEN 'HIGH'
         END score_buckets,
         sum(count_of_profiles) count_of_profiles
  FROM QSAccel.profile_agg.adwh_fact_profile_by_segment_ai_models a
  JOIN QSAccel.profile_agg.adwh_dim_ai_models b ON a.merge_policy_id=b.merge_policy_id
  AND a.model_id=b.model_id
  JOIN QSAccel.profile_agg.adwh_dim_segments c ON a.segment_id=c.segment_id
  WHERE a.merge_policy_id=1133248113
    AND a.model_id =1829081696
    AND a.segment_id=1870062812
    AND score_date=
      (SELECT max(score_date)
       FROM QSAccel.profile_agg.adwh_fact_profile_by_segment_ai_models d
       WHERE d.model_id=a.model_id)
  GROUP BY model_name,
           model_type,
           segment_name,
           CASE
               WHEN score BETWEEN 0 AND 24 THEN 'LOW'
               WHEN score BETWEEN 25 AND 74 THEN 'MEDIUM'
               WHEN score BETWEEN 75 AND 100 THEN 'HIGH'
           END;
```

+++

詳しくは、 [顧客 AI スコアリング概要ウィジェットのドキュメント](../guides/audiences.md#customer-ai-scoring-summary) を参照してください。

## ID の重複 {#identity-overlap}

このインサイトによって回答された質問：

- A と B の共通の交差点は何ですか。 [!UICONTROL ID タイプ A] および [!UICONTROL ID タイプ B] フィルターを適用したオーディエンスの場合は、
- 特定の ID タイプの重複に基づいて顧客オーディエンスを絞り込み、ターゲットを絞ったマーケティング戦略を強化するには、どうすればよいですか？
- 交差する領域内のキャンペーンのパフォーマンスを評価することで得られるインサイトは何ですか？
- これらのインサイトに基づいて、今後のマーケティング活動をどのように最適化できますか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT Sum(overlap_col1) overlap_col1,
        Sum(overlap_col2) overlap_col2,
        Sum(overlap_count) Overlap_count
  FROM
    (SELECT 0 overlap_col1,
            0 overlap_col2,
            Sum(count_of_profiles) Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace_by_segment
    WHERE qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace_by_segment.segment_id = 1333234510
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace_by_segment.merge_policy_id = 1709997014
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace_by_segment.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace_by_segment.overlap_id IN
        (SELECT a.overlap_id
          FROM
            (SELECT qsaccel.profile_agg.adwh_dim_overlap_namespaces.overlap_id overlap_id,
                    count(*) cnt_num
            FROM qsaccel.profile_agg.adwh_dim_overlap_namespaces
            WHERE qsaccel.profile_agg.adwh_dim_overlap_namespaces.merge_policy_id = 1709997014
              AND qsaccel.profile_agg.adwh_dim_overlap_namespaces.overlap_namespaces in ('crmid',
                                                                                          'email')
            GROUP BY qsaccel.profile_agg.adwh_dim_overlap_namespaces.overlap_id)a
          WHERE a.cnt_num>1 )
    UNION ALL SELECT count_of_profiles overlap_col1,
                      0 overlap_col2,
                      0 Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines
    LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
    AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
    WHERE qsaccel.profile_agg.adwh_dim_namespaces.namespace_description = 'crmid'
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.segment_id = 1333234510
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.merge_policy_id = 1709997014
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.date_key = '2024-01-10'
    UNION ALL SELECT 0 overlap_col1,
                      count_of_profiles overlap_col2,
                      0 Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines
    LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
    AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
    WHERE qsaccel.profile_agg.adwh_dim_namespaces.namespace_description = 'email'
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.segment_id = 1333234510
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.merge_policy_id = 1709997014
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.date_key = '2024-01-10' ) a;
```

+++

詳しくは、 [ID 重複ウィジェットのドキュメント](../guides/audiences.md#identity-overlap) を参照してください。

## ID 別プロファイル {#profiles-by-identity}

このインサイトによって回答された質問：

- 選択したオーディエンスのプロファイルの合計数の中で最も割合が高い ID タイプはどれですか？
- 選択したオーディエンスの ID タイプには大きな相違はありますか？
- オーディエンスごとの ID タイプの全体的な分布は何ですか？
- 様々なオーディエンスの ID 数に重大な相違点や異常点はありますか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT qsaccel.profile_agg.adwh_dim_namespaces.namespace_description,
        sum(qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.count_of_profiles) count_of_profiles
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
  AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.segment_id = 1333234510
    AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.merge_policy_id = 1709997014
    AND qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace_trendlines.date_key = '2024-01-10'
  GROUP BY qsaccel.profile_agg.adwh_dim_namespaces.namespace_description
  ORDER BY count_of_profiles DESC;
```

+++

詳しくは、 [ID 別プロファイルウィジェットドキュメント](../guides/audiences.md#profiles-by-identity) を参照してください。

## 予定されているアクティベーション {#scheduled-activations}

このインサイトによって回答された質問：

- 特定のプラットフォームでの特定のオーディエンスに対するパフォーマンスの最も高いアクティベーションの開始日と終了日はどれくらいですか？
- 特定のオーディエンスのスケジュールされたアクティベーションに最も使用されたプラットフォームはどれですか？
- 特定のオーディエンスに対するアクティベーション戦略の優先順位付けや多様化に関する決定をガイドする、プラットフォームの使用にパターンはありますか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT p.destination_platform ,
       p.destination_platform_name AS platform ,
       d.destination_name ,
       d.destination ,
       br.start_date ,
       CASE
           WHEN br.end_date = '9999-12-31' THEN 'Ongoing'
           ELSE br.end_date
       END AS end_date
  FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations br
  JOIN qsaccel.profile_agg.adwh_dim_destination d ON br.destination_id = d.destination_id
  JOIN qsaccel.profile_agg.adwh_dim_destination_platform p ON d.destination_platform_id = p.destination_platform_id
  JOIN
    (SELECT MAX(process_date) AS last_process_date
     FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
     WHERE process_name = 'FACT_TABLES_PROCESSING'
       AND process_status = 'SUCCESSFUL' ) lpd ON lpd.last_process_date BETWEEN br.start_date AND br.end_date
  AND br.segment_id = 1333234510;
```

+++

詳しくは、 [スケジュールされたアクティベーションウィジェットのドキュメント](../guides/audiences.md#scheduled-activations) を参照してください。

## 次の手順

このドキュメントでは、ダッシュボードのインサイトを生成する SQL と、この分析で解決する一般的な質問について説明します。 これで、SQL を編集および繰り返して、独自のインサイトを生成できます。

<!-- This link will go in during the January release.
See the [View SQL documentation]() for more information on how to adapt your insights' SQL directly through the PLatform UI. -->

また、 [プロファイル](./profiles.md) および [宛先](./destinations.md) ダッシュボード。
