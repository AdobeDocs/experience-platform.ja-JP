---
title: アカウントプロファイルインサイト
description: アカウントプロファイルのインサイトを強化する SQL を確認し、これらのクエリを使用して、顧客と消費者エクスペリエンスをさらに詳しく調べるカスタムインサイトを生成します。
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: a953dd56-7dd8-4cd0-baa0-85f92d192789
source-git-commit: cce576c00823a0c02e4b639f0888a466a5af6a0c
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 0%

---

# アカウントプロファイルインサイト

[ アカウントプロファイル ](../../rtcdp/accounts/account-profile-overview.md) は、複数のマーケティングチャネルや組織システムなど、様々なソースのアカウント情報を統合するために使用されます。 この統合ビューにより、顧客アカウントの包括的な把握が可能になり、B2B マーケティングキャンペーンが強化されます。 データモデルの分析から得られるインサイトにより、Adobe Real-Time CDP B2B データがよりアクセスしやすく、理解しやすく、意思決定に影響を与えやすくなります。

インサイトを強化する SQL へのアクセスにより、B2B データをより深く理解し、高度にカスタマイズされた再利用可能なインサイトを生成して、顧客アカウント情報をさらに詳しく調べることができます。 既存のReal-Time CDP データモデル SQL をインスピレーションとして使用し、独自のビジネスニーズに合ったクエリを作成することで、生データを新しい実用的なインサイトに変換します。

<!-- Add link to new generate insights with SQL workflow doc after April release.-->

