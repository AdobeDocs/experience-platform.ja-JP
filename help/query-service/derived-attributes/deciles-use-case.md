---
title: 'デシルベースの派生属性の使用例 '
description: このガイドでは、クエリサービスを使用して、プロファイルデータで使用するデシルベースの派生属性を作成するために必要な手順を示します。
hide: true
hidefromtoc: true
source-git-commit: 4f44e66ee7537e038628a5beb6b3b3bd0760ff1b
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 3%

---

# デシルベースの派生属性の使用例

派生属性は、他のダウンストリーム Platform サービスと共に使用したり、リアルタイム顧客プロファイルデータに公開したりできるデータレイクのデータを分析する複雑な使用例を容易にします。

この使用例では、リアルタイム顧客プロファイルデータで使用する、デシルベースの派生属性を作成する方法を示します。 航空会社のロイヤリティシナリオの例を使用して、このガイドでは、分類デシルを使用してセグメント化し、ランク付け属性に基づいてオーディエンスを作成するデータセットの作成方法について説明します。

次の主要概念を示します。

* デシルのグループ化のスキーマ作成。
* 分類決定者の作成。
* 複雑な派生属性の作成。
* ルックバック期間中のデシルの計算。
* クエリの例を使用して、集計、ランク付け、一意の ID の追加を示し、これらのデシルグループに基づいてオーディエンスを生成できるようにします。

## はじめに

このガイドでは、 [クエリサービスでのクエリの実行](../best-practices/writing-queries.md) Adobe Experience Platformの以下の構成要素

