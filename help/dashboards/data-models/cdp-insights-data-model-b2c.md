---
title: Real-time Customer Data Platform Insights データモデル B2C エディション
description: Real-time Customer Data Platform Insights データモデル（B2C Edition）で SQL クエリを使用して、マーケティングおよび KPI のユースケースに合わせて独自のReal-Time CDP レポートをカスタマイズする方法を説明します。
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 61bc7f23-9f79-4c75-a515-85dd9dda2d02
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 1%

---

# Real-time Customer Data Platform Insights データモデル B2C エディション

のReal-time Customer Data Platform Insights データモデル [B2C エディション](../../rtcdp/overview.md#rtcdp-b2c) は、様々なプロファイル、宛先、セグメント化ウィジェットに関するインサイトを強化するデータモデルと SQL を公開します。 これらの SQL クエリテンプレートをカスタマイズすると、マーケティングおよび主要業績評価指標（KPI）のユースケースに関するReal-Time CDP レポートを作成できます。 ユーザー定義のダッシュボードのカスタムウィジェットとして、これらのインサイトを使用できます。 詳しくは、クエリ高速化ストアレポートインサイトのドキュメントを参照してください [クエリサービスを通じてレポートインサイトデータモデルを作成し、高速化ストアデータとユーザー定義ダッシュボードで使用する方法](../../query-service/data-distiller/customizable-insights/reporting-insights-data-model.md).

>[!NOTE]
>
>Adobe Experience Platform システム全体で「セグメント」という用語が「オーディエンス」に更新されました。 セグメントへの参照の中には、ファイルパスやデータセット命名規則で引き続き使用されるものもあります。

## 前提条件

このガイドでは、について実際に理解している必要があります [ユーザー定義ダッシュボード機能](../user-defined-dashboards.md). このガイドに進む前に、ドキュメントを読んでください。

## Real-Time CDP インサイトレポートとユースケース

Real-Time CDP レポートは、プロファイルデータと、そのオーディエンスおよび宛先との関係に関するインサイトを提供します。 様々なスタースキーマモデルは、様々な一般的なマーケティングユースケースに答えるために開発されたものであり、各データモデルは複数のユースケースをサポートできます。

>[!IMPORTANT]
>
>Real-Time CDP レポートに使用されるデータは、選択した結合ポリシーに関して、および最新の日別スナップショットから正確です。

### プロファイルモデル {#profile-model}

プロファイルモデルは、次の 3 つのデータセットで構成されます。

- `adwh_dim_date`
- `adwh_fact_profile`
- `adwh_dim_merge_policies`

次の画像には、各データセット内の関連データフィールドが含まれています。

![プロファイルモデルの ERD。](../images/cdp-insights/profile-model.png)

#### プロファイル数のユースケース {#profile-count}

に使用されるロジック [!UICONTROL プロファイル数] ウィジェットは、スナップショットが作成された時点でのプロファイルストア内の結合プロファイルの合計数を返します。 を参照してください。 [[!UICONTROL プロファイル数] ウィジェットのドキュメント](../guides/profiles.md#profile-count) を参照してください。

を生成する SQL [!UICONTROL プロファイル数] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name,
       sum(qsaccel.profile_agg.adwh_fact_profile.count_of_profiles) CNT
  FROM qsaccel.profile_agg.adwh_fact_profile
  LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_id=adwh_fact_profile.merge_policy_id
  WHERE qsaccel.profile_agg.adwh_fact_profile.date_key='2024-01-10'
    AND qsaccel.profile_agg.adwh_fact_profile.merge_policy_id = 2027892989
  GROUP BY qsaccel.profile_agg.adwh_dim_merge_policies.merge_policy_name;
```

+++

#### 単一の ID プロファイルのユースケース {#single-identity-profiles}

に使用されるロジック [!UICONTROL 単一の ID プロファイル] ウィジェットは、ID を作成する 1 つのタイプの ID タイプのみを持つ組織のプロファイルの数を提供します。 を参照してください。 [[!UICONTROL 単一の ID プロファイル] ウィジェットのドキュメント](../guides/profiles.md#single-identity-profiles) を参照してください。

を生成する SQL [!UICONTROL 単一の ID プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

### 名前空間モデル {#namespace-model}

名前空間モデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_fact_profile_by_namespace`
- `adwh_dim_merge_policies`
- `adwh_dim_namespaces`

次の画像には、各データセット内の関連データフィールドが含まれています。

![名前空間モデルの ERD。](../images/cdp-insights/namespace-model.png)

#### ID 別プロファイルのユースケース {#profiles-by-identity}

この [!UICONTROL ID 別プロファイル] ウィジェットは、プロファイルストアにあるすべての結合済みプロファイルで ID の分類を表示します。 を参照してください。 [[!UICONTROL ID 別プロファイル] ウィジェットのドキュメント](../guides/profiles.md#profiles-by-identity) を参照してください。

を生成する SQL [!UICONTROL ID 別プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

#### ID ユースケース別の単一の ID プロファイル {#single-identity-profiles-by-identity}

に使用されるロジック [!UICONTROL 単一の ID プロファイル （ID 別）] ウィジェットは、単一の一意の ID のみで識別されるプロファイルの合計数を示します。 を参照してください。 [単一の ID プロファイル （ID ウィジェットドキュメント別）](../guides/profiles.md#single-identity-profiles-by-identity) を参照してください。

を生成する SQL [!UICONTROL 単一の ID プロファイル （ID 別）] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

### オーディエンスモデル {#audience-model}

オーディエンスモデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_fact_profile_by_segment`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセット内の関連データフィールドが含まれています。

![オーディエンスモデルの ERD。](../images/cdp-insights/audience-model.png)

#### オーディエンスサイズのユースケース {#audience-size}

に使用されるロジック [!UICONTROL オーディエンスサイズ] ウィジェットは、最新のスナップショットの時点での、選択したオーディエンス内での結合プロファイルの合計数を返します。 を参照してください。 [[!UICONTROL オーディエンスサイズ] ウィジェットのドキュメント](../guides/audiences.md#audience-size) を参照してください。

を生成する SQL [!UICONTROL オーディエンスサイズ] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

#### オーディエンスサイズ変更トレンドのユースケース {#audience-size-change-trend}

に使用されるロジック [!UICONTROL オーディエンスサイズの変更のトレンド] ウィジェットは、最新の日別スナップショット間の特定のオーディエンスに対して選定されたプロファイルの合計数の違いを示す折れ線グラフを提供します。 を参照してください。 [[!UICONTROL オーディエンスサイズの変更のトレンド] ウィジェットのドキュメント](../guides/audiences.md#audience-size-change-trend) を参照してください。

を生成する SQL [!UICONTROL オーディエンスサイズの変更のトレンド] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

#### 最も使用されている宛先のユースケース {#most-used-destinations}

で使用されるロジック [!UICONTROL 最も使用されている宛先] ウィジェットは、マッピングされたオーディエンスの数に応じて、組織で最も使用されている宛先をリストします。 このランキングは、使用率が低い可能性のある宛先を表示しながら、使用されている宛先に関するインサイトも提供します。 のドキュメントを参照してください。 [[!UICONTROL 最も使用されている宛先] ウィジェット](../guides/destinations.md#most-used-destinations) を参照してください。

を生成する SQL [!UICONTROL 最も使用されている宛先] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

#### 最近アクティブ化されたオーディエンスのユースケース {#recently-activated-audiences}

のロジック [!UICONTROL 最近アクティブ化されたオーディエンス] ウィジェットは、宛先に最近マッピングされたオーディエンスのリストを提供します。 このリストには、システムでアクティブに使用されているオーディエンスと宛先のスナップショットが表示され、誤ったマッピングのトラブルシューティングに役立ちます。 を参照してください。 [[!UICONTROL 最近アクティブ化されたオーディエンス] ウィジェットのドキュメント](../guides/destinations.md#recently-activated-audiences) を参照してください。

を生成する SQL [!UICONTROL 最近アクティブ化されたオーディエンス] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

### 名前空間 – オーディエンスモデル {#namespace-audience-model}

名前空間とオーディエンスのモデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_namespaces`
- `adwh_fact_profile_by_segment_and_namespace`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセット内の関連データフィールドが含まれています。

![名前空間 – オーディエンスモデルの ERD。](../images/cdp-insights/namespace-audience-model.png)

#### オーディエンスのユースケースの ID 別プロファイル {#audience-profiles-by-identity}

で使用されるロジック [!UICONTROL ID 別プロファイル] ウィジェットは、特定のオーディエンスに対して、プロファイルストアにあるすべての結合済みプロファイルをまたいで ID の分類を提供します。 を参照してください。 [[!UICONTROL ID 別プロファイル] ウィジェットのドキュメント](../guides/audiences.md#profiles-by-identity) を参照してください。

を生成する SQL [!UICONTROL ID 別プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

### 名前空間モデルを重複

重複名前空間モデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_overlap_namespaces`
- `adwh_fact_profile_overlap_of_namespace`
- `adwh_dim_merge_policies`

次の画像には、各データセット内の関連データフィールドが含まれています。

![重複名前空間モデルの ERD。](../images/cdp-insights/overlap-namespace-model.png)

#### ID 重複（プロファイル）のユースケース {#profiles-identity-overlap}

で使用されるロジック [!UICONTROL ID の重複] ウィジェットは、のプロファイルの重複を表示します **プロファイルストア** には、選択した 2 つの ID が含まれます。 詳しくは、 [[!UICONTROL ID の重複] のウィジェットセクション [!UICONTROL プロファイル] ダッシュボードのドキュメント](../guides/profiles.md#identity-overlap).

を生成する SQL [!UICONTROL ID の重複] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

### オーディエンスモデルによる重複名前空間 {#overlap-namespace-by-audience-model}

オーディエンスモデルによる重複名前空間は、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_overlap_namespaces`
- `adwh_fact_profile_overlap_of_namespace_by_segment`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセット内の関連データフィールドが含まれています。

![オーディエンスモデルによる重複名前空間の ERD。](../images/cdp-insights/overlap-namespace-by-audience-model.png)

#### ID 重複（オーディエンス）のユースケース {#audiences-identity-overlap}

で使用されるロジック [!UICONTROL オーディエンス] dashboard [!UICONTROL ID の重複] ウィジェットは、特定のオーディエンス用に選択された 2 つの id を含むプロファイルの重複を示します。 詳しくは、 [[!UICONTROL ID の重複] のウィジェットセクション [!UICONTROL オーディエンス] ダッシュボードのドキュメント](../guides/audiences.md#identity-overlap).

を生成する SQL [!UICONTROL ID の重複] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

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

<!-- Commented out as Anil wanted to add something but did not provide information yet:
### Overlap Namespace-Audience model {#overlap-namespace-audience-model}

The overlap namespace-audience model is comprised of the following datasets: 

- `adwh_fact_profile_overlap_by_namespace_and_segment`
- `adwh_dim_date`
- `adwh_dim_namespace`
- `adwh_dim_overlap_namespaces`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

![An ERD of the overlap namespace-audience model.](../images/cdp-insights/overlap-namespace-audience-model.png) -->

<!-- What insights are gathered from this particular data model? -->

<!-- Commented out as Anil wanted to add something but did not provide information yet:
### AI model {#ai-model}

The AI model is comprised of the following datasets: 

- `adwh_fact_profile_ai_models`
- `adwh_dim_date`
- `adwh_dim_merge_policies`
- `adwh_dim_ai_models`

![An ERD of the AI model.](./images/cdp-insights/ai-model.png) -->

<!-- What insights are gathered from this particular data model? -->

