---
title: Real-time Customer Data Platform Insights データモデル
description: Real-time Customer Data Platformインサイトのデータモデルで SQL クエリを使用して、マーケティングおよび KPI の使用例に合わせて独自のReal-Time CDPレポートをカスタマイズする方法を説明します。
exl-id: 61bc7f23-9f79-4c75-a515-85dd9dda2d02
source-git-commit: e55bbba92b0e3b9c86a9962ffa0131dfb7c15e77
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 4%

---

# Real-time Customer Data Platform Insights データモデル

Real-time Customer Data Platformインサイトデータモデル機能では、様々なプロファイル、宛先、セグメント化ウィジェットに関するインサイトを強化するデータモデルと SQL が表示されます。 これらの SQL クエリテンプレートをカスタマイズして、マーケティングおよび主要業績評価指標 (KPI) の使用例に関するReal-Time CDPレポートを作成できます。 ユーザー定義のダッシュボードのカスタムウィジェットとして、これらのインサイトを使用できます。 詳しくは、 Query Accelerated Store reporting Insights のドキュメントを参照してください [クエリサービスを通じてレポートインサイトデータモデルを構築し、高速ストアデータとユーザー定義ダッシュボードで使用する方法](../query-service/data-distiller/query-accelerated-store/reporting-insights-data-model.md).

## 前提条件

このガイドでは、 [ユーザー定義ダッシュボード機能](./user-defined-dashboards.md). このガイドを続行する前に、ドキュメントをお読みください。

## Real-Time CDP Insight レポートと使用例

Real-Time CDPレポートは、プロファイルデータと、そのオーディエンスおよび宛先との関係に関するインサイトを提供します。 様々なスタースキーマモデルが開発され、様々な一般的なマーケティングの使用例に回答しました。また、各データモデルは複数の使用例をサポートできます。

>[!IMPORTANT]
>
>Real-Time CDPのレポートに使用されるデータは、選択した結合ポリシーに対して、また最新の毎日のスナップショットに対して正確です。

### プロファイルモデル {#profile-model}

プロファイルモデルは、次の 3 つのデータセットで構成されます。

- `adwh_dim_date`
- `adwh_fact_profile`
- `adwh_dim_merge_policies`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![プロファイルモデルの ERD。](./images/cdp-insights/profile-model.png)

#### プロファイル数の使用例

