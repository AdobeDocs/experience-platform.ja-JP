---
title: 顧客データプラットフォーム (CDP) インサイトデータモデル
description: CDP インサイトデータモデルの SQL クエリを使用して、マーケティングおよび KPI の使用例に合わせて独自の CDP レポートをカスタマイズする方法について説明します。
exl-id: 61bc7f23-9f79-4c75-a515-85dd9dda2d02
source-git-commit: 2c96bfd2c1b541d30a72fcf2bac414ee06607456
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 0%

---

# 顧客データプラットフォーム (CDP) インサイトデータモデル

顧客データプラットフォーム (CDP) インサイトデータモデル機能は、データモデルと SQL を公開し、様々なプロファイル、宛先、セグメント化ウィジェットに関するインサイトを強化します。 これらの SQL クエリテンプレートをカスタマイズして、マーケティングおよび KPI（主要業績評価指標）の使用例に関する CDP レポートを作成できます。 これらのインサイトは、ユーザー定義のダッシュボードのカスタムウィジェットとして使用できます。

## 前提条件

このガイドでは、 [ユーザー定義ダッシュボード機能](./user-defined-dashboards.md). このガイドを続行する前に、ドキュメントをお読みください。

## CDP インサイトレポートと使用例

CDP レポートは、プロファイルデータと、セグメントおよび宛先との関係に関するインサイトを提供します。 様々なスタースキーマモデルが開発され、様々な一般的なマーケティングの使用例に回答しました。また、各データモデルは複数の使用例をサポートできます。

>[!IMPORTANT]
>
>CDP レポートに使用されるデータは、選択した結合ポリシーと、最新の毎日のスナップショットに対して正確です。

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

- `adwh_fact_profile_by_namespace`
- `adwh_dim_date`
- `adwh_dim_namespaces`
- `adwh_dim_merge_policies`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![名前空間モデルの ERD。](./images/cdp-insights/namespace-model.png)

#### ID 別プロファイルのユースケース

この [!UICONTROL ID 別プロファイル] ウィジェットは、プロファイルストア内のすべての結合済みプロファイルで id の分類を表示します。 詳しくは、 [[!UICONTROL ID 別プロファイル] ウィジェットドキュメント](./guides/profiles.md#profiles-by-identity) を参照してください。

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

### セグメントモデル {#segment-model}

セグメントモデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_fact_profile_by_segment`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![セグメントモデルの ERD。](./images/cdp-insights/segment-model.png)

#### オーディエンスサイズの使用例

に使用されるロジック [!UICONTROL オーディエンスサイズ] ウィジェットは、最新のスナップショットの時点で選択したセグメント内で結合されたプロファイルの合計数を返します。 詳しくは、 [[!UICONTROL オーディエンスサイズ] ウィジェットドキュメント](./guides/segments.md#audience-size) を参照してください。

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

に使用されるロジック [!UICONTROL オーディエンスサイズの変更の傾向] widget は、最新の日別スナップショット間の特定のセグメントについて認定されたプロファイルの合計数の違いを示す線グラフを提供します。 詳しくは、 [[!UICONTROL オーディエンスサイズの変更の傾向] ウィジェットドキュメント](./guides/segments.md#audience-size-change-trend) を参照してください。

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

で使用されるロジック [!UICONTROL 最も使用されている宛先] ウィジェットには、マッピングされたセグメントの数に応じて、組織で最も使用されている宛先がリストされます。 このランキングは、どの宛先が利用されているかに関するインサイトを提供すると共に、利用率が低い可能性のある宛先も示します。 詳しくは、 [[!UICONTROL 最も使用されている宛先] widget](./guides/destinations.md#most-used-destinations) を参照してください。

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

#### 最近アクティブにしたセグメントの使用例

のロジック [!UICONTROL 最近アクティブ化されたセグメント] widget は、宛先に最近マッピングされたセグメントのリストを提供します。 このリストには、システムでアクティブに使用されているセグメントと宛先のスナップショットが表示され、誤ったマッピングのトラブルシューティングに役立ちます。 詳しくは、 [[!UICONTROL 最近アクティブ化されたセグメント] ウィジェットドキュメント](./guides/destinations.md#recently-activated-segments) を参照してください。

を生成する SQL [!UICONTROL 最近アクティブ化されたセグメント] ウィジェットは、下の折りたたみ可能なセクションに表示されます。

+++SQL クエリ

```sql
SELECT segment_name, segment, destination_name, a.create_time create_time
FROM qsaccel.profile_agg.adwh_dim_br_segment_destinations a
INNER JOIN qsaccel.profile_agg.adwh_dim_segments b ON a.segment_id = b.segment_id
INNER JOIN qsaccel.profile_agg.adwh_dim_destination c ON a.destination_id = c.destination_id
ORDER BY create_time desc, segment LIMIT 5;
```

+++

### Namespace-segment モデル

名前空間セグメントモデルは、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_merge_policies`
- `adwh_dim_namespaces`
- `adwh_fact_profile_by_segment_and_namespace`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![セグメントモデルの ERD。](./images/cdp-insights/namespace-segment-model.png)

#### セグメントの使用例に対する ID 別プロファイル

で使用されるロジック [!UICONTROL ID 別プロファイル] ウィジェットは、特定のセグメントに対して、プロファイルストア内のすべての結合プロファイルで id を分類します。 詳しくは、 [[!UICONTROL ID 別プロファイル] ウィジェットドキュメント](./guides/segments.md#profiles-by-identity) を参照してください。

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
- `adwh_dim_namespaces`
- `adwh_fact_profile_overlap_of_namespace`
- `adwh_dim_merge_policies`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![セグメントモデルの ERD。](./images/cdp-insights/overlap-namespace-model.png)

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

### セグメントモデル別の Overlap Namespace

セグメントモデル別の重複名前空間は、次のデータセットで構成されます。

- `adwh_dim_date`
- `adwh_dim_namespaces`
- `adwh_fact_profile_overlap_of_namespace_by_segment`
- `adwh_dim_merge_policies`
- `adwh_dim_segments`
- `adwh_dim_br_segment_destinations`
- `adwh_dim_destination`
- `adwh_dim_destination_platform`

次の画像には、各データセットの関連するデータフィールドが含まれています。

![セグメントモデルの ERD。](./images/cdp-insights/overlap-namespace-by-segment-model.png)

#### ID の重複（セグメント）の使用例

で使用されるロジック [!UICONTROL セグメント] dashboard [!UICONTROL ID の重複] ウィジェットは、特定のセグメントに対して選択された 2 つの id を含むプロファイルの重複を示します。 詳しくは、 [[!UICONTROL ID の重複] ウィジェットセクション [!UICONTROL セグメント化] ダッシュボードドキュメント](./guides/segments.md#identity-overlap).

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