次のインサイトはすべて、[ アカウントプロファイルダッシュボード ](../guides/account-profiles.md) または [ カスタムダッシュボード ](../standard-dashboards.md) の一部として使用できます。 ダッシュボードをカスタマイズする方法、またはウィジェットライブラリおよび [ ユーザー定義ダッシュボード ](../customize/custom-widgets.md) で [ 新しいウィジェットの作成と編集 ](../customize/overview.md) を使用する方法については、[ カスタマイズの概要 ](../standard-dashboards.md#create-widget) を参照してください。

## 追加されたアカウントプロファイル {#account-profiles-added}

このインサイトによって回答された質問：

- 特定の期間に追加されたアカウントプロファイルの数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH accounts_by_mm_dd AS
(
          SELECT    d.date_key,
                    COALESCE(Sum(a.counts), 0) AS account_counts
          FROM      adwh_b2b_date d
          LEFT JOIN adwh_fact_account a
          ON        d.date_key = a.accounts_created_date
          WHERE     d.date_key BETWEEN Upper(COALESCE('$START_DATE', '')) AND       Upper(COALESCE('$END_DATE', ''))
          GROUP BY  d.date_key)
SELECT   date_key,
         account_counts
FROM     accounts_by_mm_dd
ORDER BY date_key limit 5000;
```

+++

## 業界別の新しいアカウント {#accounts-by-industry}

このインサイトによって回答された質問：

- アカウントプロファイルが属する上位 5 つの業界は何ですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH rankedindustries AS
(
           SELECT     i.industry,
                      Sum(f.counts)                                   AS total_accounts,
                      Row_number() OVER (ORDER BY Sum(f.counts) DESC) AS industry_rank
           FROM       adwh_fact_account f
           INNER JOIN adwh_dim_industry i
           ON         f.industry_id = i.industry_id
           WHERE      f.accounts_created_date BETWEEN Upper(COALESCE('$START_DATE', '')) AND        Upper(COALESCE('$END_DATE', ''))
           GROUP BY   i.industry )
SELECT
         CASE
                  WHEN industry_rank <= 5 THEN industry
                  ELSE 'Others'
         END                 AS industry_group,
         Sum(total_accounts) AS total_accounts
FROM     rankedindustries
GROUP BY
         CASE
                  WHEN industry_rank <= 5 THEN industry
                  ELSE 'Others'
         END
ORDER BY total_accounts DESC limit 5000;
```

+++

## タイプ別の新しいアカウント {#accounts-by-type}

このインサイトによって回答された質問：

- アカウントのタイプ別のカウント数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT t.account_type,
       Sum(f.counts) AS account_count
FROM   adwh_fact_account f
       JOIN adwh_dim_account_type t
         ON f.account_type_id = t.account_type_id
WHERE  accounts_created_date BETWEEN Upper(Coalesce('$START_DATE', '')) AND
                                     Upper(
                                     Coalesce('$END_DATE', ''))
GROUP  BY t.account_type
LIMIT  5000; 
```

+++

## 追加された商談 {#opportunities-added}

このインサイトによって回答された質問：

- 特定の期間に追加された商談の数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT d.date_key,
       Coalesce(Sum(o.counts), 0) AS opportunity_counts
FROM   adwh_b2b_date d
       LEFT JOIN adwh_fact_opportunity o
              ON d.date_key = o.opportunities_created_date
WHERE  d.date_key BETWEEN Upper(Coalesce('$START_DATE', '')) AND
                          Upper(Coalesce('$END_DATE', ''))
GROUP  BY d.date_key
ORDER  BY d.date_key
LIMIT  5000; 
```

+++

## 人物の役割別の新しい機会 {#opportunities-by-person-role}

このインサイトによって回答された質問：

- オポチュニティ内の様々な役割の相対的なサイズと数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
SELECT p.person_role,
       Sum(f.counts) AS opportunity_counts
FROM   adwh_fact_opportunity_person f
       JOIN adwh_dim_person_role p
         ON f.person_role_id = p.person_role_id
WHERE  f.opportunity_person_created_date BETWEEN
       Upper(Coalesce('$START_DATE', '')) AND Upper(Coalesce('$END_DATE', ''))
GROUP  BY p.person_role
LIMIT  5000; 
```

+++

## 収益別の新しい商談 {#opportunities-by-revenue}

このインサイトによって回答された質問：

- 売上高（米ドル）でランク付けされた上位 20 の商談は何ですか？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH ranked_opportunities AS
(
           SELECT     n.opportunity_name,
                      a.expected_revenue,
                      t.source_type,
                      Row_number() OVER (ORDER BY a.expected_revenue DESC) AS rank
           FROM       adwh_opportunity_amount a
           INNER JOIN adwh_dim_opportunity_name n
           ON         a.name_id = n.name_id
           INNER JOIN adwh_dim_opportunity_source_type t
           ON         n.source_type_id = t.source_type_id
           WHERE      a.opportunity_created_date BETWEEN Upper(COALESCE('$START_DATE', '')) AND        Upper(COALESCE('$END_DATE', ''))
           AND        a.isclosed='false' )
SELECT
         CASE
                  WHEN rank <= 20 THEN opportunity_name
                  ELSE 'Others'
         END                   AS opportunity_name,
         Sum(expected_revenue) AS total_expected_revenue
FROM     ranked_opportunities
GROUP BY
         CASE
                  WHEN rank <= 20 THEN opportunity_name
                  ELSE 'Others'
         END,
         source_type
ORDER BY total_expected_revenue DESC limit 5000;
```

+++

## ステータスおよびステージ別の新しい商談 {#opportunities-by-status-and-stage}

このインサイトによって回答された質問：

- どのようなオープンなオポチュニティがあり、セールスまたはマーケティングファネルのどのステージにいますか？
- クローズされたオポチュニティは何ですか。また、セールスまたはマーケティング・ファネルのどのステージに存在しますか。

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH opportunities_by_isclosed AS
(
         SELECT   f.isclosed,
                  Sum(f.counts)             AS opportunity_counts,
                  COALESCE(s.stage, 'null') AS stage
         FROM     adwh_fact_opportunity f
         JOIN     adwh_dim_opportunity_stage s
         ON       f.stage_id = s.stage_id
         WHERE    opportunities_created_date BETWEEN Upper(COALESCE('$START_DATE', '')) AND      Upper(COALESCE('$END_DATE', ''))
         GROUP BY f.isclosed,
                  s.stage)
SELECT
       CASE
              WHEN isclosed='true' THEN 'Closed'
              ELSE 'Open'
       END AS opportunity_closed,
       stage,
       opportunity_counts
FROM   opportunities_by_isclosed limit 5000;
```

+++

## 獲得済みの新規商談 {#opportunities-won}

このインサイトによって回答された質問：

- 正常にクローズまたは確定された商談の数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH opportunities_by_iswon AS
(
         SELECT   iswon,
                  Sum(counts) AS opportunity_counts
         FROM     adwh_fact_opportunity
         WHERE    opportunities_created_date BETWEEN Upper(COALESCE('$START_DATE', '')) AND      Upper(COALESCE('$END_DATE', ''))
         GROUP BY iswon)
SELECT
       CASE
              WHEN iswon ='true' THEN 'True'
              ELSE 'False'
       END AS opportunity_won,
       opportunity_counts
FROM   opportunities_by_iswon limit 5000;
```

+++

## 獲得商談（折れ線グラフ） {#opportunities-won-line-graph}

<!-- Q) Can we change this name? -->