プロファイル数ウィジェットに使用されるロジックは、スナップショットが作成された時点でのプロファイルストア内の結合プロファイルの合計数を返します。 詳しくは、 [[!UICONTROL プロファイル数] ウィジェットドキュメント](./guides/profiles.md#profile-count) を参照してください。

を生成する SQL [!UICONTROL プロファイル数] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT adwh_dim_merge_policies.merge_policy_name,
  sum(adwh_fact_profile.count_of_profiles) CNT
FROM qsaccel.profile_agg.adwh_fact_profile
LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON adwh_dim_merge_policies.merge_policy_id=adwh_fact_profile.merge_policy_id
WHERE adwh_fact_profile.date_key='${lastProcessDate}'
AND adwh_fact_profile.merge_policy_id=${mergePolicyId}
GROUP BY adwh_dim_merge_policies.merge_policy_name;
```

+++

#### 単一の ID プロファイルの使用例

に使用されるロジック [!UICONTROL 単一の ID プロファイル] ウィジェットは、id を作成する 1 つのタイプの ID のみを持つ組織のプロファイルの数を提供します。 詳しくは、[[!UICONTROL 単一の ID プロファイル] ウィジェットドキュメント](./guides/profiles.md#single-identity-profiles) を参照してください。

を生成する SQL [!UICONTROL 単一の ID プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT adwh_dim_merge_policies.merge_policy_name,
  sum(adwh_fact_profile.count_of_Single_Identity_profiles) CNT
FROM QSAccel.profile_agg.adwh_fact_profile
LEFT OUTER JOIN QSAccel.profile_agg.adwh_dim_merge_policies ON adwh_dim_merge_policies.merge_policy_id=adwh_fact_profile.merge_policy_id
WHERE adwh_fact_profile.date_key='${lastProcessDate}'
  AND adwh_fact_profile.merge_policy_id =${mergePolicyId}
GROUP BY adwh_dim_merge_policies.merge_policy_name;
```

+++

### 名前空間モデル {#namespace-model}

名前空間モデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_fact_profile_by_namespace`
- `adwh_dim_merge_policies`
- `adwh_dim_namespaces`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![名前空間モデルの ERD。](./images/cdp-insights/namespace-model.png)

#### ID 別プロファイルのユースケース

[!UICONTROL ID 別プロファイル]ウィジェットは、プロファイルストアにあるすべての結合済みプロファイルで ID の分類を表示します。 詳しくは、 [[!UICONTROL ID 別プロファイル] ウィジェットドキュメント](./guides/profiles.md#profiles-by-identity) を参照してください。

を生成する SQL [!UICONTROL ID 別プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT adwh_dim_namespaces.namespace_description,
    sum(adwh_fact_profile_by_namespace.count_of_profiles) count_of_profiles
FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace
JOIN qsaccel.profile_agg.adwh_dim_namespaces ON adwh_fact_profile_by_namespace.namespace_id = adwh_dim_namespaces.namespace_id
AND adwh_fact_profile_by_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
WHERE adwh_fact_profile_by_namespace.merge_policy_id =${mergePolicyId}
AND adwh_fact_profile_by_namespace.date_key = '${lastProcessDate}'
GROUP BY adwh_fact_profile_by_namespace.date_key,
        adwh_fact_profile_by_namespace.merge_policy_id,
        adwh_dim_namespaces.namespace_description
ORDER BY count_of_profiles DESC
LIMIT 5;
```

+++

#### ID 使用例別の単一の ID プロファイル

に使用されるロジック [!UICONTROL ID 別の単一の ID プロファイル] ウィジェットは、1 つの一意の識別子のみで識別されたプロファイルの合計数を示します。 詳しくは、 [ID ウィジェットドキュメントによる単一の ID プロファイル](./guides/profiles.md#single-identity-profiles-by-identity) を参照してください。

を生成する SQL [!UICONTROL ID 別の単一の ID プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT
  adwh_dim_namespaces.namespace_description,
  sum(adwh_fact_profile_by_namespace.count_of_Single_Identity_profiles) count_of_Single_Identity_profiles
FROM
  qsaccel.profile_agg.adwh_fact_profile_by_namespace
  LEFT OUTER JOIN
    qsaccel.profile_agg.adwh_dim_namespaces
    ON adwh_fact_profile_by_namespace.namespace_id = adwh_dim_namespaces.namespace_id
AND adwh_fact_profile_by_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
WHERE
  adwh_fact_profile_by_namespace.merge_policy_id=${mergePolicyId}
  AND adwh_fact_profile_by_namespace.date_key='${lastProcessDate}'
GROUP BY
  adwh_fact_profile_by_namespace.date_key,
  adwh_fact_profile_by_namespace.merge_policy_id,
  adwh_dim_namespaces.namespace_description;
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

次の画像には、各データセットの関連するデータフィールドが含まれています。

![オーディエンスモデルの ERD。](./images/cdp-insights/audience-model.png)

#### オーディエンスサイズの使用例

に使用されるロジック [!UICONTROL オーディエンスサイズ] ウィジェットは、最新のスナップショットの時点での、選択したオーディエンス内の結合されたプロファイルの合計数を返します。 詳しくは、 [[!UICONTROL オーディエンスサイズ] ウィジェットドキュメント](./guides/audiences.md#audience-size) を参照してください。

を生成する SQL [!UICONTROL オーディエンスサイズ] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT adwh_fact_profile_by_segment.date_key,
       adwh_dim_merge_policies.merge_policy_name,
       adwh_dim_segments.segment,
       adwh_dim_segments.segment_name,
       sum(adwh_fact_profile_by_segment.count_of_profiles)count_of_profiles
FROM qsaccel.profile_agg.adwh_fact_profile_by_segment
LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_segments ON adwh_fact_profile_by_segment.segment_id = adwh_dim_segments.segment_id
LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON adwh_fact_profile_by_segment.merge_policy_id=adwh_dim_merge_policies.merge_policy_id
WHERE adwh_fact_profile_by_segment.date_key ='${lastProcessDate}'
  AND adwh_fact_profile_by_segment.merge_policy_id=${mergePolicyId}
GROUP BY adwh_fact_profile_by_segment.date_key,
         adwh_dim_merge_policies.merge_policy_name,
         adwh_dim_segments.segment,
         adwh_dim_segments.segment_name
ORDER BY count_of_profiles DESC
LIMIT 20;
```

+++

#### オーディエンスサイズの変更のトレンドの使用例

に使用されるロジック [!UICONTROL オーディエンスサイズの変更の傾向] widget は、最新の日別スナップショット間の特定のオーディエンスについて認定されたプロファイルの合計数の違いを示す線グラフを提供します。 詳しくは、 [[!UICONTROL オーディエンスサイズの変更の傾向] ウィジェットドキュメント](./guides/audiences.md#audience-size-change-trend) を参照してください。

を生成する SQL [!UICONTROL オーディエンスサイズの変更の傾向] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT DISTINCT cast(adwh_dim_segments.create_date AS Date) Date_key, adwh_dim_merge_policies.merge_policy_name,
  count(DISTINCT adwh_dim_segments.segment_id)Segments_Added
FROM qsaccel.profile_agg.adwh_fact_profile_by_segment
JOIN qsaccel.profile_agg.adwh_dim_segments ON adwh_fact_profile_by_segment.segment_id = adwh_dim_segments.segment_id
JOIN qsaccel.profile_agg.adwh_dim_merge_policies ON adwh_fact_profile_by_segment.merge_policy_id=adwh_dim_merge_policies.merge_policy_id
WHERE Cast(adwh_dim_segments.create_date AS date) >= dateadd(DAY, - ${dayRange}, '${lastProcessDate}')
AND adwh_fact_profile_by_segment.merge_policy_id=${mergePolicyId}
GROUP BY cast(adwh_dim_segments.create_date AS date), adwh_dim_merge_policies.merge_policy_name ;
```

+++

#### 最も使用されている宛先の使用例

で使用されるロジック [!UICONTROL 最も使用されている宛先] ウィジェットには、マッピングされているオーディエンスの数に応じて、組織で最も使用されている宛先が表示されます。 このランキングは、使用率が低い可能性のある宛先を表示しながら、使用されている宛先に関するインサイトも提供します。詳しくは、 [[!UICONTROL 最も使用されている宛先] widget](./guides/destinations.md#most-used-destinations) を参照してください。

を生成する SQL [!UICONTROL 最も使用されている宛先] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT
   adwh_dim_destination.destination_name, adwh_dim_destination.destination_id,
   count( distinct adwh_dim_br_segment_destinations.segment_id ) segment_count
FROM
   qsaccel.profile_agg.adwh_dim_destination
   join qsaccel.profile_agg.adwh_dim_br_segment_destinations
 ON
   adwh_dim_destination.destination_id = adwh_dim_br_segment_destinations.destination_id
 WHERE
   adwh_dim_destination.destination_name is not null
 group by
   adwh_dim_destination.destination_name,
   adwh_dim_destination.destination_id
   order by segment_count desc limit 5;
```

+++

#### 最近アクティブ化されたオーディエンスの使用例

のロジック [!UICONTROL 最近アクティブ化されたオーディエンス] ウィジェットは、宛先に最も最近マッピングされたオーディエンスのリストを提供します。 このリストには、システムでアクティブに使用されているオーディエンスと宛先のスナップショットが表示され、誤ったマッピングのトラブルシューティングに役立ちます。 詳しくは、 [[!UICONTROL 最近アクティブ化されたオーディエンス] ウィジェットドキュメント](./guides/destinations.md#recently-activated-audiences) を参照してください。

を生成する SQL [!UICONTROL 最近アクティブ化されたオーディエンス] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT segment_name, segment, destination_name, a.create_time create_time
FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations a
INNER JOIN qsaccel.profile_agg.adwh_dim_segments b ON a.segment_id = b.segment_id
INNER JOIN qsaccel.profile_agg.adwh_dim_destination c ON a.destination_id = c.destination_id
ORDER BY create_time desc, segment LIMIT 5;
```

+++

### 名前空間 — オーディエンスモデル

namespace-audience モデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_namespaces`
- `adwh_fact_profile_by_segment_and_namespace`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![namespace-audience モデルの ERD。](./images/cdp-insights/namespace-audience-model.png)

#### オーディエンスの使用例の ID 別プロファイル

で使用されるロジック [!UICONTROL ID 別プロファイル] ウィジェットは、特定のオーディエンスに対して、プロファイルストア内のすべての結合プロファイルで id を分類します。 詳しくは、 [[!UICONTROL ID 別プロファイル] ウィジェットドキュメント](./guides/audiences.md#profiles-by-identity) を参照してください。

を生成する SQL [!UICONTROL ID 別プロファイル] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT adwh_dim_namespaces.namespace_description,
  sum( adwh_fact_profile_by_segment_and_namespace.count_of_profiles) count_of_profiles
FROM qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace
LEFT OUTER JOIN qsaccel.profile_agg.adwh_dim_namespaces
ON adwh_fact_profile_by_segment_and_namespace.namespace_id = adwh_dim_namespaces.namespace_id
AND adwh_fact_profile_by_segment_and_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
WHERE adwh_fact_profile_by_segment_and_namespace.segment_id = {segment_id}
AND adwh_fact_profile_by_segment_and_namespace.merge_policy_id = {merge_policy_id}
AND adwh_fact_profile_by_segment_and_namespace.date_key = '{date}'
GROUP BY adwh_dim_namespaces.namespace_description;
```

+++

### 重複名前空間モデル

重複名前空間モデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_overlap_namespaces`
- `adwh_fact_profile_overlap_of_namespace`
- `adwh_dim_merge_policies`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![重複名前空間モデルの ERD。](./images/cdp-insights/overlap-namespace-model.png)

#### ID の重複（プロファイル）の使用例

で使用されるロジック [!UICONTROL ID の重複] ウィジェットは、 **プロファイルストア** 選択した 2 つの id を含む 詳しくは、 [[!UICONTROL ID の重複] ウィジェットセクション [!UICONTROL プロファイル] ダッシュボードドキュメント](./guides/profiles.md#identity-overlap).

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
     WHERE adwh_fact_profile_overlap_of_namespace.merge_policy_id = ${mergePolicyId}
       AND adwh_fact_profile_overlap_of_namespace.date_key = '${lastProcessDate}'
       AND adwh_fact_profile_overlap_of_namespace.overlap_id IN
         (SELECT adwh_dim_overlap_namespaces.overlap_id
          FROM qsaccel.profile_agg.adwh_dim_overlap_namespaces
          WHERE adwh_dim_overlap_namespaces.merge_policy_id=${mergePolicyId}
            AND adwh_dim_overlap_namespaces.overlap_namespaces IN ('${namespace1}',
                                                                   '${namespace2}')
          GROUP BY adwh_dim_overlap_namespaces.overlap_id
          HAVING Count(*) > 1)
     UNION ALL SELECT count_of_profiles overlap_col1,
                      0 overlap_col2,
                      0 overlap_count
     FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace
     JOIN qsaccel.profile_agg.adwh_dim_namespaces ON
     adwh_fact_profile_by_namespace.namespace_id = adwh_dim_namespaces.namespace_id
     AND adwh_fact_profile_by_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
     WHERE adwh_fact_profile_by_namespace.merge_policy_id = ${mergePolicyId}
       AND adwh_fact_profile_by_namespace.date_key = '${lastProcessDate}'
       AND adwh_dim_namespaces.namespace_description = '${namespace1}'
     UNION ALL SELECT 0 overlap_col1,
                      count_of_profiles overlap_col2,
                      0 Overlap_count
     FROM qsaccel.profile_agg.adwh_fact_profile_by_namespace
     JOIN qsaccel.profile_agg.adwh_dim_namespaces ON
     adwh_fact_profile_by_namespace.namespace_id = adwh_dim_namespaces.namespace_id
     AND adwh_fact_profile_by_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
     WHERE adwh_fact_profile_by_namespace.merge_policy_id = ${mergePolicyId}
       AND adwh_fact_profile_by_namespace.date_key = '${lastProcessDate}'
       AND adwh_dim_namespaces.namespace_description = '${namespace2}' ) a;
```

+++

### オーディエンスモデル別の重複名前空間

オーディエンスモデル別の重複名前空間は、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_overlap_namespaces`
- `adwh_fact_profile_overlap_of_namespace_by_segment`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![オーディエンスモデル別の重複名前空間の ERD。](./images/cdp-insights/overlap-namespace-by-audience-model.png)

#### ID の重複（オーディエンス）の使用例

で使用されるロジック [!UICONTROL オーディエンス] dashboard [!UICONTROL ID の重複] ウィジェットは、特定のオーディエンスに対して選択された 2 つの ID を含むプロファイルの重複を示します。 詳しくは、 [[!UICONTROL ID の重複] ウィジェットセクション [!UICONTROL オーディエンス] ダッシュボードドキュメント](./guides/audiences.md#identity-overlap).

を生成する SQL [!UICONTROL ID の重複] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT
   Sum(overlap_col1) overlap_col1,
   Sum( overlap_col2) overlap_col2,
   Sum(overlap_count) Overlap_count
FROM
   (
      SELECT
         0 overlap_col1,
         0 overlap_col2,
         Sum(count_of_profiles) Overlap_count
      FROM
         qsaccel.profile_agg.adwh_fact_profile_overlap_of_namespace_by_segment
      WHERE
         adwh_fact_profile_overlap_of_namespace_by_segment.segment_id = $ {segmentId}
         and adwh_fact_profile_overlap_of_namespace_by_segment.merge_policy_id =$ {mergePolicyId}
         and adwh_fact_profile_overlap_of_namespace_by_segment.date_key = '${lastProcessDate}'
         and adwh_fact_profile_overlap_of_namespace_by_segment.overlap_id IN
         (
            SELECT
               adwh_dim_overlap_namespaces.overlap_id
            FROM
               qsaccel.profile_agg.adwh_dim_overlap_namespaces
            WHERE
               adwh_dim_overlap_namespaces.merge_policy_id =$ {mergePolicyId}
               AND adwh_dim_overlap_namespaces.overlap_namespaces IN
               (
                  '${namespace1}',
                  '${namespace2}'
               )
            GROUP BY
               adwh_dim_overlap_namespaces.overlap_id
            HAVING
               Count(*) > 1
         )
      UNION ALL
      SELECT
         count_of_profiles overlap_col1,
         0 overlap_col2,
         0 Overlap_count
      FROM
         qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace
         LEFT OUTER JOIN
            qsaccel.profile_agg.adwh_dim_namespaces
            ON adwh_fact_profile_by_segment_and_namespace.namespace_id = adwh_dim_namespaces.namespace_id
            and adwh_fact_profile_by_segment_and_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
      WHERE
         adwh_dim_namespaces.namespace_description = '${namespace1}'
         and adwh_fact_profile_by_segment_and_namespace.segment_id = $ {segmentId}
         and adwh_fact_profile_by_segment_and_namespace.merge_policy_id =$ {mergePolicyId}
         and adwh_fact_profile_by_segment_and_namespace.date_key = '${lastProcessDate}'
      UNION ALL
      SELECT
         0 overlap_col1,
         count_of_profiles overlap_col2,
         0 Overlap_count
      FROM
         qsaccel.profile_agg.adwh_fact_profile_by_segment_and_namespace
         LEFT OUTER JOIN
            qsaccel.profile_agg.adwh_dim_namespaces
            ON adwh_fact_profile_by_segment_and_namespace.namespace_id = adwh_dim_namespaces.namespace_id
            and adwh_fact_profile_by_segment_and_namespace.merge_policy_id = adwh_dim_namespaces.merge_policy_id
      WHERE
         adwh_dim_namespaces.namespace_description = '${namespace2}'
         and adwh_fact_profile_by_segment_and_namespace.segment_id = $ {segmentId}
         and adwh_fact_profile_by_segment_and_namespace.merge_policy_id =$ {mergePolicyId}
         and adwh_fact_profile_by_segment_and_namespace.date_key = '${lastProcessDate}'
   )
   a;
```

+++
