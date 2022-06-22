---
title: 派生属性
description: 派生属性を使用すると、通常のケイデンスで属性を計算でき、必要に応じて、これらの派生属性をプロファイル属性としてリアルタイム顧客プロファイルに公開できます。 このドキュメントでは、クエリサービスを使用して、プロファイルデータで使用する派生属性を作成する方法の概要を説明します。
source-git-commit: 72e157e0a6310ebf2f55205b03b60600a56e3cf6
workflow-type: tm+mt
source-wordcount: '1650'
ht-degree: 7%

---

# 派生属性

派生属性を使用すると、通常のケイデンスで属性を計算でき、必要に応じて、これらの派生属性をプロファイル属性としてリアルタイム顧客プロファイルに公開できます。

十分な数のデータを使用して作成された派生属性など、プロファイルデータを分析する様々な使用例に必要な派生属性。 デシルデータを使用すると、特定の属性のパーセンタイル値またはランクに基づいて、セグメントからオーディエンスを作成できます。 例えば、次のような使用例が考えられます。

* チャネル別の視聴数に基づく、購読者の最低 10%を識別する。 これにより、マーケターは、特定のオーディエンスをターゲティングし、新しい購読者パッケージを販売できます。
* 旅行マイル数と「チラシ」ステータスを基に、チラシの上位 10%にいるオーディエンスを識別する。 このオーディエンスは、新しいクレジットカードオファーの販売を選択的にターゲット設定するために使用できます。

## はじめに

この概要では、 [Platform API 呼び出し](../landing/api-guide.md) Adobe Experience Platformの以下の構成要素

