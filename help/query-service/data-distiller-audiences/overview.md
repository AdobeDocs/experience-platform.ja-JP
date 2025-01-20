---
title: SQL を使用したオーディエンスの構築
description: Adobe Experience Platformの Data Distillerで SQL オーディエンス拡張機能を使用して、SQL コマンドを使用してオーディエンスを作成、管理および公開する方法を説明します。 このガイドでは、プロファイルの作成、更新、削除、ファイルベースの宛先をターゲットにするためのデータ駆動型オーディエンス定義の使用など、オーディエンスのライフサイクルのすべての側面について説明します。
exl-id: c35757c1-898e-4d65-aeca-4f7113173473
source-git-commit: c66a7cf779c1b6e55ace86916985087dfaa3363b
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 1%

---

# SQL を使用したオーディエンスの構築

SQL オーディエンス拡張機能を使用して、既存のディメンションエンティティ（顧客属性や製品情報など）を含む、データレイクからのデータでオーディエンスを作成します。

この SQL 拡張機能を使用すると、オーディエンスセグメントを定義する際にプロファイルに生のデータを必要としないので、オーディエンスを作成する機能が向上します。 この方法を使用して作成されたオーディエンスは、オーディエンスワークスペースに自動的に登録され、ファイルベースの宛先をさらにターゲット設定できます。

![SQL オーディエンス拡張機能のワークフローを示すインフォグラフィック。 段階としては、SQL コマンドを使用してクエリサービスでオーディエンスを作成し、Platform UI で管理して、ファイルベースの宛先でアクティブ化する ](../images/data-distiller/sql-audiences/sql-audience-extension-workflow.png) が含まれます。

このドキュメントでは、Adobe Experience Platformの Data Distillerの SQL オーディエンス拡張機能を使用して、SQL コマンドを使用してオーディエンスを作成、管理および公開する方法について説明します。

## Data Distillerでのオーディエンス作成のライフサイクル {#audience-creation-lifecycle}

オーディエンスを作成、管理およびアクティブ化するには、次の手順に従います。 作成されたオーディエンスは「オーディエンスフロー」にシームレスに統合されるので、ベースオーディエンスとターゲットファイルベースの宛先（CSV アップロードやクラウドストレージの場所など）からセグメントを作成して、顧客アウトリーチに活用できます。 「オーディエンスフロー」とは、オーディエンスを作成、管理およびアクティブ化し、宛先間のシームレスな統合を確保する完全なプロセスを指します。