* [リアルタイム顧客プロファイルの概要](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [スキーマ構成の基本](../../xdm/schema/composition.md):Experience Data Model(XDM) スキーマと、スキーマを構成するための構成要素、原則およびベストプラクティスの紹介です。
* [リアルタイム顧客プロファイルのスキーマを有効にする方法](../../profile/tutorials/add-profile-data.md):このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順について説明します。
* [カスタムデータタイプの定義方法](../../xdm/api/data-types.md):データ型は、クラスまたはスキーマフィールドグループの参照型フィールドとして使用され、スキーマの任意の場所に含めることができる複数フィールド構造を一貫して使用できます。

## 目標

このドキュメントで示す例では、デシルを使用して、航空会社のロイヤリティスキーマからランキングデータの派生属性を作成します。 派生属性を使用すると、選択したカテゴリの上位「n」%に基づいてオーディエンスを識別することで、データのユーティリティを最大化できます。

## 十分な値に基づく派生属性の作成

特定のディメンションと対応する指標に基づいてデシルのランクを定義するには、デシルのグループ化を可能にするようにスキーマを設計する必要があります。

このガイドでは、航空会社のロイヤリティデータセットを使用し、様々なルックバック期間に流れるマイル数に基づいて、クエリサービスを使用してデシルを構築する方法を示します。

## クエリサービスを使用したデシルの作成

クエリサービスを使用すると、分類されたデシルを含むデータセットを作成し、それをセグメント化して、属性のランクに基づいてオーディエンスを作成できます。 次の例に示す概念は、カテゴリが定義され、指標が使用可能である限り、他のデシルバケットデータセットを作成するために適用できます。

この例の航空会社のロイヤリティデータでは、 [XDM ExperienceEvents クラス](../../xdm/classes/experienceevent.md). 各イベントは、マイルに関するビジネス取引の記録で、クレジットまたは引き落としが行われ、「チラシ」、「頻繁」、「シルバー」、「ゴールド」のメンバーシップロイヤルティステータスが記録されます。 プライマリ ID フィールドは、 `membershipNumber`.

### サンプルデータセット

この例の最初の航空会社のロイヤリティーデータセットは「航空会社のロイヤリティーデータ」で、次のスキーマを持ちます。 スキーマのプライマリ ID は、 `_profilefoundationreportingstg.membershipNumber`.

![航空会社のロイヤリティデータスキーマの図です。](../images/derived-attributes/deciles-use-case/airline-loyalty-data.png)

**サンプルデータ**

次の表に、 `_profilefoundationreportingstg` オブジェクトは、この例で使用されます。 デシルグループを使用して複雑な派生属性を作成するためのコンテキストを提供します。

>[!NOTE]
>
>簡潔にすると、テスト ID `_profilefoundationreportingstg` は、列タイトルの名前空間の開始と、ドキュメント全体での後続のメンションから除外されています。

| `.membershipNumber` | `.emailAddress.address` | `.transactionDate` | `.transactionType` | `.transactionDetails` | `.mileage` | `.loyaltyStatus` |
|---|---|---|---|---|---|---|
| C435678623 | sfeldmark1vr@studiopress.com | 2022-01-01 | STATUS_MILES | 新しいメンバー | 5,000 | チラシ |
| B789279247 | pgalton32n@barnesandnoble.com | 2022-02-01 | AWARD_MILES | JFK-FRA | 7500 | 銀 |
| B789279247 | pgalton32n@barnesandnoble.com | 2022-02-01 | STATUS_MILES | JFK-FRA | 7500 | 銀 |
| B789279247 | pgalton32n@barnesandnoble.com | 2022-02-10 | AWARD_MILES | FRA-JFK | 5,000 | 銀 |
| A123487284 | rritson1zn@sciencedaily.com | 2022-01-07 | STATUS_MILES | 新しいクレジットカード | 10000 | チラシ |

{style=&quot;table-layout:auto&quot;}

## 十分なデータセットを生成

上記の航空会社のロイヤリティデータでは、 `.mileage` 値には、1 回のフライトでのメンバーのフローマイル数が含まれます。 このデータは、全期間ルックバックおよび様々なルックバック期間を通過したマイル数のデシルを作成するために使用されます。 この目的で、各ルックバック期間のマップデータ型のデシルと、以下に割り当てられた各ルックバック期間の適切なデシルを含むデータセットを作成します。 `membershipNumber`.

クエリサービスを使用して、「Airline Loyalty Decile スキーマ」を作成し、Decile データセットを作成します。

![「航空会社のロイヤリティデシルスキーマ」の図です。](../images/derived-attributes/deciles-use-case/airline-loyalty-decile-schema.png)

### リアルタイム顧客プロファイルのスキーマの有効化

リアルタイム顧客プロファイルで使用するためにExperience Platformに取り込まれるデータは、次に準拠している必要があります。 [プロファイルに対して有効なエクスペリエンスデータモデル (XDM) スキーマ](../../xdm/ui/resources/schemas.md). プロファイルでスキーマを有効にするには、XDM の個別プロファイルまたは XDM ExperienceEvent クラスのいずれかを実装する必要があります。

[スキーマレジストリ API を使用して、リアルタイム顧客プロファイルでスキーマの使用を有効にする](../../xdm/tutorials/create-schema-api.md) または [スキーマエディターのユーザーインターフェイス](../../xdm/tutorials/create-schema-ui.md).  プロファイルのスキーマを有効にする方法に関する詳細な手順は、それぞれのドキュメントで参照できます。

次に、デシール関連のすべてのフィールドグループで再利用するデータ型を作成します。 十分位数フィールドグループの作成は、サンドボックスごとに 1 回限りの手順です。 また、デシル関連のすべてのスキーマでも再利用できます。

### ID 名前空間を作成し、プライマリ識別子としてマークします {#identity-namespace}

デシルで使用するために作成されたスキーマには、プライマリ ID が割り当てられている必要があります。 以下が可能です。 [Adobe Experience Platform Schemas UI で id フィールドを定義する](../../xdm/ui/fields/identity.md#define-an-identity-field)または [スキーマレジストリ API](../../xdm/api/descriptors.md#create).

また、クエリサービスを使用すると、アドホックスキーマデータセットフィールドの ID またはプライマリ ID を SQL を介して直接設定できます。 詳しくは、 [アドホックスキーマ id でのセカンダリ id とプライマリ id の設定](../data-governance/ad-hoc-schema-identities.md) を参照してください。

### ルックバック期間中のデシルを計算するクエリの作成 {#create-a-query}

次の例は、ルックバック期間中にデシールを計算する SQL クエリを示しています。

テンプレートは、UI のクエリエディターを使用して作成するか、 [クエリサービス API](../api/query-templates.md#create-a-query-template).

```sql
CREATE TABLE AS airline_loyality_decile 
{  WITH summed_miles_1 AS (
        SELECT _profilefoundationreportingstg.membershipNumber AS membershipNumber,
            _profilefoundationreportingstg.loyaltyStatus AS loyaltyStatus,
            SUM(_profilefoundationreportingstg.mileage) AS totalMiles
        FROM airline_loyalty_data
        WHERE _profilefoundationreportingstg.transactionDate < (MAKE_DATE(YEAR(CURRENT_DATE), MONTH(CURRENT_DATE), 1) - MAKE_YM_INTERVAL(0, 0))
    GROUP BY 1,2
    ),
    summed_miles_3 AS (
        SELECT _profilefoundationreportingstg.membershipNumber AS membershipNumber,
            _profilefoundationreportingstg.loyaltyStatus AS loyaltyStatus,
            SUM(_profilefoundationreportingstg.mileage) AS totalMiles
        FROM airline_loyalty_data
        WHERE _profilefoundationreportingstg.transactionDate < (MAKE_DATE(YEAR(CURRENT_DATE), MONTH(CURRENT_DATE), 1) - MAKE_YM_INTERVAL(0, 1))
    GROUP BY 1,2
    ),
    summed_miles_6 AS (
        SELECT _profilefoundationreportingstg.membershipNumber AS membershipNumber,
            _profilefoundationreportingstg.loyaltyStatus AS loyaltyStatus,
            SUM(_profilefoundationreportingstg.mileage) AS totalMiles
        FROM airline_loyalty_data
        WHERE _profilefoundationreportingstg.transactionDate < (MAKE_DATE(YEAR(CURRENT_DATE), MONTH(CURRENT_DATE), 1) - MAKE_YM_INTERVAL(0, 4))
    GROUP BY 1,2
    ),
    rankings_1 AS (
        SELECT membershipNumber,
            loyaltyStatus,
            totalMiles,
            NTILE(10) OVER (PARTITION BY loyaltyStatus ORDER BY totalMiles DESC) AS decileBucket
        FROM summed_miles_1
    ),
    rankings_3 AS (
        SELECT membershipNumber,
            loyaltyStatus,
            totalMiles,
            NTILE(10) OVER (PARTITION BY loyaltyStatus ORDER BY totalMiles DESC) AS decileBucket
        FROM summed_miles_3
    ),
    rankings_6 AS (
        SELECT membershipNumber,
            loyaltyStatus,
            totalMiles,
            NTILE(10) OVER (PARTITION BY loyaltyStatus ORDER BY totalMiles DESC) AS decileBucket
        FROM summed_miles_6
    ),
    map_1 AS (
        SELECT membershipNumber,
            MAP_FROM_ARRAYS(COLLECT_LIST(loyaltyStatus), COLLECT_LIST(decileBucket)) AS decileMonth1
        FROM rankings_1
        GROUP BY membershipNumber
    ),
    map_3 AS (
        SELECT membershipNumber,
            MAP_FROM_ARRAYS(COLLECT_LIST(loyaltyStatus), COLLECT_LIST(decileBucket)) AS decileMonth3
        FROM rankings_3
        GROUP BY membershipNumber
    ),
    map_6 AS (
        SELECT membershipNumber,
            MAP_FROM_ARRAYS(COLLECT_LIST(loyaltyStatus), COLLECT_LIST(decileBucket)) AS decileMonth6
        FROM rankings_6
        GROUP BY membershipNumber
    ),
    all_memberships AS (
        SELECT DISTINCT _profilefoundationreportingstg.membershipNumber AS membershipNumber FROM airline_loyalty_data
    )
    SELECT STRUCT(
            all_memberships.membershipNumber AS membershipNumber,
            STRUCT(
                    map_1.decileMonth1 AS decileMonth1,
                    map_3.decileMonth3 AS decileMonth3,
                    map_6.decileMonth6 AS decileMonth6
            ) AS decilesMileage
        ) AS _profilefoundationreportingstg
    FROM all_memberships
        LEFT JOIN map_1 ON  (all_memberships.membershipNumber = map_1.membershipNumber)
        LEFT JOIN map_3 ON  (all_memberships.membershipNumber = map_3.membershipNumber)
        LEFT JOIN map_6 ON  (all_memberships.membershipNumber = map_6.membershipNumber)
    }
```

### クエリのレビュー

クエリ例の各セクションの詳細を以下に示します。

#### ルックバック期間

デシールのデータ型には、1、3、6、9、12 およびライフタイムのルックバックのバケットが含まれます。 クエリでは 1、3、6 ヶ月のルックバック期間を使用するので、各セクションには、各ルックバック期間の一時テーブルを作成するために、「繰り返し」クエリがいくつか含まれます。

>[!NOTE]
>
>ソースデータに、ルックバック期間の判断に使用できる列がない場合、すべての decile クラスのランク付けは、 `decileMonthAll`.

#### 集計

共通のテーブル式 (CTE) を使用して、デシルバケットを作成する前にマイルをまとめて集計します。 これにより、特定のルックバック期間の合計マイル数が示されます。 CTE は一時的に存在し、より大きなクエリの範囲内でのみ使用できます。

```sql
summed_miles_1 AS (
    SELECT _profilefoundationreportingstg.membershipNumber AS membershipNumber,
           _profilefoundationreportingstg.loyaltyStatus AS loyaltyStatus,
           SUM(_profilefoundationreportingstg.mileage) AS totalMiles
    FROM airline_loyalty_data
    WHERE _profilefoundationreportingstg.transactionDate < (MAKE_DATE(YEAR(CURRENT_DATE), MONTH(CURRENT_DATE), 1) - MAKE_YM_INTERVAL(0, 0))
    GROUP BY 1,2
)
```

このブロックは、テンプレート (`summed_miles_3` および `summed_miles_6`) の日付の計算を変更して、他のルックバック期間のデータを生成する必要があります。

クエリの ID、ディメンション、指標の列 (`membershipNumber`, `loyaltyStatus` および `totalMiles` それぞれ )。

#### ランキング

デシルを使用すると、分類グループ化を実行できます。 ランキング番号を作成するには、 `NTILE` 関数は `10` を `loyaltyStatus` フィールドに入力します。 これにより、1 から 10 のランキングが得られます。 を `ORDER BY` 節 `WINDOW` から `DESC` ランキング値を `1` が **greatest** 指標を含める必要があります。

```sql
rankings_1 AS (
    SELECT membershipNumber,
           loyaltyStatus,
           totalMiles,
           NTILE(10) OVER (PARTITION BY loyaltyStatus ORDER BY totalMiles DESC) AS decileBucket
    FROM summed_miles_1
)
```

#### マップの集計

複数のルックバック期間がある場合は、事前に `MAP_FROM_ARRAYS` および `COLLECT_LIST` 関数 サンプルスニペットで、 `MAP_FROM_ARRAYS` キーのペア (`loyaltyStatus`) と値 (`decileBucket`) 配列を使用します。 `COLLECT_LIST` 指定された列のすべての値を持つ配列を返します。

```sql
map_1 AS (
    SELECT membershipNumber,
           MAP_FROM_ARRAYS(COLLECT_LIST(loyaltyStatus), COLLECT_LIST(decileBucket)) AS decileMonth1
    FROM rankings_1
    GROUP BY membershipNumber
)
```

>[!NOTE]
>
>十分位数のランク付けが全期間に対してのみ必要な場合は、マップの集計は必要ありません。

#### 一意の ID

一意の ID のリスト (`membershipNumber`) は、すべてのメンバーシップの一意のリストを作成するために必要です。

```sql
all_memberships AS (
    SELECT DISTINCT _profilefoundationreportingstg.membershipNumber AS membershipNumber FROM airline_loyalty_data
)
```

>[!NOTE]
>
>十分位数のランク付けが全期間に対してのみ必要な場合、この手順を省略し、 `membershipNumber` は、最後の手順で実行できます。

#### すべての一時データを結合

最後の手順は、すべての一時データを結合し、フィールドグループ内のデシルの構造と同じフォームにすることです。

```sql
SELECT STRUCT(
           all_memberships.membershipNumber AS membershipNumber,
           STRUCT(
                map_1.decileMonth1 AS decileMonth1,
                map_3.decileMonth3 AS decileMonth3,
                map_6.decileMonth6 AS decileMonth6
           ) AS decilesMileage
       ) AS _profilefoundationreportingstg
FROM all_memberships
    LEFT JOIN map_1 ON  (all_memberships.membershipNumber = map_1.membershipNumber)
    LEFT JOIN map_3 ON  (all_memberships.membershipNumber = map_3.membershipNumber)
    LEFT JOIN map_6 ON  (all_memberships.membershipNumber = map_6.membershipNumber)
```

ライフタイムデータのみを使用できる場合、クエリは次のように表示されます。

```sql
SELECT STRUCT(
           rankings.membershipNumber AS membershipNumber,
           STRUCT(
                MAP_FROM_ARRAYS(COLLECT_LIST(loyaltyStatus), COLLECT_LIST(decileBucket)) AS decileMonthAll
           ) AS decilesMileage
       ) AS _profilefoundationreportingstg
FROM rankings
GROUP BY rankings.membershipNumber
```

デシルが使用されるので、クエリの結果では、ランキング番号とパーセンタイルの相関関係が保証されます。 各ランクは 10%と等しいので、上位 30%に基づいてオーディエンスを識別する場合は、1、2、3 のランクをターゲットにするだけで済みます。

### クエリテンプレートを実行する

クエリを実行して、デシルのデータセットを設定します。 また、クエリをテンプレートとして保存し、スケジュールを設定して、クエリをケイデンスで実行することもできます。 テンプレートとして保存した場合、クエリを更新して、 `table_exists` コマンドを使用します。 の使用方法の詳細 `table_exists`コマンドは、 [SQL 構文ガイド](../sql/syntax.md#table-exists).

## 次の手順

上記の使用例では、デシル属性をリアルタイム顧客プロファイルで使用できるようにする手順を重点的に説明しています。 これにより、ユーザーインターフェイスまたは RESTful API を介してセグメント化サービスを使用して、これらのデシルバケットに基づいてオーディエンスを生成できます。 詳しくは、 [セグメント化サービスの概要](../../segmentation/home.md) を参照してください。