このインサイトによって回答された質問：

- 特定の期間に正常にクローズまたは確定（受注）した商談の数は？

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH opportunities_won_counts AS
(
         SELECT   opportunities_created_date,
                  Sum(counts) AS opportunities_counts
         FROM     adwh_fact_opportunity
         WHERE    iswon='true'
         AND      opportunities_created_date BETWEEN Upper(COALESCE('$START_DATE', '')) AND      Upper(COALESCE('$END_DATE', ''))
         GROUP BY opportunities_created_date)
SELECT    d.date_key,
          COALESCE(o.opportunities_counts, 0) AS opportunity_won_counts
FROM      adwh_b2b_date d
LEFT JOIN opportunities_won_counts o
ON        d.date_key = o.opportunities_created_date
WHERE     d.date_key BETWEEN Upper(COALESCE('$START_DATE', '')) AND       Upper(COALESCE('$END_DATE', ''))
ORDER BY  d.date_key limit 5000;
```

+++

## アカウントあたりの顧客数の概要 {#customers-per-account-overview}

>[!NOTE]
>
>[!UICONTROL  アカウントごとの顧客 ] グラフには、[!UICONTROL  アカウントごとの顧客 ]、[!UICONTROL  アカウントごとの商談 ]、および [!UICONTROL  アカウントごとの商談 ] という 3 つのドリルスルーインサイトが含まれています。 これらのドリルスルーでは、より詳細なインサイトが提供され、顧客と商談のカウントをカテゴリ（直接顧客と間接顧客など）別や範囲（顧客と商談数のバンドなど）別に分類します。 これらのグラフは、設定したグローバル日付フィルターの影響を受けません。

このインサイトによって回答された質問：

- 顧客の直接的または間接的な有無に基づくアカウントの配分

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH LatestDate AS (SELECT MAX(inserted_date) AS max_inserted_date FROM adwh_b2b_account_person_association),
     CategorizedData AS (
         SELECT CASE 
                    WHEN is_direct = 'true' AND person_count = 0 THEN 'Accounts without Direct Customers' 
                    WHEN is_direct = 'false' AND person_count = 0 THEN 'Accounts without Indirect Customers' 
                    WHEN is_direct = 'true' AND person_count > 0 THEN 'Accounts with Direct Customers' 
                    WHEN is_direct = 'false' AND person_count > 0 THEN 'Accounts with Indirect Customers' 
                END AS Account_Category, 
                account_count 
         FROM adwh_b2b_account_person_association 
         WHERE inserted_date = (SELECT max_inserted_date FROM LatestDate)
     ),
     AggregatedData AS (
         SELECT Account_Category, SUM(account_count) AS Accounts 
         FROM CategorizedData 
         GROUP BY Account_Category
     ),
     AllCategories AS (
         SELECT 'Accounts without Direct Customers' AS Account_Category 
         UNION ALL SELECT 'Accounts without Indirect Customers' 
         UNION ALL SELECT 'Accounts with Direct Customers' 
         UNION ALL SELECT 'Accounts with Indirect Customers'
     )
SELECT ac.Account_Category AS Account_Category, COALESCE(ad.Accounts, 0) AS Accounts 
FROM AllCategories ac 
LEFT JOIN AggregatedData ad ON ac.Account_Category = ad.Account_Category 
ORDER BY ac.Account_Category;
```

