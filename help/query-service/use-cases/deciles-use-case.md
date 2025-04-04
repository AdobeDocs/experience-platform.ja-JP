---
title: デシルベースの派生データセットのユースケース
description: このガイドでは、クエリサービスを使用して、プロファイルデータで使用するデシルベースの派生データセットを作成するために必要な手順を示します。
exl-id: 0ec6b511-b9fd-4447-b63d-85aa1f235436
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 2%

---

# デシルベースの派生データセットのユースケース

派生データセットを使用すると、データレイクからデータを分析するための複雑なユースケースが容易になります。これらのユースケースは、他のダウンストリーム Experience Platform サービスで使用したり、リアルタイム顧客プロファイルデータに公開したりできます。

このユースケースは、リアルタイム顧客プロファイルデータで使用するデシルベースの派生データセットを作成する方法を示しています。 このガイドでは、航空会社のロイヤルティシナリオを例として使用し、カテゴリデシルを使用してランク付けされた属性に基づいてオーディエンスをセグメント化および作成するデータセットの作成方法について説明します。

次の主要な概念を図解しました。

* デシルバケット化のスキーマ作成。
* 分類デシルの作成。
* 複雑な派生データセットの作成。
* ルックバック期間中の十分位数の計算。
* 集計、ランキング、一意の ID の追加を示すクエリの例を示し、これらのデシルバケットに基づいてオーディエンスを生成できるようにします。

## はじめに

このガイドでは、[ クエリサービスでのクエリの実行 ](../best-practices/writing-queries.md) およびAdobe Experience Platformの次のコンポーネントについて、実際に理解している必要があります。