* [リアルタイム顧客プロファイルの概要](../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [スキーマ構成の基本](../xdm/schema/composition.md):Experience Data Model(XDM) スキーマと、スキーマを構成するための構成要素、原則およびベストプラクティスの紹介です。
* [リアルタイム顧客プロファイルのスキーマを有効にする方法](../profile/tutorials/add-profile-data.md):このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順について説明します。

## 派生属性の SQL サポート

特定のディメンション（カテゴリ）と対応する指標（売上高、ポイント、閲覧期間など）に基づいてデシルのランクを定義するには、デシルのグループ化を可能にするスキーマを設計する必要があります。 このスキーマは、大きなプロファイルスキーマの一部として使用できます。

### プロファイルスキーマ用の ID 名前空間の作成 {#identity-namespace}

ID 名前空間は、ID が関連するコンテキストのインジケーターとして機能する [ID サービス](../identity-service/home.md)のコンポーネントです。完全修飾 ID には、ID 値と名前空間が含まれます。プロファイルフラグメント間でレコードデータを照合および結合する場合、ID 値と名前空間の両方が一致する必要があります。

ID に関連して作成されたデータセットは、データグループとしてグループ化でき、データのライフサイクルを維持するのに役立ちます。 カスタム名前空間は、 [ID サービス API](../identity-service/api/create-custom-namespace.md) または UI を使用します。 詳しくは、 [カスタム名前空間の管理ドキュメント](../identity-service/namespaces.md#manage-namespaces) を参照してください。

プライマリ ID 記述子は、スキーマ UI のフィールドに割り当てることも、スキーマレジストリ API を使用して作成することもできます。 手順については、ドキュメントを参照してください。 [Adobe Experience Platform UI での id フィールドの定義](../xdm/ui/fields/identity.md#define-an-identity-field)または [スキーマレジストリ API](../xdm/api/descriptors.md#create).

また、クエリサービスを使用すると、アドホックスキーマデータセットフィールドの ID またはプライマリ ID を SQL を介して直接設定できます。 詳しくは、 [アドホックスキーマ id でのセカンダリ id とプライマリ id の設定](./data-governance/ad-hoc-schema-identities.md) を参照してください。

## 派生属性の作成

このドキュメントで提供されるクエリ例では、デシルに焦点を当てて、大きなデータセットを分類し、値の大きい方から小さい方（またはその逆）にデータをランク付けし、期間に基づいてフィルタリングします。

### デシル {#deciles}

十分位数とは、ランク付けされたデータのセットを 10 個の等しい部分に分割する方法です。 データがデシルに分割されると、デシルのランクがデータセット内の各行に割り当てられます。 これにより、データを降順または昇順に並べ替えることができます。

十分位数のランクでは、データを最低から最高の順に並べ、1～10 のスケールで行います。連続する各数は、10%ポイントの増加に対応します。

デシルグループは、ランク付けされたグループの数を表し、データセット内のディメンション（カテゴリ）にランキングを割り当てるために使用されます。 バケットには、各パーティションの正の整数値に評価される数値または式を指定できます。 バケットに null 値を含めることはできません。

四分位数は、分布を 4 で割り、百分位数は 100 で割るために使用されます。

### 十分位数グループのスキーマの作成 {#create-schema}

十分位数のバケット用に作成されたスキーマは、次の 3 つの整数部分で構成されます。データタイプ、（ソースデータセットごとの）データタイプのフィールドグループ、プライマリ id フィールドを指定します。

>[!NOTE]
>
>スキーマを作成する前に、ID 名前空間を作成する必要があります。 詳しくは、 [id 名前空間](#identity-namespace) 」の節を参照してください。

Adobeは、XDM 個別プロファイルや XDM ExperienceEvent など、いくつかの標準（「コア」）XDM クラスを提供します。 これらのコアクラスに加えて、独自のカスタムクラスを作成して、組織の具体的な使用例を説明することもできます。 方法に関するガイドを参照してください [UI でのスキーマの作成と編集](../xdm/ui/resources/schemas.md#create) または [スキーマレジストリ API のスキーマエンドポイント](../xdm/api/schemas.md#create) を参照してください。

リアルタイム顧客プロファイルでの使用を目的として Experience Platform に取り込むデータは、プロファイルで有効な Experience Data Model（XDM）スキーマに一致している必要があります。プロファイルでスキーマを有効にするには、XDM の個別プロファイルまたは XDM ExperienceEvent クラスのいずれかを実装する必要があります。

以下が可能です。 [スキーマレジストリ API を使用して、リアルタイム顧客プロファイルでスキーマの使用を有効にする](../xdm/tutorials/create-schema-api.md) または [スキーマエディターのユーザーインターフェイス](../xdm/tutorials/create-schema-ui.md).  プロファイルのスキーマを有効にする方法に関する詳細な手順は、それぞれのドキュメントで参照できます。

### データタイプの作成 {#create-data-type}

データ型は、クラスまたはスキーマフィールドグループの参照型フィールドとして使用され、スキーマの任意の場所に含めることができる複数フィールド構造を一貫して使用できます。 データタイプの作成は、デシル関連のすべてのフィールドグループで再利用できるので、サンドボックスごとに 1 回限りの手順です。

手順については、ドキュメントを参照してください。 [カスタムデータ型の定義](../xdm/api/data-types.md) の使用 [スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/).

### 十分位フィールドグループの作成 {#create-field-group}

フィールドグループの作成は、サンドボックスごとに 1 回限りの手順です。 また、デシル関連のすべてのスキーマでも再利用できます。

手順については、ドキュメントを参照してください。 [UI を使用したフィールドグループの作成](../xdm/ui/resources/field-groups.md#create)

## クエリサービスを使用したデシルの作成

クエリサービスは、分類されたデシルを含むデータセットを作成する理想的な方法を提供します。 これをセグメント化サービスと組み合わせて使用し、属性のランクに基づいてオーディエンスを作成できます。

次の例に示す概念は、カテゴリ（ディメンション）が定義され、指標が使用可能である限り、他のデシルバケットデータセットを作成するために適用できます。 この例は、航空会社のロイヤリティスキームのデータに基づいています。 航空会社のロイヤリティデータは、エクスペリエンスイベントクラスを利用します。このクラスでは、各イベントが **マイレージ**、クレジットまたは引き落としのどちらか、およびメンバーシップ **ロイヤルティステータス** 「チラシ」、「頻繁」、「シルバー」、「ゴールド」のいずれかです。 プライマリ ID フィールドは、 `membershipNumber`.

### クエリテンプレートの作成 {#create-a-query-template}

テンプレートは、UI のクエリエディターを使用して作成するか、 [クエリサービス API](./api/query-templates.md#create-a-query-template).

以下に示すクエリテンプレートのセクションを詳しく調べます。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/query/query-templates \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
         "name": "Airline Loyalty Deciles Insert into profile",
         "sql":
            "WITH summed_miles_1 AS (
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
                LEFT JOIN map_6 ON  (all_memberships.membershipNumber = map_6.membershipNumber)"
        }'
```

#### ルックバック期間

decile のデータタイプには、1、3、6、9、12 および全期間のルックバックのグループが含まれます。 クエリでは 1、3、6 か月のルックバック期間を使用するので、各ルックバック期間の一時テーブルを作成するために、各セクションには「繰り返し」クエリがいくつか含まれます。

>[!NOTE]
>
>ソースデータに、ルックバック期間の決定に使用できる列がない場合、すべての decile クラスのランク付けは、 `decileMonthAll`.

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

### クエリテンプレートを実行する

クエリは次のことができます [UI を使用して実行されます](./ui/user-guide.md#executing-queries) または [クエリサービス API](./api/queries.md#create-a-query).

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/query/queries \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "dbName": "prod:all",
    "templateId": "{{airline_decile_query_template_id}}",
    "insertIntoParameters": {
        "datasetName": "airline_loyalty_decile"
    }
}'
```

デシルが使用されるので、クエリの結果では、ランキング番号とパーセンタイルの相関関係が保証されます。 各ランクは 10%と等しいので、上位 30%に基づいてオーディエンスを識別する場合は、1、2、3 のランクをターゲットにするだけで済みます。

## 次の手順

セグメント化サービスは、これらのデシルバケットに基づいてオーディエンスを生成できるユーザーインターフェイスと RESTful API を提供します。 詳しくは、 [セグメント化サービスの概要](../segmentation/home.md) を参照してください。

セグメントビルダー（セグメント化サービスの UI 実装）でセグメントを作成して使用する方法について詳しくは、 [セグメントビルダーガイド](../segmentation/ui/overview.md).

API を使用したセグメント定義の作成について詳しくは、[API を使用したオーディエンスセグメントの作成](../segmentation/tutorials/create-a-segment.md)に関するチュートリアルを参照してください。