「オーディエンスフロー」の一部として、次の SQL コマンドを使用してAdobe Experience Platform内のオーディエンスを [ 作成 ](#create-audience)、[ 変更 ](#add-profiles-to-audience) および [ 削除 ](#delete-audience) します。

### オーディエンスの作成 {#create-audience}

`CREATE AUDIENCE AS SELECT` コマンドを使用して、新しいオーディエンスを定義します。 作成したオーディエンスはデータセットに保存され、データDistillerの下の [!UICONTROL  オーディエンス ] ワークスペースに登録されます。

```sql
CREATE AUDIENCE table_name  
WITH (primary_identity='IdentitycolName', identity_namespace='Namespace for the identity used', [schema='target_schema_title'])
AS (select_query)
```

**パラメーター**

これらのパラメーターを使用して、SQL オーディエンス作成クエリを定義します。

| パラメーター | 説明 |
|--------------------|------------------------------------------------------------------|
| `schema` | オプション。クエリによって作成されたデータセットの XDM スキーマを定義します。 |
| `table_name` | テーブルとオーディエンスの名前。 |
| `primary_identity` | オーディエンスのプライマリ ID 列を指定します。 |
| `identity_namespace` | ID の名前空間。 既存の名前空間を使用することも、新しい名前空間を作成することもできます。 使用可能な名前空間を表示するには、`SHOW NAMESPACES` コマンドを使用します。 新しい名前空間を作成するには、`CREATE NAMESPACE` を使用します。 例：`CREATE NAMESPACE lumaCrmId WITH (code='testns', TYPE='Email')`。 |
| `select_query` | オーディエンスを定義する SELECT ステートメント。 SELECT クエリの構文は、[SELECT クエリ ](../sql/syntax.md#select-queries) セクションにあります。 |

{style="table-layout:auto"}

>[!NOTE]
>
>複雑なデータ構造の柔軟性を高めるために、オーディエンスを定義する際にエンリッチメントされた属性をネストできます。 `orders`、`total_revenue`、`recency`、`frequency`、`monetization` などのエンリッチメントされた属性を使用して、必要に応じてオーディエンスをフィルタリングできます。

**例：**

次の例は、SQL オーディエンス作成クエリの構築方法を示しています。

```sql
CREATE Audience aud_test
WITH (primary_identity=userId, identity_namespace=lumaCrmId)
AS SELECT userId, orders, total_revenue, recency, frequency, monetization FROM profile_dim_customer;
```

この例では、`userId` 列が ID 列として識別され、適切な名前空間（`lumaCrmId`）が割り当てられます。 残りの列（`orders`、`total_revenue`、`recency`、`frequency` および `monetization`）は、オーディエンスに追加のコンテキストを提供するエンリッチメントされた属性です。

**制限事項:**

オーディエンス作成に SQL を使用する場合は、次の制限事項に注意してください。

- プライマリ ID 列は、他の属性やカテゴリ内にネストされることなく、データセットの最高レベルに存在します **必須**。
- SQL コマンドを使用して作成された外部オーディエンスの保持期間は 30 日です。 30 日後、これらのオーディエンスは自動的に削除されます。これは、オーディエンス管理戦略を計画する際の重要な考慮事項です。

### 既存オーディエンスへのプロファイルの追加 {#add-profiles-to-audience}

`INSERT INTO` コマンドを使用して、プロファイル（またはオーディエンス全体）を既存のオーディエンスに追加します。

```sql
INSERT INTO table_name
SELECT select_query
```

**パラメーター**

次の表に、`INSERT INTO` のコマンドに必要なパラメーターを示します。

| パラメーター | 説明 |
|----------------|--------------------------------------------------------------------------------|
| `table_name` | オーディエンス作成コマンドの一部として作成されたテーブルの名前。 |
| `select_query` | SELECT 文。 SELECT クエリの構文は、SELECT クエリ セクションにあります。 |

{style="table-layout:auto"}

**例：**

次の例は、`INSERT INTO` コマンドを使用した既存のオーディエンスへのプロファイルの追加方法を示しています。

```sql
INSERT INTO Audience aud_test
SELECT userId, orders, total_revenue, recency, frequency, monetization FROM customer_ds;
```

### RFM モデルオーディエンスの例 {#rfm-model-audience-example}

次の例は、最新性、頻度、収益化（RFM）モデルを使用したオーディエンスの作成方法を示しています。 この例では、リーセンシー、頻度および収益化スコアに基づいて顧客をセグメント化し、常連客、新規顧客、高価値顧客などの主要なグループを識別します。

<!--  Q) Since the focus of this document is on external audiences, or should I just include this temporarily? We could simply provide a link to the separate RFM modeling documentation rather than including the full example here. (Add link to new RFM document when it is published) -->

次のクエリは、RFM オーディエンスのスキーマを作成します。 ステートメントは、`userId`、`days_since_last_purchase`、`orders`、`total_revenue` などの顧客情報を保持するフィールドを設定します。

```sql
CREATE Audience adls_rfm_profile
WITH (primary_identity=userId, identity_namespace=lumaCrmId) AS
SELECT
    cast(NULL AS string) userId,
    cast(NULL AS integer) days_since_last_purchase,
    cast(NULL AS integer) orders,
    cast(NULL AS decimal(18,2)) total_revenue,
    cast(NULL AS integer) recency,
    cast(NULL AS integer) frequency,
    cast(NULL AS integer) monetization,
    cast(NULL AS string) rfm_model
WHERE false;
```

オーディエンスを作成したら、顧客データを入力し、RFM スコアに基づいてプロファイルをセグメント化します。 以下の SQL ステートメントでは、`NTILE(4)` 関数を使用して、RFM （最新性、頻度、収益化）スコアに基づいて顧客を四分位数にランク付けします。 これらのスコアでは、顧客を「コア」、「ロイヤルティ」、「クジラ」などの 6 つのセグメントに分類します。 次に、セグメント化された顧客データがオーディエンスの `adls_rfm_profile` テーブルに挿入されます。」

```sql
INSERT INTO Audience adls_rfm_profile
SELECT
    userId,
    days_since_last_purchase,
    orders,
    total_revenue,
    recency,
    frequency,
    monetization,
    CASE
        WHEN Recency=1 AND Frequency=1 AND Monetization=1 THEN '1. Core - Your Best Customers'
        WHEN Recency IN(1,2,3,4) AND Frequency=1 AND Monetization IN (1,2,3,4) THEN '2. Loyal - Your Most Loyal Customers'
        WHEN Recency IN(1,2,3,4) AND Frequency IN (1,2,3,4) AND Monetization=1 THEN '3. Whales - Your Highest Paying Customers'
        WHEN Recency IN(1,2,3,4) AND Frequency IN(1,2,3) AND Monetization IN(2,3,4) THEN '4. Promising - Faithful Customers'
        WHEN Recency=1 AND Frequency=4 AND Monetization IN (1,2,3,4) THEN '5. Rookies - Your Newest Customers'
        WHEN Recency IN (2,3,4) AND Frequency=4 AND Monetization IN (1,2,3,4) THEN '6. Slipping - Once Loyal, Now Gone'
    END AS rfm_model
FROM (
    SELECT
        userId,
        days_since_last_purchase,
        orders,
        total_revenue,
        NTILE(4) OVER (ORDER BY days_since_last_purchase) AS recency,
        NTILE(4) OVER (ORDER BY orders DESC) AS frequency,
        NTILE(4) OVER (ORDER BY total_revenue DESC) AS monetization
    FROM (
        SELECT
            userid,
            DATEDIFF(current_date, MAX(purchase_date)) AS days_since_last_purchase,
            COUNT(purchaseid) AS orders,
            CAST(SUM(total_revenue) AS double) AS total_revenue
        FROM (
            SELECT DISTINCT
                ENDUSERIDS._EXPERIENCE.EMAILID.ID AS userid,
                commerce.`ORDER`.purchaseid AS purchaseid,
                commerce.`ORDER`.pricetotal AS total_revenue,
                TO_DATE(timestamp) AS purchase_date
            FROM sample_data_for_ootb_templates
            WHERE commerce.`ORDER`.purchaseid IS NOT NULL
        ) AS b
        GROUP BY userId
    )
);
```

### オーディエンスを削除（オーディエンスを削除） {#delete-audience}

既存のオーディエンスを削除するには、`DROP AUDIENCE` コマンドを使用します。 オーディエンスが存在しない場合は、`IF EXISTS` が指定されていない限り例外が発生します。

```sql
DROP AUDIENCE [IF EXISTS] [db_name.]table_name
```

**パラメーター**

テーブルには、`DROP AUDIENCE` のコマンドに必要なパラメーターが含まれています。

| パラメーター | 説明 |
|----------------|----------------------------------------------------------------------------------------|
| `IF EXISTS` | オプション。指定した場合、テーブルが見つからない場合に例外は発生しません。 |
| `db_name` | オーディエンスデータセットの選定に使用するデータグループを指定します。 |
| `table_name` | オーディエンス作成コマンドの一部として作成されたテーブルの名前。 |

{style="table-layout:auto"}

**例：**

次の例は、DROP AUDIENCE コマンドを使用したオーディエンスの削除方法を示しています。

```sql
DROP AUDIENCE IF EXISTS aud_test;
```

### 自動オーディエンス登録と可用性 {#registration-and-availability}

SQL 拡張機能を使用して作成されたオーディエンスは、オーディエンスワークスペースの Data Distiller[!UICONTROL  オリジン ] に自動的に登録されます。 登録すると、これらのオーディエンスをファイルベースの宛先でのターゲティングに使用できるようになり、セグメント化とターゲティング戦略が強化されます。 このプロセスには追加の設定は必要なく、オーディエンス管理を合理化します。 Platform UI 内でのオーディエンスの表示、管理、作成方法について詳しくは、[ オーディエンスポータルの概要 ](../../segmentation/ui/audience-portal.md) を参照してください。

<!-- Q) Do you know how long it takes for the audience to register? This info would help manage user expectations. -->

