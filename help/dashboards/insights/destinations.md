---
title: 宛先インサイト
description: 宛先のインサイトを強化する SQL を確認し、これらのクエリを使用してカスタムインサイトを生成し、Adobe Experience Platformからのデータのアクティブ化をさらに詳しく調べます。
exl-id: 762a9960-e7a5-4796-80c7-ef745157cc04
source-git-commit: d4baf6cfaa772e5d46cef470fb35818c7af868b1
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 2%

---

# 宛先インサイト

データモデルの分析から得られるインサイトにより、Adobe Real-time Customer Data Platform データがよりアクセスしやすく、理解しやすく、意思決定に影響を与えやすくなります。

宛先を強化する SQL にアクセスして宛先インサイトを理解し、独自のインサイトを生成して、Adobe Experience Platformから宛先プラットフォームへのデータのアクティブ化をさらに詳しく調べます。 既存のReal-Time CDP データモデル SQL をインスピレーションとして使用し、独自のビジネスニーズに合ったクエリを作成することで、生データを新しい実用的なインサイトに変換します。

を参照してください。 [SQL ドキュメントを表示](../view-sql.md) platform UI を使用してインサイトの SQL を直接調整する方法について詳しくは、こちらを参照してください。

次のインサイトはすべて [宛先ダッシュボード](../guides/destinations.md) またはカスタム [ユーザー定義ダッシュボード](../user-defined-dashboards.md). を参照してください。 [カスタマイズの概要](../customize/overview.md) ダッシュボードをカスタマイズする方法、または [新しいウィジェットの作成と編集](../customize/custom-widgets.md) ウィジェットライブラリ内および [ユーザー定義ダッシュボード](../user-defined-dashboards.md#create-widget).

## 有効化されたオーディエンス {#activated-audiences}

このインサイトによって回答された質問：

- 特定の宛先でフィルタリングされた、アクティブ化されたオーディエンスの合計数はどれくらいですか？
- 各宛先ごとのアクティブ化されたオーディエンス数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT
  COUNT(segment_id) AS Activated_Audiences_Count
FROM
  qsaccel.profile_agg.adwh_dim_br_segment_destinations
WHERE
  (
    SELECT
      MAX(process_date)
    FROM
      qsaccel.profile_agg.adwh_lkup_process_delta_log
    WHERE
      process_name = 'FACT_TABLES_PROCESSING'
      AND process_status = 'SUCCESSFUL'
  ) BETWEEN start_date AND end_date
  AND destination_id = 1458738325;
```

+++

を参照してください。 [アクティブ化されたオーディエンスウィジェットのドキュメント](../guides/destinations.md#activated-audiences) このインサイトの外観と機能について説明します。

## すべての宛先のアクティブ化されたオーディエンス {#activated-audiences-across-all-destinations}

このインサイトによって回答された質問：

- すべての宛先でアクティブ化されているオーディエンスの数
- アクティブ化されたオーディエンスの合計数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT count(segment_id) AS Activated_Audiences_Count
FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations
WHERE
    (SELECT MAX(process_date)
     FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
     WHERE process_name = 'FACT_TABLES_PROCESSING'
       AND process_status = 'SUCCESSFUL' ) BETWEEN start_date AND end_date;
```

+++

を参照してください。 [すべての宛先のアクティブ化されたオーディエンス ウィジェットドキュメント](../guides/destinations.md#activated-audiences-across-all-destinations) このインサイトの外観と機能について説明します。

## 宛先プラットフォーム別のアクティブな宛先 {#active-destinations-by-destination-platform}

このインサイトによって回答された質問：

- アクティブな宛先の数
- 宛先プラットフォーム別のアクティブな宛先の分類は何ですか？
- 宛先プラットフォームごとに分類された、アクティブな宛先の数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT destination_platform_name AS Destination_Platform_Name,
       COUNT(destination_id) AS Active_Destinations_Count
  FROM qsaccel.profile_agg.adwh_dim_destination a
  INNER JOIN qsaccel.profile_agg.adwh_dim_destination_platform b ON a.destination_platform_id = b.destination_platform_id
  WHERE destination_status='enabled'
  GROUP BY destination_platform_name
  ORDER BY Active_Destinations_Count DESC
  LIMIT 20;
```

+++

を参照してください。 [宛先プラットフォームウィジェット別のアクティブな宛先に関するドキュメント](../guides/destinations.md#active-destinations-by-destination-platform) このインサイトの外観と機能について説明します。

## オーディエンスサイズのトレンド {#audience-size-trend}

このインサイトによって回答された質問：

- 宛先にマッピングされたオーディエンスの異常値を含め、オーディエンスサイズは時間の経過とともにどのように変化しますか？
- 30 日、90 日および 12 か月の指定期間におけるオーディエンスサイズの宛先別の全体的なトレンドを見つけるにはどうすればよいですか？
- サイズに影響するオーディエンスの主な特徴（メールマーケティングキャンペーンに関するスパイクなど）

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT d.destination_name,
        d.destination,
        d.destination_id,
        b.segment_name,
        b.segment,
        c.segment_id,
        a.date_key,
        sum(a.count_of_profiles) AS profile_count
  FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines a
  INNER JOIN qsaccel.profile_agg.adwh_dim_segments b ON a.segment_id = b.segment_id
  INNER JOIN qsaccel.profile_agg.adwh_dim_br_segment_destinations c ON a.segment_id = c.segment_id
  INNER JOIN qsaccel.profile_agg.adwh_dim_destination d ON c.destination_id = d.destination_id
  INNER JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
    FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
    WHERE process_name = 'FACT_TABLES_PROCESSING'
      AND process_status = 'SUCCESSFUL'
    GROUP BY merge_policy_id) f ON a.merge_policy_id = f.merge_policy_id
  WHERE a.date_key >= dateadd(DAY, -30-1, f.last_process_date)
    AND d.destination_id = -1275507046
    AND c.segment_id = -1452100519
  GROUP BY d.destination_name,
          d.destination,
          d.destination_id,
          b.segment_name,
          b.segment,
          c.segment_id,
          a.date_key;
```

+++

を参照してください。 [オーディエンスサイズのトレンドウィジェットのドキュメント](../guides/destinations.md#audience-size-trend) このインサイトの外観と機能について説明します。

## 一般的なオーディエンス {#common-audiences}

このインサイトによって回答された質問：

- 2 つの異なる宛先間で共通するオーディエンスはどれですか？
- 2 つの異なる宛先間の共通オーディエンスの各プロファイルはいくつ持ちますか？
- 2 つの宛先のマッピング先となる最大のオーディエンスはどれですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT k.destination_name1,
       k.destination_1,
       k.destination_id1,
       k.destination_name2,
       k.destination_2,
       k.destination_id2,
       b.segment_name,
       b.segment,
       b.segment_id,
       sum(a.count_of_profiles) AS profile_count
  FROM
    (SELECT i.destination_name AS destination_name1,
            i.destination AS destination_1,
            i.destination_id AS destination_id1,
            j.destination_name AS destination_name2,
            j.destination AS destination_2,
            j.destination_id AS destination_id2,
            i.segment_id
     FROM
       (SELECT b.destination_name,
               b.destination,
               b.destination_id,
               a.segment_id
        FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations a
        INNER JOIN qsaccel.profile_agg.adwh_dim_destination b ON a.destination_id=b.destination_id
        WHERE b.destination_id=1458738325) AS i
     INNER JOIN
       (SELECT b.destination_name,
               b.destination,
               b.destination_id,
               a.segment_id
        FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations a
        INNER JOIN qsaccel.profile_agg.adwh_dim_destination b ON a.destination_id=b.destination_id
        WHERE b.destination_id=-635802802) AS j ON i.segment_id=j.segment_id) AS k
  INNER JOIN qsaccel.profile_agg.adwh_fact_profile_by_segment a ON a.segment_id = k.segment_id
  INNER JOIN qsaccel.profile_agg.adwh_dim_segments b ON b.segment_id = k.segment_id
  INNER JOIN
    (SELECT MAX(process_date) last_process_date,
            merge_policy_id
     FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
     WHERE process_name = 'FACT_TABLES_PROCESSING'
       AND process_status = 'SUCCESSFUL'
     GROUP BY merge_policy_id) c ON a.merge_policy_id = c.merge_policy_id
  WHERE a.date_key = c.last_process_date
  GROUP BY k.destination_name1,
           k.destination_1,
           k.destination_id1,
           k.destination_name2,
           k.destination_2,
           k.destination_id2,
           b.segment_name,
           b.segment,
           b.segment_id
  ORDER BY profile_count DESC
  LIMIT 20;
```

+++

を参照してください。 [一般的なオーディエンスウィジェットのドキュメント](../guides/destinations.md#common-audiences) このインサイトの外観と機能について説明します。

## 宛先ステータス {#destination-status}

このインサイトによって回答された質問：

- 使用可能な宛先の合計数は？
- 無効になっている宛先の合計数は何ですか？
- 有効な宛先と無効な宛先のパーセンテージ分割は何ですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT COUNT(CASE
                 WHEN destination_status='enabled' THEN 1
             END) AS count_of_active_destinations,
       COUNT(CASE
                 WHEN destination_status='disabled' THEN 1
             END) AS count_of_inactive_destinations
FROM qsaccel.profile_agg.adwh_dim_destination;
```

+++

を参照してください。 [宛先ステータスウィジェットのドキュメント](../guides/destinations.md#destination-status) このインサイトの外観と機能について説明します。

## 宛先数 {#destinations-count}

このインサイトによって回答された質問：

- 現在設定されている宛先の数
- 宛先の合計数は時間の経過とともにどのように変化していますか。

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT count(destination_id) AS total_number_of_destinations
  FROM qsaccel.profile_agg.adwh_dim_destination;
```

+++

を参照してください。 [宛先数ウィジェットのドキュメント](../guides/destinations.md#destinations-count) このインサイトの外観と機能について説明します。

## マッピングされたオーディエンスの正常性 {#mapped-audience-health}

このインサイトによって回答された質問：

- 宛先にマッピングされたオーディエンスのうち、過去 30 日間に大きな変化を持つものはどれですか？
- マッピングされたオーディエンスの最新サイズはどれくらいですか？また、先月にわたって変更されたかどうかも教えてください。
- 先月のサイズ変更の重大度に基づいて、宛先にマッピングされたすべてのオーディエンスをリストするにはどうすればよいですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT destination_name,
        SEGMENT,
        segment_id,
        segment_name,
        avg_profile_count,
        latest_profile_count,
        stddev_profile_count,
        profile_count_z_factor
  FROM
    (SELECT b.destination_name,
            f.segment_id,
            c.segment_name,
            c.segment,
            f.avg_profile_count,
            f.latest_profile_count,
            f.stddev_profile_count,
            CASE
                WHEN stddev_profile_count = 0 THEN 0 ELSE(f.latest_profile_count - f.avg_profile_count)/f.stddev_profile_count
            END AS profile_count_z_factor
    FROM
      (SELECT segment_id,
              avg(profile_count) AS avg_profile_count,
              sum(CASE
                      WHEN last_process_date = date_key THEN profile_count
                      ELSE 0
                  END) AS latest_profile_count,
              stdevp(profile_count) AS stddev_profile_count
        FROM
          (SELECT x.date_key,
                  x.segment_id,
                  d.last_process_date,
                  sum(x.count_of_profiles) AS profile_count
          FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_trendlines x
          INNER JOIN
            (SELECT MAX(process_date) last_process_date,
                    merge_policy_id
              FROM qsaccel.profile_agg.adwh_lkup_process_delta_log
              WHERE process_name = 'FACT_TABLES_PROCESSING'
                AND process_status = 'SUCCESSFUL'
              GROUP BY merge_policy_id) d ON x.merge_policy_id = d.merge_policy_id
          WHERE x.date_key >= dateadd (DAY, -30, d.last_process_date)
          GROUP BY x.date_key,
                    x.segment_id,
                    d.last_process_date) AS t
        GROUP BY segment_id) AS f
    INNER JOIN qsaccel.profile_agg.adwh_dim_segments c ON f.segment_id = c.segment_id
    INNER JOIN qsaccel.profile_agg.adwh_dim_br_segment_destinations a ON a.segment_id = c.segment_id
    INNER JOIN qsaccel.profile_agg.adwh_dim_destination b ON a.destination_id = b.destination_id
    WHERE b.destination_id = 1458738325) AS m
  WHERE abs(m.profile_count_z_factor) >= 1
  ORDER BY m.latest_profile_count DESC
  LIMIT 20;
```

+++

を参照してください。 [マッピングされたオーディエンス正常性ウィジェットのドキュメント](../guides/destinations.md#mapped-audience-health) このインサイトの外観と機能について説明します。

## マッピングされたオーディエンス {#mapped-audiences}

このインサイトによって回答された質問：

- 特定の宛先にマッピングされているオーディエンスの数
- マッピングされたオーディエンスの数は時間の経過と共にどのように変化していますか。
- 2 つの宛先を比較して、各宛先にマッピングされたオーディエンスの重複を確認するにはどうすればよいですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT COUNT(segment_id) AS mapped_audiences_count
FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations
WHERE destination_id = 1458738325;
```

+++

を参照してください。 [マッピングされたオーディエンスウィジェットのドキュメント](../guides/destinations.md#mapped-audiences) このインサイトの外観と機能について説明します。

<!-- Commented out until the Jan release as the SQL IS MISSING:
## Mapped audiences by identity {#mapped-audiences-by-identity}

Questions answered by this insight:

- How do I find a list of audiences that are mapped to a destination?
- What is the count of identities for audiences mapped to a destination?
- Which audiences have the highest count of identities mapped to a particular destination?

+++Select to reveal the SQL that generates this insight

```sql
```

+++

See the [Mapped audiences by identity widget documentation](../guides/destinations.md#mapped-audiences-by-identity) for information on the appearance and functionality of this insight.
-->

## 最も使用されている宛先 {#most-used-destinations}

このインサイトによって回答された質問：

- 最も使用されている宛先は何ですか？
- 各宛先にマッピングされるオーディエンスの数（多い順に並べ替えたもの）。
- オーディエンスと宛先のマッピングは、スナップショット間でどのように変化しますか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT qsaccel.profile_agg.adwh_dim_destination.destination_name,
       qsaccel.profile_agg.adwh_dim_destination.destination_id,
       qsaccel.profile_agg.adwh_dim_destination.destination,
       count(DISTINCT qsaccel.profile_agg.adwh_dim_br_segment_destinations.segment_id) segment_count
  FROM qsaccel.profile_agg.adwh_dim_destination
  JOIN qsaccel.profile_agg.adwh_dim_br_segment_destinations ON qsaccel.profile_agg.adwh_dim_destination.destination_id = qsaccel.profile_agg.adwh_dim_br_segment_destinations.destination_id
  WHERE qsaccel.profile_agg.adwh_dim_destination.destination_name IS NOT NULL
  GROUP BY qsaccel.profile_agg.adwh_dim_destination.destination_name,
           qsaccel.profile_agg.adwh_dim_destination.destination,
           qsaccel.profile_agg.adwh_dim_destination.destination_id
  ORDER BY segment_count DESC
  LIMIT 20;
```

+++

を参照してください。 [最も使用されている宛先ウィジェットのドキュメント](../guides/destinations.md#most-used-destinations) このインサイトの外観と機能について説明します。

## 最近アクティブ化されたオーディエンス {#recently-activated-audiences}

このインサイトによって回答された質問：

- 最近アクティブ化されたオーディエンスの宛先はどれですか？
- 最終更新日順に並べ替えたすべての宛先のリストを見つけるにはどうすればよいですか？
- 最新のアクティベーションに基づいて 2 つの宛先を比較するにはどうすればよいですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT
  segment_name,
  segment,
  destination_name,
  a.create_time create_time
FROM
  qsaccel.profile_agg.adwh_dim_br_segment_destinations a
  INNER JOIN qsaccel.profile_agg.adwh_dim_segments b ON a.segment_id = b.segment_id
  INNER JOIN qsaccel.profile_agg.adwh_dim_destination c ON a.destination_id = c.destination_id
ORDER BY
  create_time DESC,
  segment
LIMIT
  20;
```

+++

を参照してください。 [最近アクティブ化されたオーディエンスウィジェットのドキュメント](../guides/destinations.md#recently-activated-audiences) このインサイトの外観と機能について説明します。

## 最近アクティブ化されたセグメント（宛先別） {#recently-activated-audiences-by-destination}

このインサイトによって回答された質問：

- 特定の宛先に対してアクティブ化されたオーディエンスは何ですか？
- 特定のオーディエンスによってアクティブ化されたオーディエンスのリストを最新の状態から最新の状態にするにはどうすればよいですか？
- 特定の宛先に対してアクティブ化された日付までのオーディエンスのリストを見つけるにはどうすればよいですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT c.destination_name,
       c.destination,
       c.destination_id,
       b.segment_name,
       b.segment,
       b.segment_id,
       a.create_time activated
  FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations a
  INNER JOIN qsaccel.profile_agg.adwh_dim_segments b ON a.segment_id=b.segment_id
  INNER JOIN qsaccel.profile_agg.adwh_dim_destination c ON a.destination_id=c.destination_id
  WHERE c.destination_id=-1275507046
  ORDER BY a.create_time DESC,
           a.segment_id
  LIMIT 20;
```

+++

を参照してください。 [最近アクティブ化されたオーディエンス （宛先別）のウィジェットドキュメント](../guides/destinations.md#recently-activated-audiences-by-destination) このインサイトの外観と機能について説明します。

## 最近作成した宛先 {#recently-created-destinations}

このインサイトによって回答された質問：

- 最も最近作成された宛先はどれか
- 作成日を含む宛先のリストを見つけるにはどうすればよいですか？
- 最近作成された新しい宛先

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT DISTINCT
  destination,
  destination_name,
  create_time
FROM
  qsaccel.profile_agg.adwh_dim_destination
WHERE
  destination_status = 'enabled'
ORDER BY
  create_time DESC
LIMIT
  20;
```

+++

を参照してください。 [最近作成した宛先ウィジェットのドキュメント](../guides/destinations.md#recently-created-destinations) このインサイトの外観と機能について説明します。

<!-- Commented out until the Jan release as SQL MISSING FROM WIKI:

## Unmapped audiences by identity {#unmapped-audiences-by-identity}

Questions answered by this insight:

- How do I find a list of audiences that are not mapped to a destination?
- What is the count of identities for audiences that are not mapped to a destination?
- Which audiences have the highest count of identities not mapped to a particular destination?

+++Select to reveal the SQL that generates this insight

```sql
```

+++

See the [Unmapped audiences by identity widget documentation](../guides/destinations.md#unmapped-audiences-by-identity) for information on the appearance and functionality of this insight.

-->

## 次の手順 {#next-steps}

このドキュメントでは、ダッシュボードインサイトを生成する SQL と、この分析で解決される一般的な質問について説明しました。 これらの SQL クエリを編集および反復して、独自のインサイトを生成できるようになりました。

を参照してください。 [SQL ドキュメントを表示](../view-sql.md) platform UI を使用してインサイトの SQL を直接調整する方法について詳しくは、こちらを参照してください。

また、のインサイトを生成する SQL を読んで理解することもできます [プロファイル](./profiles.md), [アカウントプロファイル](./account-profiles.md) および [オーディエンス](./audiences.md) ダッシュボード。
