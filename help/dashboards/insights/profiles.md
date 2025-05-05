---
title: プロファイルインサイト
description: プロファイルインサイトを強化する SQL を確認し、これらのクエリを使用して、顧客とその消費者エクスペリエンスをさらに詳しく調べるカスタムインサイトを生成します。
exl-id: f3792076-3e01-4e26-8788-32927202a2e5
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '1660'
ht-degree: 3%

---

# プロファイルインサイト

データモデルの分析から得られるインサイトにより、Adobe Real-Time CDP データがよりアクセスしやすく、理解しやすく、意思決定に影響を与えやすくなります。

プロファイルを強化する SQL にアクセスしてプロファイルインサイトを理解し、独自のインサイトを生成して、プロファイルを構成する顧客とその消費者エクスペリエンスをさらに詳しく調べます。 既存のReal-Time CDP データモデル SQL をインスピレーションとして使用し、独自のビジネスニーズに合ったクエリを作成することで、生データを新しい実用的なインサイトに変換します。

Experience Platform UI を使用してインサイトの SQL を直接調整する方法について詳しくは、[SQL のドキュメントを表示 ](../view-sql.md) を参照してください。

次のインサイトはすべて、[ プロファイルダッシュボード ](../guides/profiles.md) またはカスタム [ ユーザー定義ダッシュボード ](../standard-dashboards.md) の一部として使用できます。 ダッシュボードをカスタマイズする方法、またはウィジェットライブラリおよび [ ユーザー定義ダッシュボード ](../customize/custom-widgets.md) で [ 新しいウィジェットの作成と編集 ](../customize/overview.md) を使用する方法については、[ カスタマイズの概要 ](../standard-dashboards.md#create-widget) を参照してください。

## オーディエンスの重複 (結合ポリシー別) {#audience-overlap-by-merge-policy}

このinsightが回答した質問：

- 両方のオーディエンスに共通のプロファイルはどれですか？
- 重複は、エンゲージメントやコンバージョン率にどのような影響を与えますか？
- 重複するセグメントに合わせてマーケティング戦略を調整するにはどうすればよいですか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT Sum(overlap_col1) overlap_col1,
        Sum(overlap_col2) overlap_col2,
        Sum(overlap_count) Overlap_count
  FROM
    (SELECT 0 overlap_col1,
            0 overlap_col2,
            sum(count_of_overlap)Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments
    WHERE qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.date_key = '2024-01-10'
      AND ((qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment1=1333234510
            AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment2=1559754729)
            OR (qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment1=1559754729
                AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_segments.segment2=1333234510))
    UNION ALL SELECT sum(count_of_profiles) overlap_col1,
                      0 overlap_col2,
                      0 overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
    LEFT JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_Id = qsaccel.profile_agg.adwh_dim_segments.segment_Id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_dim_segments.segment_Id = 1333234510
    UNION ALL SELECT 0 overlap_col1,
                      sum(count_of_profiles) overlap_col2,
                      0 Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
    JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_Id = qsaccel.profile_agg.adwh_dim_segments.segment_Id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_dim_segments.segment_Id = 1559754729 ) a;
```

+++

このinsightの外観と機能について詳しくは、[ 結合ポリシーウィジェットによるオーディエンスの重複 ](../guides/profiles.md#audience-overlap-by-merge-policy) ドキュメント）を参照してください。

## オーディエンス重複レポート {#audience-overlap-report}

このinsightが回答した質問：

- 最も重複している 50 人のオーディエンスは何ですか？
- 重複が最も少ない 50 個のオーディエンスはどれですか？
- 結合ポリシーによって重複するパターンはどのように変化しますか？

+++選択して、このinsightを生成する SQL を表示します

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

このinsightの外観と機能について詳しくは、[ オーディエンスの重複レポートウィジェットのドキュメント ](../guides/profiles.md#audience-overlap-report) を参照してください。

## オーディエンス （数） {#audiences}

このinsightが回答した質問：

- セグメント化に主に使用される結合ポリシーはどれですか？
- 結合ポリシー間でのオーディエンスの配分はどうなっていますか。
- 特定の結合ポリシーのオーディエンス数に経時的に大きな変化はありますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT count(DISTINCT a.segment_id) count_of_segments
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines a
  JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
    FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
    WHERE process_name = 'FACT_TABLES_PROCESSING'
      AND process_status = 'SUCCESSFUL'
    GROUP BY merge_policy_id) b ON a.merge_policy_id= b.merge_policy_id
  AND a.date_key = b.last_process_date
  WHERE a.merge_policy_id= 2027892989;
```

+++

このinsightの外観と機能については、[Audiences ウィジェットのドキュメント ](../guides/profiles.md#audiences) を参照してください。

## 宛先ステータスにマッピングされたオーディエンス {#audiences-mapped-to-destination-status}

このinsightが回答した質問：

- マッピングされた宛先とマッピングされていない宛先の間のオーディエンスの全体的な分布
- マッピングされたオーディエンスの数が最も多い特定の宛先はどれですか？
- マッピングされていないオーディエンスが合計に占める割合はどれくらいですか？
- マッピングされていないオーディエンスのうち、パターンや関連するトレンドはありますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT COUNT(DISTINCT (y.segment_id)) AS count_mapped_segments,
        COUNT(DISTINCT (x.segment_id)) - COUNT(DISTINCT (y.segment_id)) AS count_unmapped_segments,
        COUNT(DISTINCT (x.segment_id)) AS total_segments
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines x
  LEFT JOIN qsaccel.profile_agg.adwh_dim_br_segment_destinations y ON x.segment_id = y.segment_id
  INNER JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
    FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
    WHERE process_name = 'FACT_TABLES_PROCESSING'
      AND process_status = 'SUCCESSFUL'
    GROUP BY merge_policy_id) z ON x.merge_policy_id = z.merge_policy_id
  AND x.date_key = z.last_process_date
  WHERE x.merge_policy_id = 2027892989;
```

+++

このinsightの外観と機能について詳しくは、[ 宛先ステータスウィジェットにマッピングされたオーディエンス ](../guides/profiles.md#audiences-mapped-to-destination-status) ドキュメントを参照してください。

## オーディエンスサイズ {#audiences-size}

このinsightが回答した質問：

- 最大サイズを持つのは、どのオーディエンスセグメントですか？
- 最大 5 人のオーディエンスはどれですか？
- 上位オーディエンスのオーディエンスサイズ分布は時間の経過と共にどのように変化しますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key,
        qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name,
        qsaccel.profile_agg.adwh_dim_segments.segment,
        qsaccel.profile_agg.adwh_dim_segments.segment_name,
        sum(qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.count_of_profiles)count_of_profiles
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_segments ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.segment_id = qsaccel.profile_agg.adwh_dim_segments.segment_id
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id=adwh_dim_merge_policies.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key = '2024-01-10'
    AND qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.merge_policy_id= 2027892989
  GROUP BY qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines.date_key,
          qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name,
          qsaccel.profile_agg.adwh_dim_segments.segment,
          qsaccel.profile_agg.adwh_dim_segments.segment_name
  ORDER BY count_of_profiles DESC
  LIMIT 20;
```

+++

このinsightの外観と機能について詳しくは、[ オーディエンスサイズウィジェットのドキュメント ](../guides/profiles.md#audiences-size) を参照してください。

## スコアの顧客 AI 分布 {#customer-ai-distribution-of-scores}

このinsightが回答した質問：

- 顧客 AI モデルごとに、バケット間でのスコアの分布はどれくらいですか？
- 高スコア、中スコア、低スコアごとのスコアの分布は何ですか？
- 結合ポリシーによるスコアリング分布の分類は何ですか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT b.model_name,
     b.model_type,
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
  FROM qsaccel.profile_agg.adwh_fact_profile_ai_models a
  JOIN qsaccel.profile_agg.adwh_dim_ai_models b ON a.merge_policy_id = b.merge_policy_id
  AND a.model_id = b.model_id
  WHERE a.merge_policy_id = 2027892989
    AND a.model_id = 1829081696
    AND score_date =
      (SELECT Max(score_date)
       FROM qsaccel.profile_agg.adwh_fact_profile_ai_models d
       WHERE d.model_id = a.model_id) GROUP  BY b.model_name,
          model_type,
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

このinsightのアピアランスと機能について詳しくは、[ スコアの顧客 AI 分布ウィジェットのドキュメント ](../guides/profiles.md#customer-ai-distribution-of-scores) を参照してください。

## 顧客 AI スコアリングの概要 {#customer-ai-scoring-summary}

このinsightが回答した質問：

- 顧客 AI モデルごとのスコアリングの概要は何ですか？
- 様々なオーディエンスに対して顧客 AI の傾向スコアはどのように変化しますか？
- プロファイルの概要で、スコアリング概要が他の KPI と比較してどのように変化するか。

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT model_name,
         model_type,
         CASE
             WHEN score BETWEEN 0 AND 24 THEN 'LOW'
             WHEN score BETWEEN 25 AND 74 THEN 'MEDIUM'
             WHEN score BETWEEN 75 AND 100 THEN 'HIGH'
         END score_buckets,
         sum(count_of_profiles) count_of_profiles
  FROM QSAccel.profile_agg.adwh_fact_profile_ai_models a
  JOIN QSAccel.profile_agg.adwh_dim_ai_models b ON a.merge_policy_id=b.merge_policy_id
  AND a.model_id=b.model_id
  WHERE a.merge_policy_id=2027892989
    AND a.model_id =1829081696
    AND score_date=
      (SELECT max(score_date)
       FROM QSAccel.profile_agg.adwh_fact_profile_ai_models d
       WHERE d.model_id=a.model_id)
  GROUP BY model_name,
           model_type,
           CASE
               WHEN score BETWEEN 0 AND 24 THEN 'LOW'
               WHEN score BETWEEN 25 AND 74 THEN 'MEDIUM'
               WHEN score BETWEEN 75 AND 100 THEN 'HIGH'
           END;
```

+++

このinsightのアピアランスと機能について詳しくは、[ 顧客 AI スコア概要ウィジェットのドキュメント ](../guides/profiles.md#customer-ai-scoring-summary) を参照してください。

## ID の重複 {#identity-overlap}

このinsightが回答した質問：

- [!UICONTROL ID タイプ A] と [!UICONTROL ID タイプ B] の一般的な積集合は何ですか。
- 特定の ID タイプの重複に基づいて顧客オーディエンスを絞り込み、ターゲットマーケティング戦略を強化するにはどうすればよいですか？
- 交差領域内のキャンペーンのパフォーマンスを評価することで、どのようなインサイトを得ることができますか？
- このキャンペーンパフォーマンス insightを使用して、今後のマーケティング活動をどのように最適化できますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT Sum(overlap_col1) overlap_col1,
        Sum(overlap_col2) overlap_col2,
        coalesce(Sum(overlap_count), 0) overlap_count
  FROM
    (SELECT 0 overlap_col1,
            0 overlap_col2,
            Sum(count_of_profiles) overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace
    WHERE qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace.overlap_id IN
        (SELECT a.overlap_id
          FROM
            (SELECT qsaccel.profile_agg.adwh_dim_overlap_namespaces.overlap_id overlap_id,
                    count(*) cnt_num
            FROM qsaccel.profile_agg.adwh_dim_overlap_namespaces
            WHERE qsaccel.profile_agg.adwh_dim_overlap_namespaces.merge_policy_id = 2027892989
              AND qsaccel.profile_agg.adwh_dim_overlap_namespaces.overlap_namespaces in ('avid',
                                                                                          'crmid')
            GROUP BY qsaccel.profile_agg.adwh_dim_overlap_namespaces.overlap_id)a
          WHERE a.cnt_num>1 )
    UNION ALL SELECT count_of_profiles overlap_col1,
                      0 overlap_col2,
                      0 overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines
    JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
    AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_dim_namespaces.namespace_description = 'avid'
    UNION ALL SELECT 0 overlap_col1,
                      count_of_profiles overlap_col2,
                      0 Overlap_count
    FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines
    JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
    AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
    WHERE qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key = '2024-01-10'
      AND qsaccel.profile_agg.adwh_dim_namespaces.namespace_description = 'crmid' )a;
```

+++

このinsightのアピアランスと機能について詳しくは、[ID の重複ウィジェットのドキュメント ](../guides/profiles.md#identity-overlap) を参照してください。

## プロファイル数 {#profile-count}

このinsightが回答した質問：

- Adobe Real-Time Customer Data Platformの全体的なプロファイル数
- 結合ポリシーに基づいたプロファイルの分散方法
- プロファイル数が最も多い結合ポリシーはどれですか？

これらのインサイトを生成する SQL を次に示します。

```sql
SELECT qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name,
       sum(qsaccel.profile_agg.adwh_fact_profile.count_of_profiles) CNT
  FROM qsaccel.profile_agg.adwh_fact_profile
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_id=adwh_fact_profile.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile.date_key='2024-01-10'
    AND qsaccel.profile_agg.adwh_fact_profile.merge_policy_id = 2027892989
  GROUP BY qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name;
```

このinsightの外観と機能について詳しくは、[ プロファイル数ウィジェットガイド ](https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/profiles.html?lang=ja#profile-count) を参照してください。

このinsightのアピアランスと機能について詳しくは、[ プロファイル数ウィジェットのドキュメント ](../guides/profiles.md#profile-count) を参照してください。

## プロファイル数の変更 {#profile-count-change}

このinsightが回答した質問：

- プロファイル数の全体的な変化のトレンドは何ですか？
- プロファイル数の大幅なスパイクや低下の原因は何ですか？
- プロファイル数の変更を促進している特定の結合ポリシーはありますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT (sum(count_of_profiles) - sum(count_of_profiles_days_ago)) profiles_added
  FROM
    (SELECT sum(qsaccel.profile_agg.adwh_fact_profile.count_of_profiles) count_of_profiles,
            0 count_of_profiles_days_ago
    FROM qsaccel.profile_agg.adwh_fact_profile
    WHERE qsaccel.profile_agg.adwh_fact_profile.merge_policy_id = 2027892989
      AND qsaccel.profile_agg.adwh_fact_profile.date_key = '2024-01-10'
    UNION ALL SELECT 0 count_of_profiles,
                      CASE
                          WHEN sum(cntondatediff) =0 THEN sum(cntmin)
                          ELSE sum(cntondatediff)
                      END AS count_of_profiles_days_ago
    FROM
      (SELECT coalesce(sum(qsaccel.profile_agg.adwh_fact_profile_by_trendlines.count_of_profiles), 0) cntondatediff,
              0 cntmin
        FROM qsaccel.profile_agg.adwh_fact_profile_by_trendlines
        WHERE qsaccel.profile_agg.adwh_fact_profile_by_trendlines.merge_policy_id =2027892989
          AND qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key =dateadd(DAY, - 30, '2024-01-10')
        UNION ALL SELECT 0 cntondatediff,
                        sum(qsaccel.profile_agg.adwh_fact_profile_by_trendlines.count_of_profiles) countMin
        FROM qsaccel.profile_agg.adwh_fact_profile_by_trendlines
        WHERE qsaccel.profile_agg.adwh_fact_profile_by_trendlines.merge_policy_id = 2027892989
          AND qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key =
            (SELECT min(qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key) col
            FROM qsaccel.profile_agg.adwh_fact_profile_by_trendlines
            WHERE qsaccel.profile_agg.adwh_fact_profile_by_trendlines.merge_policy_id =2027892989
              AND qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key >= dateadd(DAY, - 30, '2024-01-10')
              AND qsaccel.profile_agg.adwh_fact_profile_by_trendlines.count_of_profiles IS NOT NULL) )b) a;
```

+++

このinsightのアピアランスと機能について詳しくは、[ プロファイル数の変更ウィジェットのドキュメント ](../guides/profiles.md#profile-count-change) を参照してください。

## プロファイル数の変化のトレンド {#profile-count-change-trend}

このinsightが回答した質問：

- 結合ポリシーに基づいた、過去 12 か月間のプロファイル数の変化の全体的なトレンドは何ですか。
- 過去 30 日以内にプロファイル数の変化に特定のパターンや変動があり、注意が必要なものはありますか？
- 過去 90 日間のプロファイル数の変化を、全体的なトレンドと比較してどうですか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT date_key,
         profiles_count_change
  FROM
    (SELECT rn_num,
            date_key,
            (count_of_profiles-lag(count_of_profiles, 1, 0) over(
                                                            ORDER BY date_key))profiles_count_change
    FROM
      (SELECT qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key,
              sum(qsaccel.profile_agg.adwh_fact_profile_by_trendlines.count_of_profiles) count_of_profiles,
              row_number() OVER (
                              ORDER BY qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key) rn_num
      FROM qsaccel.profile_agg.adwh_fact_profile_by_trendlines
  WHERE qsaccel.profile_agg.adwh_fact_profile_by_trendlines.merge_policy_id = 2027892989
    AND qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key >=dateadd(DAY, - 30 -1, '2024-01-10')
  GROUP BY qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key)a)b
  WHERE rn_num > 1;
```

+++

このinsightのアピアランスと機能について詳しくは、[ プロファイル数の変化のトレンドウィジェットのドキュメント ](../guides/profiles.md#profile-count-change-trend) を参照してください。

## プロファイル数のトレンド {#profile-count-trend}

このinsightが回答した質問：

- 過去 30 日間の結合ポリシーに基づくプロファイル数の全体的なトレンドは何ですか？
- このトレンドに基づいて、長期的なトレンド（90 日と 12 か月など）と比較するとどうですか？
- 指定した期間（30 日、90 日および 12 か月）のプロファイル数の増減に最も貢献している結合ポリシーはどれですか？
- プロファイル数に、30 日間の期間内の特定のイベントや期間と関連する特定のスパイクや急減はありますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT date_key,
       sum(count_of_profiles) AS count_of_profiles
  FROM qsaccel.profile_agg.adwh_fact_profile_by_trendlines x
  INNER JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
     FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
     WHERE process_name = 'FACT_TABLES_PROCESSING'
       AND process_status = 'SUCCESSFUL'
     GROUP BY merge_policy_id) y ON x.merge_policy_id = y.merge_policy_id
  WHERE date_key >= dateadd(DAY, -365, y.last_process_date)
    AND x.merge_policy_id = 2027892989
  GROUP BY date_key;
```

+++

このinsightのアピアランスと機能について詳しくは、[ プロファイル数のトレンドウィジェットのドキュメント ](../guides/profiles.md#profile-count-trend) を参照してください。

## ID 別プロファイル {#profiles-by-identity}

このinsightが回答した質問：

- プロファイルの合計数のうち、割合が高い ID タイプはどれですか？
- ID タイプ間に大きな違いはありますか？
- ID タイプの全体的な分布はどれくらいですか？
- ID カウントに大きな相違や異常はありますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT qsaccel.profile_agg.adwh_dim_namespaces.namespace_description,
        sum(qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.count_of_profiles) count_of_profiles
  FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
  AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = 2027892989
    AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key = '2024-01-10'
  GROUP BY qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key,
          qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id,
          qsaccel.profile_agg.adwh_dim_namespaces.namespace_description
  ORDER BY count_of_profiles DESC;
```

+++

このinsightのアピアランスと機能について詳しくは、[ID 別プロファイル ウィジェットのドキュメント ](../guides/profiles.md#profiles-by-identity) を参照してください。

## プロファイル数の変化のトレンド {#profiles-count-change-trend}

このinsightが回答した質問：

- 結合ポリシーに基づいて、過去 12 か月間のプロファイル数の変化の全体的なトレンドは何ですか。
- 注意が必要な特定のパターンや、過去 30 日間のプロファイル数の変化の変動はありますか？
- 過去 90 日間のプロファイルの変化は、全体的なトレンドと比較してどのようにカウントされますか。

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT date_key,
         profiles_count_change
  FROM
    (SELECT rn_num,
            date_key,
            (count_of_profiles-lag(count_of_profiles, 1, 0) over(
                                                            ORDER BY date_key))profiles_count_change
    FROM
      (SELECT qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key,
              sum(qsaccel.profile_agg.adwh_fact_profile_by_trendlines.count_of_profiles) count_of_profiles,
              row_number() OVER (
                              ORDER BY qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key) rn_num
      FROM qsaccel.profile_agg.adwh_fact_profile_by_trendlines
  WHERE qsaccel.profile_agg.adwh_fact_profile_by_trendlines.merge_policy_id = 2027892989
    AND qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key >=dateadd(DAY, - 30 -1, '2024-01-10')
  GROUP BY qsaccel.profile_agg.adwh_fact_profile_by_trendlines.date_key)a)b
  WHERE rn_num > 1;
```

+++

このinsightのアピアランスと機能について詳しくは、[ プロファイル数の変化のトレンドウィジェットのドキュメント ](../guides/profiles.md#profiles-count-change-trend) を参照してください。

## プロファイル数の変化のトレンド (ID 別) {#profiles-count-change-trend-by-identity}

このinsightが回答した質問：

- 過去 12 か月間の、様々な ID でのプロファイル数の変化の全体的なトレンドは何ですか。
- 過去 30 日以内に大きな変化を示す特定の ID のトレンドはありますか？
- 特定の ID の 30 日、90 日および 12 か月のトレンドを比較する場合、プロファイル数の変化はどのように異なりますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT date_key,
        namespace_description,
        profiles_count_change
  FROM
    (SELECT rn_num,
            date_key,
            namespace_description,
            (count_of_profiles - lag(count_of_profiles, 1, 0) over(PARTITION BY namespace_description
                                                                  ORDER BY date_key)) profiles_count_change
    FROM
      (SELECT qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key,
              qsaccel.profile_agg.adwh_dim_namespaces.namespace_description,
              sum(qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.count_of_profiles) count_of_profiles,
              row_number() OVER (PARTITION BY qsaccel.profile_agg.adwh_dim_namespaces.namespace_description
                                  ORDER BY qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key) rn_num
        FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines
        LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
        AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
        WHERE qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = 2027892989
          AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.namespace_id= -1042977439
          AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key >= dateadd(DAY, - 30 -1, '2024-01-10')
        GROUP BY qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key,
                adwh_dim_namespaces.namespace_description)a)b
  WHERE rn_num > 1;
```

+++

このinsightのアピアランスと機能について詳しくは、[ID ウィジェット別プロファイル数の変化のトレンド ](../guides/profiles.md#profiles-count-change-trend-by-identity) のドキュメントを参照してください。

## 単一の ID プロファイル {#single-identity-profiles}

このinsightが回答した質問：

- 顧客 ID データは、単一の ID で一貫して表されますか？
- 単一のタイプの ID のみを持つプロファイルで構成される、ユーザーベースの割合
- 単一のタイプの ID のみを持つプロファイルの場合、これはプロファイルの完全性にどのように影響しますか？
- 最も一般的な ID タイプと単一 ID プロファイル数には相関関係がありますか。

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name,
       sum(qsaccel.profile_agg.adwh_fact_profile.count_of_Single_Identity_profiles) CNT
  FROM qsaccel.profile_agg.adwh_fact_profile
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_id=adwh_fact_profile.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile.date_key='2024-01-10'
    AND qsaccel.profile_agg.adwh_fact_profile.merge_policy_id = 2027892989
  GROUP BY qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name;
```

+++

このinsightのアピアランスと機能について詳しくは、[ 単一の ID プロファイルウィジェットのドキュメント ](../guides/profiles.md#single-identity-profiles) を参照してください。

## 単一の ID プロファイル (ID 別) {#single-identity-profiles-by-identity}

このinsightが回答した質問：

- 単一の ID （メール、電話番号など）で登録した一意の顧客は何人ですか？
- 電子メールや電話番号など、様々な ID タイプ間での単一の ID プロファイルの分布は何ですか？
- 単一の ID プロファイル内に新たな ID パターンやシフトはありますか？

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT qsaccel.profile_agg.adwh_dim_namespaces.namespace_description,
        sum(qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.count_of_Single_Identity_profiles) count_of_Single_Identity_profiles
  FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces ON qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.namespace_id = qsaccel.profile_agg.adwh_dim_namespaces.namespace_id
  AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = qsaccel.profile_agg.adwh_dim_namespaces.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id = 2027892989
    AND qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key = '2024-01-10'
  GROUP BY qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.date_key,
          qsaccel.profile_agg.adwh_fact_profile_by_namespace_trendlines.merge_policy_id,
          qsaccel.profile_agg.adwh_dim_namespaces.namespace_description;
```

+++

このinsightのアピアランスと機能について詳しくは、[ID ウィジェット別の単一の ID プロファイル ](../guides/profiles.md#single-identity-profiles-by-identity) のドキュメントを参照してください。

## セグメント化されていないプロファイル {#unsegmented-profiles}

このinsightが回答した質問：

- オーディエンスに含まれていないプロファイルの数
- セグメント化されていないプロファイルで表される合計オーディエンスの割合
- セグメント化されていない多数のプロファイルに何らかの結合ポリシーが影響していますか。

+++選択して、このinsightを生成する SQL を表示します

```sql
SELECT qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name,
       sum(qsaccel.profile_agg.adwh_fact_profile.count_of_Orphan_profiles) CNT
  FROM qsaccel.profile_agg.adwh_fact_profile
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_id=adwh_fact_profile.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile.date_key='2024-01-10'
    AND qsaccel.profile_agg.adwh_fact_profile.merge_policy_id = 2027892989
  GROUP BY qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name;
```

+++

このinsightのアピアランスと機能について詳しくは、[ セグメント化されていないプロファイルウィジェットのドキュメント ](../guides/profiles.md#unsegmented-profiles) を参照してください。

## 次の手順

このドキュメントでは、ダッシュボードインサイトを生成する SQL と、この分析で解決される一般的な質問について説明しました。 SQL を編集および繰り返して、独自のインサイトを生成できるようになりました。

PLatform UI を使用してインサイトの SQL を直接調整する方法について詳しくは、[SQL ドキュメントの表示 ](../view-sql.md) を参照してください。

また、[ オーディエンス ](./audiences.md)、[ アカウントプロファイル ](./account-profiles.md) および [ 宛先 ](./destinations.md) ダッシュボードのインサイトを生成する SQL を読み、理解することもできます。