![Adobe Experience Platformのオーディエンスワークスペース。自動公開され、すぐに使用できる Data Distiller オーディエンスが表示されます。](../images/data-distiller/sql-audiences/audiences.png)

## 宛先に対してオーディエンスをアクティブ化 {#activate-audiences}

[!DNL Amazon S3]、[!DNL SFTP]、[!DNL Azure Blob] など、任意のファイルベースの宛先に対してオーディエンスをターゲティングすることで、オーディエンスをアクティブ化します。 エンリッチメントされたオーディエンス属性は、必要に応じてさらに絞り込み、フィルタリングするために使用できます。

![ パブリック宛先とプライベート宛先/カスタム宛先を表示する、Adobe Experience Platform宛先タイプのフローチャート（バッチオプションとストリーミングオプションを含む） ](../images/data-distiller/sql-audiences/destination-types.png)。

## 機能の説明 {#faqs}

この節では、Data Distillerで SQL を使用して外部オーディエンスを作成および管理する方法に関するよくある質問について説明します。

**質問**:

- オーディエンスの作成は、フラットなデータセットでのみサポートされますか？

+++回答

現在、オーディエンスの作成は、オーディエンスを定義する際にフラット（ルートレベル）属性に制限されます。

+++

- オーディエンスの作成の結果は、単一のデータセットまたは複数のデータセットになりますか、それとも設定によって異なりますか？