+++

## アカウントごとの顧客詳細 {#customers-per-account-detail}

>[!NOTE]
>
>このインサイトは、グローバル日付フィルターの影響を受けません。

このインサイトによって回答された質問：

- 直接顧客と間接顧客の範囲が異なるアカウントの数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH customer_ranges AS (
    SELECT 'Direct Customer' AS customer_type, '1-10 Customers' AS person_range 
    UNION ALL
    SELECT 'Direct Customer', '11-100 Customers' 
    UNION ALL
    SELECT 'Direct Customer', '101-1000 Customers' 
    UNION ALL
    SELECT 'Direct Customer', '1000+ Customers' 
    UNION ALL
    SELECT 'Indirect Customer', '1-10 Customers' 
    UNION ALL
    SELECT 'Indirect Customer', '11-100 Customers' 
    UNION ALL
    SELECT 'Indirect Customer', '101-1000 Customers' 
    UNION ALL
    SELECT 'Indirect Customer', '1000+ Customers'
)
SELECT 
    cr.customer_type, 
    cr.person_range, 
    COALESCE(SUM(ap.account_count), 0) AS Accounts
FROM customer_ranges cr
LEFT JOIN (
    SELECT 
        CASE 
            WHEN is_direct = 'true' THEN 'Direct Customer' 
            ELSE 'Indirect Customer' 
        END AS customer_type,
        CASE 
            WHEN person_count BETWEEN 1 AND 10 THEN '1-10 Customers' 
            WHEN person_count BETWEEN 11 AND 100 THEN '11-100 Customers' 
            WHEN person_count BETWEEN 101 AND 1000 THEN '101-1000 Customers' 
            WHEN person_count > 1000 THEN '1000+ Customers' 
        END AS person_range,
        SUM(account_count) AS account_count
    FROM adwh_b2b_account_person_association 
    WHERE inserted_date = (SELECT MAX(inserted_date) FROM adwh_b2b_account_person_association) 
    GROUP BY 
        CASE 
            WHEN is_direct = 'true' THEN 'Direct Customer' 
            ELSE 'Indirect Customer' 
        END,
        CASE 
            WHEN person_count BETWEEN 1 AND 10 THEN '1-10 Customers' 
            WHEN person_count BETWEEN 11 AND 100 THEN '11-100 Customers' 
            WHEN person_count BETWEEN 101 AND 1000 THEN '101-1000 Customers' 
            WHEN person_count > 1000 THEN '1000+ Customers' 
        END
) ap ON cr.customer_type = ap.customer_type AND cr.person_range = ap.person_range
GROUP BY cr.customer_type, cr.person_range
ORDER BY cr.customer_type, 
    CASE cr.person_range 
        WHEN '1-10 Customers' THEN 1 
        WHEN '11-100 Customers' THEN 2 
        WHEN '101-1000 Customers' THEN 3 
        WHEN '1000+ Customers' THEN 4 
    END;