* [ リアルタイム顧客プロファイルの概要 ](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [ スキーマ構成の基本 ](../../xdm/schema/composition.md)：エクスペリエンスデータモデル（XDM）スキーマの概要と、スキーマを構成するための構成要素、原則およびベストプラクティスについて説明します。
* [ リアルタイム顧客プロファイルのスキーマを有効にする方法 ](../../profile/tutorials/add-profile-data.md)：このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順の概要を説明します。
* [ カスタムデータタイプの定義方法 ](../../xdm/api/data-types.md)：データタイプは、クラスまたはスキーマフィールドグループで参照タイプフィールドとして使用され、スキーマの任意の場所に含めることができる複数フィールド構造を一貫して使用できるようにします。

## 目標

このドキュメントで示す例では、航空会社のロイヤルティスキーマからデータをランク付けするために、派生データセットを構築するためにデシルを使用します。 派生データセットを使用すると、選択したカテゴリの上位「n」 % に基づいてオーディエンスを識別することで、データのユーティリティを最大化できます。

## デシルベースの派生データセットの作成

特定のディメンションと対応する指標に基づいてデシルのランキングを定義するには、デシルバケット化を可能にするようにスキーマを設計する必要があります。

このガイドでは、航空会社のロイヤルティデータセットを使用して、クエリサービスを使用して、様々なルックバック期間で実行されたマイルに基づいてデシルを作成する方法を示します。

## クエリサービスを使用したデシルの作成

クエリサービスを使用すると、カテゴリデシルを含んだデータセットを作成できます。このデータセットをセグメント化して、属性ランキングに基づいてオーディエンスを作成できます。 次の例に示す概念は、カテゴリが定義され、指標が使用可能である限り、他のデシルバケットデータセットを作成するために適用できます。

航空会社のロイヤルティデータの例では、[XDM ExperienceEvents クラス ](../../xdm/classes/experienceevent.md) を使用しています。 各イベントは、クレジットまたはデビットされたマイレージの商取引の記録であり、メンバーシップのロイヤルティステータスは、「フライヤー」、「頻繁な」、「シルバー」、または「ゴールド」です。 プライマリ ID フィールドは `membershipNumber` です。

### サンプルデータセット

この例の最初の航空ロイヤルティデータセットは「航空ロイヤルティデータ」で、次のスキーマがあります。 スキーマのプライマリ ID は `_profilefoundationreportingstg.membershipNumber` です。

![ 航空会社のロイヤルティデータスキーマの図。](../images/use-cases/airline-loyalty-data.png)

**サンプルデータ**

次の表は、この例で使用する `_profilefoundationreportingstg` オブジェクトに含まれるサンプルデータを示しています。 これにより、複雑な派生データセットを作成するためのデシルバケットの使用に関するコンテキストが提供されます。

>[!NOTE]
>
>簡潔にするために、テナント ID `_profilefoundationreportingstg` は、ドキュメント全体の列タイトルとそれに続くメンションで、名前空間の先頭から省略されています。

| `.membershipNumber` | `.emailAddress.address` | `.transactionDate` | `.transactionType` | `.transactionDetails` | `.mileage` | `.loyaltyStatus` |
|---|---|---|---|---|---|---|
| C435678623 | sfeldmark1vr@studiopress.com | 2022-01-01 | STATUS_MILES | 新規メンバー | 5000 | チラシ |
| B789279247 | pgalton32n@barnesandnoble.com | 2022-02-01 | AWARD_MILES | JFK-FRA | 7500 | シルバー |
| B789279247 | pgalton32n@barnesandnoble.com | 2022-02-01 | STATUS_MILES | JFK-FRA | 7500 | シルバー |
| B789279247 | pgalton32n@barnesandnoble.com | 2022-02-10 | AWARD_MILES | FRA-JFK | 5000 | シルバー |
| A123487284 | rritson1zn@sciencedaily.com | 2022-01-07 | STATUS_MILES | 新しいクレジットカード | 10000 | チラシ |

{style="table-layout:auto"}

## デシルデータセットの生成

上記の航空会社のロイヤルティデータでは、`.mileage` 値には、受け取った個々のフライトごとにメンバーが飛行したマイル数が含まれます。 このデータは、ライフタイムルックバックおよび様々なルックバックピリオドにわたるマイル数のデシルを作成するために使用されます。 この目的のために、各ルックバック期間のマップデータタイプのデシルと、`membershipNumber` で割り当てられた各ルックバック期間の適切なデシルを含むデータセットが作成されます。

「航空会社ロイヤルティデシルスキーマ」を作成し、クエリサービスを使用してデシルデータセットを作成します。

![ 「航空会社のロイヤルティデシルスキーマ」の図。](../images/use-cases/airline-loyalty-decile-schema.png)

### リアルタイム顧客プロファイルのスキーマの有効化

リアルタイム顧客プロファイルで使用するためにExperience Platformに取り込むデータは、[ プロファイルに対して有効なエクスペリエンスデータモデル（XDM）スキーマ ](../../xdm/ui/resources/schemas.md) に準拠している必要があります。 プロファイルでスキーマを有効にするには、XDM の個別プロファイルまたは XDM ExperienceEvent クラスのいずれかを実装する必要があります。

[ スキーマレジストリ API または [ スキーマエディターのユーザーインターフェイス ](../../xdm/tutorials/create-schema-api.md) を使用して、リアルタイム顧客プロファイルで使用するスキーマを有効にします ](../../xdm/tutorials/create-schema-ui.md)。  プロファイルのスキーマを有効にする方法について詳しくは、それぞれのドキュメントを参照してください。

次に、すべてのデシル関連フィールドグループで再利用されるデータタイプを作成します。 デシルフィールドグループの作成は、サンドボックスごとに 1 回限りの手順で行います。 また、すべてのデシル関連スキーマで再利用できます。

### ID 名前空間を作成し、プライマリ識別子としてマークします {#identity-namespace}

十分位数で使用するために作成されたスキーマには、プライマリ ID が割り当てられている必要があります。 [Adobe Experience Platform スキーマ UI で ID フィールドを定義 ](../../xdm/ui/fields/identity.md#define-an-identity-field) するか、[ スキーマレジストリ API](../../xdm/api/descriptors.md#create) を使用できます。

また、クエリサービスでは、SQL を通じて、アドホックスキーマデータセットフィールドの ID またはプライマリ ID を直接設定することもできます。 詳しくは、[ アドホックスキーマ ID でのセカンダリ ID とプライマリ ID の設定 ](../data-governance/ad-hoc-schema-identities.md) に関するドキュメントを参照してください。

### ルックバック期間のデシルを計算するためのクエリを作成 {#create-a-query}

次の例は、ルックバック期間にわたってデシルを計算するための SQL クエリを示しています。

テンプレートは、UI のクエリエディターまたは [ クエリサービス API](../api/query-templates.md#create-a-query-template) を使用して作成できます。

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

### クエリレビュー

このサンプルクエリのセクションについては、以下で詳しく説明します。

#### ルックバック期間

デシルデータタイプには、1、3、6、9、12 およびライフタイムルックバックのバケットが含まれます。 クエリは 1、3、6 か月のルックバック期間を使用するので、各セクションには、ルックバック期間ごとに一時テーブルを作成するための「繰り返し」クエリが含まれます。

>[!NOTE]
>
>ルックバック期間の決定に使用できる列がソースデータにない場合、すべてのデシルクラスランキングが `decileMonthAll` の下で実行されます。

#### 集計

デシルバケットを作成する前に、共通テーブル式（CTE）を使用して走行距離を集計します。 これにより、特定のルックバック期間の合計マイルが提供されます。 CTE は一時的に存在し、大きいクエリの範囲内でのみ使用できます。

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

他のルックバック期間のデータを生成するために、テンプレート（`summed_miles_3` と `summed_miles_6`）でブロックが 2 回繰り返され、日付計算が変更されます。

クエリの ID 列、ディメンション列、指標列（それぞれ `membershipNumber`、`loyaltyStatus`、`totalMiles`）に注意することが重要です。

#### ランキング

十分位数を使用すると、カテゴリ別バケット化を実行できます。 ランキング番号を作成するには、`loyaltyStatus` フィールドでグループ化された WINDOW 内で、`10` のパラメーターを指定して `NTILE` 関数を使用します。 その結果、1 から 10 のランキングが得られます。 `WINDOW` の `ORDER BY` 句を `DESC` に設定して、ディメンション内の **最大** 指標に `1` のランキング値が確実に指定されるようにします。

```sql
rankings_1 AS (
    SELECT membershipNumber,
           loyaltyStatus,
           totalMiles,
           NTILE(10) OVER (PARTITION BY loyaltyStatus ORDER BY totalMiles DESC) AS decileBucket
    FROM summed_miles_1
)
```

#### マップの集約

複数のルックバック期間がある場合は、`MAP_FROM_ARRAYS` 関数と `COLLECT_LIST` 関数を使用して、事前にデシルバケットマップを作成する必要があります。 この例のスニペット `MAP_FROM_ARRAYS` は、キー（`loyaltyStatus`）と値（`decileBucket`）の配列のペアを使用してマップを作成します。 `COLLECT_LIST` は、指定された列のすべての値を含む配列を返します。

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
>デシルランキングが有効期間にのみ必要な場合、マップの集計は必要ありません。

#### 一意の ID

すべてのメンバーシップの一意のリストを作成するには、一意の ID （`membershipNumber`）のリストが必要です。

```sql
all_memberships AS (
    SELECT DISTINCT _profilefoundationreportingstg.membershipNumber AS membershipNumber FROM airline_loyalty_data
)
```

>[!NOTE]
>
>デシルランキングが有効期間にのみ必要な場合は、この手順を省略し、最後の手順で `membershipNumber` による集計を行うことができます。

#### すべての一時データをつなぎ合わせる

最後の手順では、すべての一時データをステッチして、フィールドグループ内のデシルの構造と同じフォームにします。

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

ライフタイムデータのみが使用可能な場合、クエリは次のように表示されます。

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

ランク付け番号とパーセンタイルの間の相関関係は、デシルの使用により、クエリ結果で保証されます。 各ランクは 10% に相当するので、上位 30% に基づいてオーディエンスを識別するには、ランク 1、2、3 をターゲットにする必要があります。

### クエリテンプレートの実行

クエリを実行してデシルデータセットを入力します。 クエリをテンプレートとして保存し、一定の頻度で実行するようにスケジュールすることもできます。 テンプレートとして保存した場合は、`table_exists` コマンドを参照する作成および挿入パターンを使用するようにクエリを更新することもできます。 `table_exists` コマンドの使用方法について詳しくは、[SQL 構文ガイド ](../sql/syntax.md#table-exists) を参照してください。

## 次の手順

上記のユースケースでは、デシルベースの派生データセットをリアルタイム顧客プロファイルで使用できるようにする手順について説明しました。 これにより、ユーザーインターフェイスまたは RESTful API を介したセグメント化サービスが、これらのデシルバケットに基づいてオーディエンスを生成できるようになります。 セグメントの作成、評価、アクセス方法について詳しくは、[ セグメント化サービスの概要 ](../../segmentation/home.md) を参照してください。