+++回答

オーディエンスとデータセットの間には 1 対 1 のマッピングがあります。

+++

- オーディエンスの作成時に作成されたデータセットはプロファイル用にマークされていますか？

+++回答

いいえ。オーディエンスの作成時に作成されたデータセットは、プロファイル用にマークされません。

+++

- データセットはデータレイク上で作成されていますか？

+++回答

はい、オーディエンスに関連付けられたデータセットは、データレイク上に作成されます。 このデータセットの属性は、Audience Composer と宛先フローでエンリッチメントされた属性として使用できます。

+++

- オーディエンス内の属性は、エンタープライズバッチファイルベースの宛先に制限されていますか？ （はい/いいえ）

+++回答

いいえ。オーディエンスのエンリッチメントされた属性は、エンタープライズバッチ宛先とファイルベースの宛先の両方で使用できます。 「次のセグメント ID には、この宛先で許可されていない名前空間があります：e917f626-a038-42f7-944c-xyxyx」のようなエラーが発生した場合は、Data Distillerで新しいセグメントを作成し、使用可能な任意の宛先で使用します。

+++

- Data Distiller オーディエンスを使用するオーディエンスのオーディエンスを作成できますか？

+++回答

はい、Data Distiller オーディエンスを使用するオーディエンスのオーディエンスを作成できます。

+++

- これらのオーディエンスはAdobe Journey Optimizerに表示されますか？ そうでない場合は、このオーディエンスのすべてのメンバーを含んだ新しいオーディエンスをルールビルダーで作成するとどうなりますか？

+++回答

Data Distiller オーディエンスは、Adobe Journey Optimizerでも使用できます。 Adobe Journey Optimizerで Data Distiller オーディエンスを使用し、エンリッチメントされた属性に基づいて結果をフィルタリングできます。

+++

- Data Distiller オーディエンスは外部オーディエンスなので、30 日ごとに削除されますか？

+++回答

はい、Data Distiller オーディエンスは外部オーディエンスなので、30 日ごとに削除されます。

+++

## 次の手順

このドキュメントでは、データDistillerの SQL オーディエンス拡張機能を使用して、SQL コマンドを使用してオーディエンスを効果的に作成、管理および公開する方法について説明しました。 独自のビジネス要件に基づいてオーディエンス定義をカスタマイズし、様々な宛先でオーディエンスをアクティブ化して、マーケティング戦略とデータ駆動型の決定を最適化できるようになりました。

次に、以下のドキュメントを参照して、Platform オーディエンス管理戦略をさらに開発および最適化してください。

- **オーディエンスの評価を調べる**:[Adobe Experience Platformのオーディエンス評価方法 ](../../segmentation/home.md#evaluate-segments)：リアルタイム更新のためのストリーミングセグメント化、スケジュールに沿った処理またはオンデマンド処理のためのバッチセグメント化、Edge Networkでの即時評価のためのエッジセグメント化）について説明します。
- **宛先との統合**:Platform の宛先 UI を使用して、[ オンデマンドでファイルをバッチ宛先に書き出す ](../../destinations/ui/export-file-now.md) 方法に関するガイドを参照してください。
- **オーディエンスパフォーマンスの確認**：様々なチャネルにおける SQL 定義オーディエンスのパフォーマンスを分析します。 データインサイトを使用すると、オーディエンスの定義とターゲティング戦略を調整および改善できます。 Adobe Real-Time CDPで SQL クエリにアクセスしてオーディエンスインサイトに適応させる方法については、[ オーディエンスインサイト ](../../dashboards/insights/audiences.md) に関するドキュメントを参照してください。 その後、オーディエンスダッシュボードをカスタマイズして独自のインサイトを作成し、生のデータを実用的な情報に変換することで、これらのインサイトを効果的に視覚化し、より良い意思決定に使用できます。

