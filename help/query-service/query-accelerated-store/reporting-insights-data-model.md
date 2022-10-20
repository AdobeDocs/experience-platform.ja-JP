---
title: Accelerated Store レポートインサイトのクエリ
description: クエリサービスを通じてレポートインサイトデータモデルを構築し、高速ストアデータとユーザー定義ダッシュボードで使用する方法を説明します。
source-git-commit: 9c18432bbd9322aee1924c34cb10aadac440e726
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# Query accelerated Store レポートインサイト

Query Accelerated Store を使用すると、データから重要なインサイトを得るのに必要な時間と処理能力を削減できます。 通常、データは、集計ビューが作成およびレポートされる一定の間隔（1 時間ごとや 1 日ごとなど）で処理されます。 集計データから生成されるこれらのレポートの分析は、ビジネスパフォーマンスを向上させるためのインサイトを導き出します。 クエリアクセラレーションストアは、キャッシュサービス、同時実行、インタラクティブなエクスペリエンス、ステートレス API を提供します。 ただし、データが事前に処理され、集計されたクエリ用に最適化されていると想定しています。生データのクエリには最適ではありません。

クエリ高速化ストアを使用すると、カスタムデータモデルを作成したり、既存のReal-time Customer Data Platformデータモデルを拡張したりできます。 その後、レポートインサイトを任意のレポート/ビジュアライゼーションフレームワークに関与させたり、組み込んだりできます。 Adobe Experience Platformのリアルタイム CDP データモデルは、プロファイル、セグメント、宛先に関するインサイトを提供し、リアルタイム CDP インサイトダッシュボードを有効にします。 このドキュメントでは、レポートインサイトデータモデルの作成プロセスと、必要に応じてリアルタイム CDP データモデルを拡張する方法について説明します。

## 前提条件

このチュートリアルでは、ユーザー定義のダッシュボードを使用して、Platform UI 内のカスタムデータモデルからのデータを視覚化します。 詳しくは、 [ユーザー定義ダッシュボードドキュメント](../../dashboards/user-defined-dashboards.md) この機能の詳細については、を参照してください。

## はじめに