```

+++

## アカウントごとのオポチュニティの概要 {#opportunities-per-account-overview}

>[!NOTE]
>
>このインサイトは、グローバル日付フィルターの影響を受けません。

このインサイトによって回答された質問：

- 関連するオポチュニティがあるかどうかに基づくアカウントの配分

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH LatestDate AS (
    SELECT MAX(inserted_date) AS max_inserted_date 
    FROM adwh_b2b_account_opportunity_association
),
CategorizedData AS (
    SELECT 
        CASE 
            WHEN opportunity_count = 0 THEN 'Accounts without Opportunities'
            WHEN opportunity_count > 0 THEN 'Accounts with Opportunities'
        END AS Opportunity_Category, 
        account_count 
    FROM adwh_b2b_account_opportunity_association 
    WHERE inserted_date = (SELECT max_inserted_date FROM LatestDate)
),
AggregatedData AS (
    SELECT 
        Opportunity_Category,
        SUM(account_count) AS Accounts 
    FROM CategorizedData 
    GROUP BY Opportunity_Category
),
AllCategories AS (
    SELECT 'Accounts without Opportunities' AS Opportunity_Category 
    UNION ALL 
    SELECT 'Accounts with Opportunities'
)
SELECT 
    ac.Opportunity_Category AS Opportunity_Category, 
    COALESCE(ad.Accounts, 0) AS Accounts 
FROM AllCategories ac 
LEFT JOIN AggregatedData ad 
    ON ac.Opportunity_Category = ad.Opportunity_Category 
ORDER BY ac.Opportunity_Category;
```

+++

## アカウントごとのオポチュニティの詳細 {#opportunities-per-account-detail}

>[!NOTE]
>
>このインサイトは、グローバル日付フィルターの影響を受けません。

このインサイトによって回答された質問：

- 関連するオポチュニティの範囲が異なるアカウントの数

+++選択すると、このインサイトを生成する SQL が表示されます

```sql
WITH opportunity_ranges AS (
    SELECT '1-10 Opportunities' AS opportunity_range 
    UNION ALL 
    SELECT '11-50 Opportunities' 
    UNION ALL 
    SELECT '51-100 Opportunities' 
    UNION ALL 
    SELECT '100+ Opportunities'
)
SELECT opportunity_ranges.opportunity_range AS OPPORTUNITIES, 
       COALESCE(SUM(accounts.total_accounts), 0) AS ACCOUNTS 
FROM opportunity_ranges 
LEFT JOIN (
    SELECT 
        CASE 
            WHEN opportunity_count BETWEEN 1 AND 10 THEN '1-10 Opportunities' 
            WHEN opportunity_count BETWEEN 11 AND 50 THEN '11-50 Opportunities' 
            WHEN opportunity_count BETWEEN 51 AND 100 THEN '51-100 Opportunities' 
            WHEN opportunity_count > 100 THEN '100+ Opportunities' 
        END AS opportunity_range, 
        SUM(account_count) AS total_accounts 
    FROM adwh_b2b_account_opportunity_association 
    WHERE inserted_date = (SELECT MAX(inserted_date) FROM adwh_b2b_account_opportunity_association) 
      AND opportunity_count > 0 
    GROUP BY 
        CASE 
            WHEN opportunity_count BETWEEN 1 AND 10 THEN '1-10 Opportunities' 
            WHEN opportunity_count BETWEEN 11 AND 50 THEN '11-50 Opportunities' 
            WHEN opportunity_count BETWEEN 51 AND 100 THEN '51-100 Opportunities' 
            WHEN opportunity_count > 100 THEN '100+ Opportunities' 
        END
) AS accounts ON opportunity_ranges.opportunity_range = accounts.opportunity_range 
GROUP BY opportunity_ranges.opportunity_range 
ORDER BY CASE opportunity_ranges.opportunity_range 
            WHEN '1-10 Opportunities' THEN 1 
            WHEN '11-50 Opportunities' THEN 2 
            WHEN '51-100 Opportunities' THEN 3 
            WHEN '100+ Opportunities' THEN 4 
        END;
```

+++

## 次の手順

このドキュメントでは、アカウントプロファイルダッシュボードのインサイトを生成する SQL と、この分析で解決される一般的な質問について説明しました。 SQL を編集および繰り返して、独自のインサイトを生成できるようになりました。 SQL を使用してカスタムインサイトを生成する方法については、[Query Pro モードの概要 ](../sql-insights-query-pro-mode/overview.md) を参照してください。

また、[Profiles](./profiles.md)、[Audiences](./audiences.md) および [Destinations](./destinations.md) ダッシュボードのインサイトを生成する SQL を読み、理解することもできます。
