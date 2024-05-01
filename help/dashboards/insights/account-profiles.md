---
title: アカウントプロファイルインサイト
description: アカウントプロファイルのインサイトを強化する SQL を確認し、これらのクエリを使用して、顧客と消費者エクスペリエンスをさらに詳しく調べるカスタムインサイトを生成します。
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
source-git-commit: b7875128592b17044b068d8064de082bf00a8309
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# アカウントプロファイルインサイト

[アカウントプロファイル](../../rtcdp/accounts/account-profile-overview.md) は、複数のマーケティングチャネルや組織システムなど、様々なソースからのアカウント情報を統合するために使用されます。 この統合ビューにより、顧客アカウントの包括的な把握が可能になり、B2B マーケティングキャンペーンが強化されます。 データモデルの分析から得られるインサイトにより、Adobe Real-time Customer Data Platform B2B データがよりアクセスしやすく、理解しやすく、意思決定に影響を与えやすくなります。

インサイトを強化する SQL へのアクセスにより、B2B データをより深く理解し、高度にカスタマイズされた再利用可能なインサイトを生成して、顧客アカウント情報をさらに詳しく調べることができます。 既存のReal-Time CDP データモデル SQL をインスピレーションとして使用し、独自のビジネスニーズに合ったクエリを作成することで、生データを新しい実用的なインサイトに変換します。

<!-- Add link to new generate insights with SQL workflow doc after April release.-->

次のインサイトはすべて [アカウントプロファイルダッシュボード](../guides/account-profiles.md) または [カスタムダッシュボード](../user-defined-dashboards.md). を参照してください。 [カスタマイズの概要](../customize/overview.md) ダッシュボードをカスタマイズする方法、または [新しいウィジェットの作成と編集](../customize/custom-widgets.md) ウィジェットライブラリ内および [ユーザー定義ダッシュボード](../user-defined-dashboards.md#create-widget).

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

## 業種別アカウント {#accounts-by-industry}

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

## タイプ別のアカウント {#accounts-by-type}

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

## 人物の役割別の商談 {#opportunities-by-person-role}

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

## 営業案件（収益別） {#opportunities-by-revenue}

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

## 商談（ステータスおよびステージ別） {#opportunities-by-status-and-stage}

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

## 獲得した商談 {#opportunities-won}

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

## 次の手順

このドキュメントでは、アカウントプロファイルダッシュボードのインサイトを生成する SQL と、この分析で解決される一般的な質問について説明しました。 SQL を編集および繰り返して、独自のインサイトを生成できるようになりました。

<!-- Add link above Learn how to [generate insights with SQL](). after April release -->

また、のインサイトを生成する SQL を読んで理解することもできます [プロファイル](./profiles.md), [オーディエンス](./audiences.md)、および [宛先](./destinations.md) ダッシュボード。