Data Distiller SKU は、レポートに関するインサイトのカスタムデータモデルを構築し、強化された Platform データを保持する Real-time CDP データモデルを拡張するために必要です。 詳しくは、 [パッケージ](../packages.md), [guardrail](../guardrails.md#query-accelerated-store)、および [ライセンス](../data-distiller/licence-usage.md) Data Distiller SKU に関するドキュメント。 Data Distiller SKU をお持ちでない場合、詳しくは、Adobeのカスタマーサービス担当者にお問い合わせください。

## レポートインサイトデータモデルの作成

このチュートリアルでは、オーディエンスインサイトデータモデルの作成例を使用します。 1 つ以上の広告主プラットフォームを使用してオーディエンスにリーチする場合は、広告主の API を使用して、オーディエンスのおおよその一致数を取得できます。

最初は、ソース（広告主プラットフォーム API から取得した可能性があります）の初期データモデルがあります。 生データを集計して表示するには、次の画像に示すように、レポートインサイトモデルを作成します。 これにより、1 つのデータセットでオーディエンスの一致の上限と下限を取得できます。

![オーディエンスインサイトユーザーモデルのエンティティリレーショナル図 (ERD) です。](../images/query-accelerated-store/audience-insight-user-model.png)

この例では、 `externalaudiencereach` テーブル/データセットは ID に基づいており、一致数の下限と上限を追跡します。 この `externalaudiencemapping` ディメンションテーブル/データセットは、外部 ID を Platform 上の宛先とセグメントにマッピングします。

## Data Distillerを使用したレポートインサイト用のモデルの作成

次に、レポートインサイトモデルを作成します (`audienceinsight` この例では )、SQL コマンドを使用します。 `ACCOUNT=acp_query_batch and TYPE=QSACCEL` 高速ストアで確実に作成されるようにします。 次に、クエリサービスを使用して `audienceinsight.audiencemodel` スキーマ `audienceinsight` データベース。

>[!NOTE]
>
>Data Distiller SKU は、 `ACCOUNT=acp_query_batch` コマンドを使用します。 これがない場合、データレイク上に通常のデータモデルが作成されます。

```sql
CREATE database audienceinsight WITH (TYPE=QSACCEL, ACCOUNT=acp_query_batch);
 
CREATE schema audienceinsight.audiencemodel;
```

## テーブル、関係の作成、データの入力

これで、 `audienceinsight` レポートインサイトモデル、作成 `externalaudiencereach` および `externalaudiencemapping` テーブルを作成し、それらの間に関係を確立します。 次に、 `ALTER TABLE` コマンドを使用して、テーブル間に外部キー制約を追加し、関係を定義します。 次の SQL の例は、この方法を示しています。

```sql
CREATE TABLE IF NOT exists audienceinsight.audiencemodel.externalaudiencereach
WITH ( DISTRIBUTION = REPLICATE ) AS
  SELECT cast(null as int) approximate_count_upper_bound,
         cast(null as string) deliverystatusdescription,
         cast(null as timestamp)  timeupdated ,
         cast(null as int) operationstatuscode ,
         cast(null as string) operationstatusdescription,
         cast(null as int) approximate_count_lower_bound,
         cast(null as timestamp)timecreated ,
         cast(null as timestamp)timecontentupdated ,
         cast(null as int) deliverystatuscode ,
         cast(null as int)  ext_custom_audience_id
   WHERE false;
 
CREATE TABLE IF NOT exists audienceinsight.audiencemodel.externalaudiencemapping
WITH ( DISTRIBUTION = REPLICATE ) AS
SELECT cast(null as int) segment_id,
       cast(null as int) destination_id,
       cast(null as int) ext_custom_audience_id
 WHERE false;
 
ALTER TABLE externalaudiencereach ADD  CONSTRAINT FOREIGN KEY (ext_custom_audience_id) REFERENCES externalaudiencemapping (ext_custom_audience_id) NOT enforced;
```

両方の `ALTER TABLE` コマンドを使用すると、ファクトテーブルとディメンションテーブルの間の関係が形成されます。

文を実行したら、 `SHOW datagroups;` コマンドを使用して、高速ストア上の使用可能なデータセットのリストを `audienceinsight.audiencemodel`. 表の結果は、次の例のようになります。

>[!IMPORTANT]
>
>Query Service ステートレス API エンドポイントからアクセスできるのは、Accelerated Store 内のデータだけです `POST /data/foundation/query/accelerated-queries`.

```console
    Database     |    Schema     | GroupType |      ChildType       |        ChildName        | PhysicalParent |               ChildId               
-----------------+---------------+-----------+----------------------+-------------------------+----------------+--------------------------------------
 audienceinsight | audiencemodel | QSACCEL   | Data Warehouse Table | externalaudiencemapping | true           | 9155d3b4-889d-41da-9014-5b174f6fa572
 audienceinsight | audiencemodel | QSACCEL   | Data Warehouse Table | externalaudiencereach   | true           | 1b941a6d-6214-4810-815c-81c497a0b636
```

## レポートインサイトデータモデルのクエリ

クエリサービスを使用して `audiencemodel.externalaudiencereach` ディメンションテーブル。 クエリの例を以下に示します。

```sql
SELECT a.ext_custom_audience_id,
       a.approximate_count_upper_bound
FROM   audiencemodel.externalaudiencereach AS a
       LEFT OUTER JOIN audiencemodel.externalaudiencemapping AS b
                    ON ( ( a.ext_custom_audience_id ) =
                         ( b.ext_custom_audience_id ) )
GROUP  BY a.ext_custom_audience_id,
          a.approximate_count_upper_bound
LIMIT  5000 ;
```

表化された結果には、カウントと ID が含まれます。

```console
ext_custom_audience_id | approximate_count_upper_bound
------------------------+-------------------------------
 23850912218170554      |                          1000
 23850808585120554      |                       1012000
 23850808585220554      |                        100000
 23850814978560554      |                          1000
 23850808585180554      |                        421000
 23850814978510554      |                       3001000
 23850814978530554      |                        300000
 23850912218160554      |                        105000
 23850808584990554      |                          1000
 23850809520110554      |                          1000
(10 rows)
```

## リアルタイム CDP インサイトデータモデルを使用して、データモデルを拡張する

オーディエンスモデルを追加の詳細で拡張して、よりリッチなディメンションテーブルを作成できます。 例えば、セグメント名と宛先名を外部オーディエンスの識別子にマッピングできます。 これをおこなうには、クエリサービスを使用して、新しいデータセットを作成または更新し、セグメントと宛先を外部 ID と組み合わせるオーディエンスモデルに追加します。 次の図は、このデータモデル拡張の概念を示しています。

![リアルタイム CDP インサイトデータモデルとクエリアクセラレーションストアモデルをリンクした ERD 図。](../images/query-accelerated-store/updatingAudienceInsightUserModel.png)

## ディメンションテーブルを作成して、レポートインサイトモデルを拡張します

クエリサービスを使用して、エンリッチメントされた Real-time CDP ディメンションデータセットからに主要な記述属性を追加する `audienceinsight` データモデルを作成し、ファクトテーブルと新しいディメンションテーブルの間の関係を確立します。 次の SQL は、既存のディメンションテーブルをレポートインサイトデータモデルに統合する方法を示しています。

```sql
CREATE TABLE audienceinsight.audiencemodel.external_seg_dest_map AS
  SELECT ext_custom_audience_id,
         destination_name,
         segment_name,
         destination_status,
         a.destination_id,
         a.segment_id
  FROM   externalaudiencemapping AS a
         LEFT OUTER JOIN adwh_dim_segments AS b
                      ON ( ( a.segment_id ) = ( b.segment_id ) )
         LEFT OUTER JOIN adwh_dim_destination AS c
                      ON ( ( a.destination_id ) = ( c.destination_id ) );
 
ALTER TABLE externalaudiencereach  ADD  CONSTRAINT FOREIGN KEY (ext_custom_audience_id) REFERENCES external_seg_dest_map (ext_custom_audience_id) NOT enforced;
```

以下を使用： `SHOW datagroups;` 追加の `external_seg_dest_map` ディメンションテーブル。

```console
    Database     |     Schema     | GroupType |      ChildType       |                ChildName  | PhysicalParent |               ChildId               
-----------------+----------------+-----------+----------------------+----------------------------------------------------+----------------+--------------------------------------
 audienceinsight | audiencemodel | QSACCEL   | Data Warehouse Table | external_seg_dest_map      | true           | 4b4b86b7-2db7-48ee-a67e-4b28cb900810
 audienceinsight | audiencemodel | QSACCEL   | Data Warehouse Table | externalaudiencemapping    | true           | b0302c05-28c3-488b-a048-1c635d88dca9
 audienceinsight | audiencemodel | QSACCEL   | Data Warehouse Table | externalaudiencereach      | true           | 4485c610-7424-4ed6-8317-eed0991b9727
```

## 拡張加速ストアレポートインサイトデータモデルのクエリ

これで、 `audienceinsight` データモデルが強化され、クエリを実行する準備が整いました。 次の SQL は、マッピングされた宛先とセグメントのリストを示しています。

```sql
SELECT a.ext_custom_audience_id,
       b.destination_name,
       b.segment_name,
       b.destination_status,
       b.destination_id,
       b.segment_id
FROM   audiencemodel.externalaudiencereach1 AS a
       LEFT OUTER JOIN audiencemodel.external_seg_dest_map AS b
                    ON ( ( a.ext_custom_audience_id ) = (
                         b.ext_custom_audience_id ) )
LIMIT  25; 
```

クエリは、クエリアクセラレーションストア上のすべてのデータセットを返します。

```console
ext_custom_audience_id | destination_name |       segment_name        | destination_status | destination_id | segment_id 
------------------------+------------------+---------------------------+--------------------+----------------+-------------
 23850808595110554      | FCA_Test2        | United States             | enabled            |     -605911558 | -1357046572
 23850799115800554      | FCA_Test2        | Born in 1980s             | enabled            |     -605911558 | -1224554872
 23850799115790554      | FCA_Test2        | Born in 1970s             | enabled            |     -605911558 |  1899603869
 23850798177620554      | FCA_Test1        | Billionaires              | enabled            |      321720439 |  1401872665
 23850814978560554      | FCA_Test3        | Canada - Saskatchewan     | enabled            |     1182494936 | -1917996562
 23850808585180554      | FCA_Test3        | United States             | enabled            |     1182494936 | -1357046572
 23850814978530554      | FCA_Test3        | Canada - British Columbia | enabled            |     1182494936 |  -652840507
 23850808585120554      | FCA_Test3        | Canada - Quebec           | enabled            |     1182494936 |  -519557860
 23850809520110554      | FCA_Test3        | Born in 1960s             | enabled            |     1182494936 |   237824266
 23850808585220554      | FCA_Test3        | Western Canada            | enabled            |     1182494936 |  1075937528
 23850808584990554      | FCA_Test3        | Canada - Ontario          | enabled            |     1182494936 |  1593438041
 23850814978510554      | FCA_Test3        | Canada - Alberta          | enabled            |     1182494936 |  1862946783
 23850912218170554      | FCA_Test4        | Canada - Alberta          | enabled            |     1549248886 |  1862946783
 23850912218160554      | FCA_Test4        | Born in 1970s             | enabled            |     1549248886 |  1899603869
```

## ユーザー定義のダッシュボードでデータを視覚化

これで、カスタムデータモデルを作成したので、カスタムクエリとユーザー定義ダッシュボードを使用してデータを視覚化する準備が整いました。

次の SQL は、宛先のオーディエンスごとの一致数の分類と、セグメント別のオーディエンスの各宛先の分類を提供します。

```sql
SELECT b.destination_name,
       a.approximate_count_upper_bound,
       b.segment_name
FROM   audiencemodel.externalaudiencereach AS a
       LEFT OUTER JOIN audiencemodel.external_seg_dest_map AS b
                    ON ( ( a.ext_custom_audience_id ) = (
                         b.ext_custom_audience_id ) )
GROUP  BY b.destination_name,
          a.approximate_count_upper_bound,
          b.segment_name
ORDER BY b.destination_name
LIMIT  5000
```

次の画像は、レポートインサイトデータモデルを使用した、考えられるカスタムビジュアライゼーションの例を示しています。

![新しいレポートインサイトデータモデルから作成された宛先およびセグメントウィジェット別の一致数。](../images/query-accelerated-store/user-defined-dashboard-widget.png)

カスタムデータモデルは、ユーザー定義のダッシュボードワークスペースで使用可能なデータモデルのリストに表示されます。 詳しくは、 [ユーザー定義ダッシュボードガイド](../../dashboards/user-defined-dashboards.md) を参照してください。
