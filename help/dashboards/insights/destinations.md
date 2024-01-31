---
title: 宛先インサイト
description: 宛先のインサイトを強化する SQL を見つけ、これらのクエリを使用してカスタムインサイトを生成し、Adobe Experience Platformからのデータのアクティベーションをさらに詳しく調べます。
source-git-commit: 3d5dd6300952409e2dddb32eb11547fb43a5feac
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 2%

---

# 宛先インサイト

データモデルの分析から得られたインサイトにより、Adobe Real-time Customer Data Platformのデータにアクセスしやすく、理解しやすく、意思決定に対する影響が大きくなります。

宛先のインサイトを理解するには、基盤となる SQL にアクセスし、独自のインサイトを生成して、Adobe Experience Platformから宛先プラットフォームへのデータのアクティベーションをさらに詳しく調べます。 既存のReal-Time CDPデータモデル SQL をインスピレーションとして使用し、生データを新しい実用的なインサイトに変換して、独自のビジネスニーズに応えるクエリを作成します。

<!-- This link will go in during the January release.
See the [View SQL documentation]() for more information on how to adapt your insights' SQL directly through the PLatform UI.  -->

以下のインサイトを、 [宛先ダッシュボード](../guides/destinations.md) またはカスタム [ユーザー定義ダッシュボード](../user-defined-dashboards.md). 詳しくは、 [カスタマイズの概要](../customize/overview.md) を参照してください。 [新しいウィジェットを作成および編集します](../customize/custom-widgets.md) ウィジェットライブラリで、および [ユーザー定義ダッシュボード](../user-defined-dashboards.md#create-widget).

## アクティブ化されたオーディエンス {#activated-audiences}

このインサイトによって回答された質問：

- 特定の宛先でフィルタリングされた、アクティブ化されたオーディエンスの総数はどれくらいですか？
- 各宛先でアクティブ化されるオーディエンスの数は何ですか？

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

詳しくは、 [アクティブ化されたオーディエンスウィジェットのドキュメント](../guides/destinations.md#activated-audiences) を参照してください。

## すべての宛先でアクティブ化されたオーディエンス {#activated-audiences-across-all-destinations}

このインサイトによって回答された質問：

- すべての宛先でアクティブ化されるオーディエンスの数は？
- アクティブ化されたオーディエンスの合計数はどれくらいですか？

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

詳しくは、 [すべての宛先ウィジェットドキュメントでアクティブ化されたオーディエンス](../guides/destinations.md#activated-audiences-across-all-destinations) を参照してください。

## 宛先プラットフォーム別のアクティブな宛先 {#active-destinations-by-destination-platform}

このインサイトによって回答された質問：

- アクティブな宛先の数
- アクティブな宛先を宛先プラットフォームごとに分類すると、どうなりますか。
- アクティブな宛先の数を各宛先プラットフォーム別に分類した結果はどれくらいですか。

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

詳しくは、 [宛先プラットフォームウィジェットドキュメント別のアクティブな宛先](../guides/destinations.md#active-destinations-by-destination-platform) を参照してください。

## オーディエンスサイズのトレンド {#audience-size-trend}

このインサイトによって回答された質問：

- オーディエンスサイズは、時間の経過と共に、宛先にマッピングされたオーディエンスの異常値を含めて、どのように変化しましたか？
- 指定した 30 日、90 日、12 ヶ月の期間で、オーディエンスの規模、宛先別の全体的なトレンドを見つけるには、どうすればよいですか？
- 電子メールマーケティングキャンペーンに関するスパイクなど、サイズに影響を与えているオーディエンスの主な特徴は何ですか？

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

詳しくは、 [オーディエンスサイズのトレンドウィジェットのドキュメント](../guides/destinations.md#audience-size-trend) を参照してください。

## 一般的なオーディエンス {#common-audiences}

このインサイトによって回答された質問：

- 2 つの異なる宛先間で共通するオーディエンスはどれですか？
- 2 つの異なる宛先間の共通のオーディエンスには、どのくらいのプロファイルがありますか。
- 2 つの宛先がマッピングされる最大のオーディエンスはどれですか？

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

詳しくは、 [一般的なオーディエンスウィジェットのドキュメント](../guides/destinations.md#common-audiences) を参照してください。

## 宛先のステータス {#destination-status}

このインサイトによって回答された質問：

- 使用が有効になっている宛先の合計数はどれくらいですか？
- 無効になっている宛先の合計数はどれくらいですか？
- 有効な宛先と無効な宛先の間の分割率はどれくらいですか？

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

詳しくは、 [宛先ステータスウィジェットのドキュメント](../guides/destinations.md#destination-status) を参照してください。

## 宛先数 {#destinations-count}

このインサイトによって回答された質問：

- 現在設定されている宛先の数はいくつですか？
- 宛先の合計数は、時間の経過と共にどのように変化しましたか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT count(destination_id) AS total_number_of_destinations
  FROM qsaccel.profile_agg.adwh_dim_destination;
```

+++

詳しくは、 [宛先数ウィジェットのドキュメント](../guides/destinations.md#destinations-count) を参照してください。

## マッピングされたオーディエンスの正常性 {#mapped-audience-health}

このインサイトによって回答された質問：

- 宛先にマッピングされたオーディエンスのうち、過去 30 日間で大きなバリエーションを持つのはどれですか。
- マッピングされたオーディエンスの最新のサイズと、過去 1 か月間に変更されたかどうか。
- 先月のサイズ変更の重大度に基づいて、宛先にマッピングされているすべてのオーディエンスをリストするには、どうすればよいですか。

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

詳しくは、 [マッピングされたオーディエンスヘルスウィジェットのドキュメント](../guides/destinations.md#mapped-audience-health) を参照してください。

## マッピングされたオーディエンス {#mapped-audiences}

このインサイトによって回答された質問：

- 特定の宛先にマッピングされるオーディエンスの数は？
- マッピングされたオーディエンスの数は、時間の経過と共にどのように変化しましたか？
- 2 つの宛先を比較して、各宛先にマッピングされたオーディエンスの重複を確認するには、どうすればよいですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT COUNT(segment_id) AS mapped_audiences_count
FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations
WHERE destination_id = 1458738325;
```

+++

詳しくは、 [マッピングされたオーディエンスウィジェットのドキュメント](../guides/destinations.md#mapped-audiences) を参照してください。

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
- 各宛先にマッピングされるオーディエンスの数（最大から最小の順）
- オーディエンスのマッピングと宛先のマッピングは、スナップショット間でどのように変化しますか。

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

詳しくは、 [最も使用される宛先ウィジェットのドキュメント](../guides/destinations.md#most-used-destinations) を参照してください。

## 最近アクティブ化されたオーディエンス {#recently-activated-audiences}

このインサイトによって回答された質問：

- 最近アクティブ化されたオーディエンスの宛先はどれですか？
- すべての宛先のリストを最終更新日で並べ替える方法を教えてください。
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

詳しくは、 [最近アクティブ化されたオーディエンスウィジェットのドキュメント](../guides/destinations.md#recently-activated-audiences) を参照してください。

## 最近アクティブ化されたセグメント（宛先別） {#recently-activated-audiences-by-destination}

このインサイトによって回答された質問：

- 特定の宛先に対してアクティブ化されるオーディエンスは何ですか？
- 特定のオーディエンスによってアクティブ化されたオーディエンスのリストを最新のオーディエンスから最新のオーディエンスに検索する方法を教えてください。
- 特定の宛先に対してアクティブ化された日付別にオーディエンスのリストを見つけるには、どうすればよいですか。

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

詳しくは、 [宛先ウィジェットによって最近アクティブ化されたオーディエンスドキュメント](../guides/destinations.md#recently-activated-audiences-by-destination) を参照してください。

## 最近作成した宛先 {#recently-created-destinations}

このインサイトによって回答された質問：

- 最近作成された宛先はどれですか？
- 宛先とその作成日のリストを見つけるにはどうすればよいですか。
- 最近、新しく作成された宛先は何ですか？

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

詳しくは、 [最近作成した宛先ウィジェットのドキュメント](../guides/destinations.md#recently-created-destinations) を参照してください。

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

このドキュメントでは、ダッシュボードのインサイトを生成する SQL と、この分析で解決する一般的な質問について説明します。 これらの SQL クエリを編集および繰り返して、独自のインサイトを生成できるようになりました。

<!-- This link will go in during the January release.
See the [View SQL documentation]() for more information on how to adapt your insights' SQL directly through the PLatform UI. -->

また、 [プロファイル](./profiles.md) および [オーディエンス](./audiences.md) ダッシュボード。

<!-- 
SQL MISSING FROM WIKI:
Unmapped audiences by identity
Mapped audiences by identity 
-->
