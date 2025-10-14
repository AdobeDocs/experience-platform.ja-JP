---
title: オーディエンスインサイト
description: オーディエンスインサイトを強化する SQL を確認し、これらのクエリを使用してカスタムインサイトを生成し、Adobe Experience Platformのオーディエンスデータをさらに詳しく調べます。
exl-id: 99624234-c4e1-44bb-9567-505bc0c4723e
source-git-commit: cce576c00823a0c02e4b639f0888a466a5af6a0c
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 3%

---

# オーディエンスインサイト

データモデルの分析から得られるインサイトにより、Adobe Real-Time CDP データがよりアクセスしやすく、理解しやすく、意思決定に影響を与えやすくなります。

オーディエンスを強化する SQL にアクセスしてオーディエンスのインサイトを理解し、オーディエンスを構成する ID とプロファイルをさらに詳しく調べるために独自のインサイトを生成します。 既存のReal-Time CDP データモデル SQL をインスピレーションとして使用し、独自のビジネスニーズに合ったクエリを作成することで、生データを新しい実用的なインサイトに変換します。

PLatform UI を使用してインサイトの SQL を直接調整する方法について詳しくは、[SQL ドキュメントの表示 &#x200B;](../view-sql.md) を参照してください。

次のインサイトはすべて、[&#x200B; オーディエンスダッシュボード &#x200B;](../guides/audiences.md) またはカスタム [&#x200B; ユーザー定義ダッシュボード &#x200B;](../standard-dashboards.md) の一部として使用できます。 ダッシュボードをカスタマイズする方法、またはウィジェットライブラリおよび [&#x200B; ユーザー定義ダッシュボード &#x200B;](../customize/custom-widgets.md) で [&#x200B; 新しいウィジェットの作成と編集 &#x200B;](../customize/overview.md) を使用する方法については、[&#x200B; カスタマイズの概要 &#x200B;](../standard-dashboards.md#create-widget) を参照してください。

次のインサイトはすべて、[&#x200B; オーディエンスダッシュボード &#x200B;](../guides/audiences.md) またはカスタムダッシュボードの一部として使用できます。

## オーディエンス重複レポート {#audience-overlap-report}

このインサイトによって回答された質問：

- 特定のフィルター済みオーディエンスの上位 50 の重複オーディエンスとは何ですか？
- 特定のフィルター済みオーディエンスのうち、重複が最も少ない 50 個のオーディエンスとは何ですか？
- フィルタリングされた別のオーディエンスに対して、重複するパターンはどのように変化しますか？

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

このインサイトの外観と機能について詳しくは、[&#x200B; オーディエンスの重複レポートウィジェットのドキュメント &#x200B;](../guides/audiences.md#audience-overlap-report) を参照してください。

## オーディエンスの重複 {#audience-overlap}

このインサイトによって回答された質問：

- 両方のオーディエンスに共通のプロファイルはどれですか？
- 重複は、エンゲージメントやコンバージョン率にどのような影響を与えますか？
- 重複するセグメントに合わせてマーケティング戦略を調整するにはどうすればよいですか？

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

このインサイトの外観と機能について詳しくは、[&#x200B; オーディエンスの重複ウィジェットのドキュメント &#x200B;](../guides/audiences.md#audience-overlap) を参照してください。

## オーディエンスサイズ変化トレンド {#audience-size-change-trend}

このインサイトによって回答された質問：

- 過去 30 日間、90 日間、12 か月以内に、オーディエンスサイズに大きなスパイクや減少はありますか？
- オーディエンスサイズは特定の日にどのように変化しますか？
- 過去 12 か月間に、異常や、スパイクやくぼみの繰り返しパターンが検出されましたか。

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

このインサイトの外観と機能について詳しくは、[&#x200B; オーディエンスサイズの変化のトレンドウィジェットのドキュメント &#x200B;](../guides/audiences.md#audience-size-change-trend) を参照してください。

## オーディエンスサイズのトレンド (ID 別) {#audience-size-trend-by-identity}

このインサイトによって回答された質問：

- オーディエンスは継続的に増加しているか、安定しているか、変動を経験しているか。
- 時間の経過と共にオーディエンスの増加にスパイクや急減を伴う特定の ID はありますか？
- ID の増加に経時的な異常はありますか？

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

このインサイトの外観と機能について詳しくは、[ID ウィジェット別のオーディエンスサイズのトレンド &#x200B;](../guides/audiences.md#audience-size-trend-by-identity) のドキュメントを参照してください。

## オーディエンスサイズのトレンド {#audience-size-trend}

このインサイトによって回答された質問：

- 異常を含め、オーディエンスサイズは時間の経過とともにどのように変化していますか。
- 30 日、90 日および 12 か月という期間のオーディエンスサイズの全体的なトレンドを見つけるにはどうすればよいですか？
- サイズに貢献するオーディエンスの主な特徴は何ですか？ 例えば、メールマーケティングキャンペーンに起因するスパイクなどです。

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

このインサイトの外観と機能について詳しくは、[&#x200B; オーディエンスサイズのトレンドウィジェットのドキュメント &#x200B;](../guides/audiences.md#audience-size-trend) を参照してください。

## オーディエンスサイズ {#audience-size}

このインサイトによって回答された質問：

- 現在の合計オーディエンスサイズはいくつですか？
- 現在のオーディエンスサイズを前期間や特定のオーディエンスと比較するとどうですか？
- 最近のマーケティングキャンペーンがオーディエンスサイズに与える影響は何ですか？

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

このインサイトの外観と機能について詳しくは、[&#x200B; オーディエンスサイズウィジェットのドキュメント &#x200B;](../guides/audiences.md#audience-size) を参照してください。

## スコアの顧客 AI 分布 {#customer-ai-distribution-of-scores}

このインサイトによって回答された質問：

- 選択したオーディエンスでフィルタリングされた、顧客 AI モデルの各バケットのスコアリング配分とは何ですか？
- 特定のオーディエンスの高、中、低のスコアリング分布は何ですか？
- 様々な対象オーディエンスごとのスコアリング分布の分類はどれくらいですか？

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

このインサイトの外観と機能について詳しくは、[&#x200B; スコアの顧客 AI 分布ウィジェットのドキュメント &#x200B;](../guides/audiences.md#customer-ai-distribution-of-scores) を参照してください。

## 顧客 AI スコアリングの概要 {#customer-ai-scoring-summary}

このインサイトによって回答された質問：

- 特定のオーディエンスの各顧客 AI モデルのスコアリングの概要は何ですか？
- 様々なオーディエンスに対して顧客 AI の傾向スコアはどのように変化しますか？
- スコアリング概要とオーディエンス概要の他の KPI はどのように比較されますか？

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

このインサイトの外観と機能について詳しくは、[&#x200B; 顧客 AI スコアの概要ウィジェットのドキュメント &#x200B;](../guides/audiences.md#customer-ai-scoring-summary) を参照してください。

## ID の重複 {#identity-overlap}

このインサイトによって回答された質問：

- フィルタリングされたオーディエンスに対する [!UICONTROL ID タイプ A] と [!UICONTROL ID タイプ B] の一般的な共通部分は何ですか。
- 特定の ID タイプの重複に基づいて顧客オーディエンスを絞り込み、ターゲット設定されたマーケティング戦略を強化するには、どうすればよいですか？
- 交差領域内のキャンペーンのパフォーマンスを評価することで、どのようなインサイトを得ることができますか？
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

このインサイトの外観と機能について詳しくは、[ID の重複ウィジェットのドキュメント &#x200B;](../guides/audiences.md#identity-overlap) を参照してください。

## ID 別プロファイル {#profiles-by-identity}

このインサイトによって回答された質問：

- 選択したオーディエンスのプロファイルの合計数のうち、最も割合が高い ID タイプはどれですか。
- 選択したオーディエンスの ID タイプ間に大きな違いはありますか？
- オーディエンス別の ID タイプの全体的な分布はどれくらいですか？
- 様々なオーディエンスの ID カウントに大きな相違や異常はありますか？

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

このインサイトの外観と機能について詳しくは、[ID 別プロファイル ウィジェットのドキュメント &#x200B;](../guides/audiences.md#profiles-by-identity) を参照してください。

## 予定されているアクティベーション {#scheduled-activations}

このインサイトによって回答された質問：

- 特定のプラットフォームでの特定のオーディエンスに対する最もパフォーマンスの高いアクティベーションの開始日と終了日
- 特定のオーディエンスの予定されたアクティベーションに最も使用されたプラットフォームはどれですか？
- 特定のオーディエンスに対するアクティベーション戦略の優先順位付けまたは多様化に関する決定のガイドとなる platform の使用のパターンはありますか？

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

このインサイトの外観と機能について詳しくは、[&#x200B; 予定されているアクティベーションウィジェットのドキュメント &#x200B;](../guides/audiences.md#scheduled-activations) を参照してください。

## 次の手順

このドキュメントでは、ダッシュボードインサイトを生成する SQL と、この分析で解決される一般的な質問について説明しました。 SQL を編集および繰り返して、独自のインサイトを生成できるようになりました。

PLatform UI を使用してインサイトの SQL を直接調整する方法について詳しくは、[SQL ドキュメントの表示 &#x200B;](../view-sql.md) を参照してください。

また、[&#x200B; プロファイル &#x200B;](./profiles.md)、[&#x200B; アカウントプロファイル &#x200B;](./account-profiles.md) および [&#x200B; 宛先 &#x200B;](./destinations.md) ダッシュボードのインサイトを生成する SQL を読み、理解することもできます。
