---
title: SQL を使用したバッチ取り込みの調査、トラブルシューティング、検証
description: Adobe Experience Platformのデータ取得プロセスを理解し、管理する方法を説明します。 このドキュメントには、バッチを検証し、取り込んだデータをクエリする方法が含まれています。
exl-id: 8f49680c-42ec-488e-8586-50182d50e900
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1170'
ht-degree: 0%

---

# SQL を使用したバッチ取り込みの調査、トラブルシューティング、検証

このドキュメントでは、取り込まれたバッチのレコードを SQL で検証および検証する方法を説明します。 このドキュメントでは、次の方法を説明します。

- データセットバッチメタデータへのアクセス
- バッチのクエリによるトラブルシューティングとデータ整合性の確保

>[!NOTE]
>
>このガイドの一部のスクリーンショットは、[!DNL DBVisualizer] から取得したものです。 [&#x200B; クエリサービスを DBVisualizer](../clients/dbvisulaizer.md) またはその他の [&#x200B; サードパーティの BI ツールに接続 &#x200B;](../clients/overview.md) する方法については、リンクされたドキュメントを参照してください。

## 前提条件

このドキュメントで説明する概念を理解しやすくするために、次のトピックに関する知識が必要です。

- **データ取り込み**：様々な方法やプロセスなど、データをExperience Platformに取り込む方法の基本については、[&#x200B; データ取り込みの概要 &#x200B;](../../ingestion/home.md) を参照してください。
- **バッチ取り込み**：バッチ取り込みの基本概念については、[&#x200B; バッチ取り込み API の概要 &#x200B;](../../ingestion/batch-ingestion/overview.md) を参照してください。 特に、「バッチ」とは何か、およびExperience Platformのデータ取得プロセス内でどのように機能するかについて説明します。
- **データセット内のシステムメタデータ**：取り込んだデータを追跡およびクエリするためにシステムメタデータフィールドを使用する方法については、[&#x200B; カタログサービスの概要 &#x200B;](../../catalog/home.md) を参照してください。
- **エクスペリエンスデータモデル（XDM）**:XDM スキーマと、それらがExperience Platformに取り込まれたデータの構造と形式を表し検証する方法について詳しくは [&#128279;](../../xdm/ui/overview.md) スキーマ UI の概要  および [&#x200B; のスキーマ構成の基本」 &#x200B;](../../xdm/schema/composition.md) を参照してください。

## データセットバッチメタデータへのアクセス {#access-dataset-batch-metadata}

システム列（メタデータ列）がクエリ結果に含まれるようにするには、クエリエディターで SQL コマンド `set drop_system_columns=false` を使用します。 SQL クエリセッションの動作を設定します。 新しいセッションを開始する場合、この入力を繰り返す必要があります。

次に、データセットのシステムフィールドを表示するには、SELECT all 文を実行して、データセットからの結果（例：`select * from movie_data`）を表示します。 結果には、右側の `_acp_system_metadata` と `_ACP_BATCHID` に 2 つの新しい列が含まれます。 メタデータ列 `_acp_system_metadata` と `_ACP_BATCHID` は、取り込まれたデータの論理パーティションと物理パーティションを識別するのに役立ちます。

![movie_data テーブルとそのメタデータ列が表示され、ハイライト表示された DBVisualizer UI。](../images/use-cases/movie_data-table-with-metadata-columns.png)

データがExperience Platformに取り込まれると、受信データに基づいて論理パーティションが割り当てられます。 この論理パーティションは `_acp_system_metadata.sourceBatchId` で表されます。 この ID は、データバッチを処理および保存する前に、データバッチを論理的にグループ化して識別するのに役立ちます。

データが処理され、データレイクに取り込まれると、`_ACP_BATCHID` で表される物理パーティションが割り当てられます。 この ID は、取り込まれたデータが存在するデータレイク内の実際のストレージパーティションを反映します。

### SQL を使用して論理パーティションと物理パーティションを把握 {#understand-partitions}

データが取り込み後にどのようにグループ化され、配布されるかを理解するには、次の問合せを使用して、各論理パーティション（`_acp_system_metadata.sourceBatchId`）の個別の物理パーティション（`_ACP_BATCHID`）の数をカウントします。

```SQL
SELECT  _acp_system_metadata, COUNT(DISTINCT _ACP_BATCHID) FROM movie_data
GROUP BY _acp_system_metadata
```

このクエリの結果を次の画像に示します。

![&#x200B; 各論理パーティションの個別の物理パーティション数を表示するクエリの結果。](../images/use-cases/logical-and-physical-partition-count.png)

これらの結果は、システムがデータをバッチ化してデータレイクに保存する最も効率的な方法を決定するので、入力バッチの数が出力バッチの数と必ずしも一致しないことを示しています。

この例では、CSV ファイルをExperience Platformに取り込み、`drug_checkout_data` というデータセットを作成したと想定しています。

この `drug_checkout_data` ファイルは、35,000 件のレコードで構成される、深くネストされたセットです。 SQL 文 `SELECT * FROM drug_orders;` を使用して、JSON ベースの `drug_orders` データセット内のレコードの最初のセットをプレビューします。

次の画像は、ファイルとそのレコードのプレビューを示しています。

![JSON ベースの drug_orders データセット内の最初のレコードセットのプレビュー。](../images/use-cases/drug-orders-preview.png)

### SQL を使用したバッチ取り込みプロセスに関するインサイトの生成 {#sql-insights-on-batch-ingestion}

以下の SQL 文を使用して、データ取り込みプロセスが入力レコードをグループ化してバッチに処理する方法に関するインサイトを提供します。

```sql
SELECT _acp_system_metadata,
       Count(DISTINCT _acp_batchid) AS numoutputbatches,
       Count(_acp_batchid)          AS recordcount
FROM   drug_orders
GROUP  BY _acp_system_metadata 
```

クエリ結果は、次の画像のようになります。

![&#x200B; レコード数を含む、一度に入力バッチをマスターする方法の分布を示す表。](../images/use-cases/distribution-of-input-batches.png)

結果は、データ取り込みプロセスの効率と動作を示しています。 レコードを組み合わせて重複排除する際に、それぞれ 2000、24000、9000 のレコードを含んだ 3 つの入力バッチが作成されましたが、残ったのは 1 つの一意のバッチのみです。

>[!NOTE]
>
>データセット内に表示されるすべてのレコードは、正常に取り込まれたレコードです。 バッチ取り込みが成功しても、ソース入力から送信されたすべてのレコードが存在するわけではありません。 データ取り込みに失敗したかどうかを確認して、失敗しなかったバッチやレコードを見つける必要があります。

## SQL を使用したバッチの検証 {#validate-a-batch-with-SQL}

次に、SQL を使用してデータセットに取り込まれたレコードを検証および検証します。

>[!TIP]
>
>バッチ ID を取得し、そのバッチ ID に関連付けられているクエリレコードを取得するには、まずExperience Platform内でバッチを作成する必要があります。 プロセスを自分でテストする場合は、CSV データをExperience Platformに取り込むことができます。 [AI で生成されたレコメンデーションを使用して、既存の XDM スキーマに CSV ファイルをマッピングする &#x200B;](../../ingestion/tutorials/map-csv/recommendations.md) 方法に関するガイドを参照してください。

バッチを取り込んだら、データを取り込んだデータセットの [!UICONTROL &#x200B; データセットアクティビティ &#x200B;] タブ）に移動する必要があります。

Experience Platform UI の左側のナビゲーションで **[!UICONTROL データセット]** を選択して、[!UICONTROL &#x200B; データセット &#x200B;] ダッシュボードを開きます。 次に、「参照 [!UICONTROL &#x200B; タブからデータセットの名前を選択して &#x200B;] データセットアクティビティ [!UICONTROL &#x200B; 画面にアクセス &#x200B;] ます。

![&#x200B; 左側のナビゲーションでデータセットがハイライト表示されたExperience Platform UI データセット &#x200B;](../images/use-cases/datasets-workspace.png)

[!UICONTROL &#x200B; データセットアクティビティ &#x200B;] ビューが表示されます。 このビューには、選択したデータセットの詳細が表示されます。 表形式で表示される、取り込まれたバッチが含まれます。

使用可能なバッチのリストからバッチを選択し、右側の詳細パネルから [!UICONTROL &#x200B; バッチ ID] をコピーします。

![&#x200B; 取り込んだレコードがバッチ ID でハイライト表示されているExperience Platform データセット UI。](../images/use-cases/batch-id.png)

次に、次のクエリを使用して、そのバッチの一部としてデータセットに含まれているすべてのレコードを取得します。

```sql
SELECT * FROM movie_data
WHERE  _acp_batchid='01H00BKCTCADYRFACAAKJTVQ8P' 
LIMIT 1;
```

`_ACP_BATCHID` キーワードは、[!UICONTROL &#x200B; バッチ ID] のフィルタリングに使用されます。

>[!TIP]
>
>`LIMIT` 句は、表示される行数を制限する場合に便利ですが、フィルター条件の方が望ましいです。

クエリエディターでこのクエリを実行すると、結果は 100 行に切り捨てられます。 クエリエディターは、プレビューと調査をすばやく行えるように設計されています。 最大 50,000 行を取得するには、DBVisualizer や DBeaver などのサードパーティツールを使用できます。

## 次の手順 {#next-steps}

このドキュメントを読むことで、データ取り込みプロセスの一環として、取り込んだバッチでレコードを検証および検証するための基本事項を学びました。 また、データセットのバッチメタデータへのアクセス、論理パーティションと物理パーティションの理解、SQL コマンドを使用した特定のバッチのクエリに関するインサイトも得ました。 この知識は、Experience Platformでのデータの整合性を確保し、データストレージを最適化するのに役立ちます。

次に、学習した概念を適用するためにデータ取り込みを練習する必要があります。 提供されたサンプルファイルまたは独自のデータを使用して、サンプルデータセットをExperience Platformに取り込みます。 まだ行っていない場合は、[&#x200B; データをAdobe Experience Platformに取り込む &#x200B;](../../ingestion/tutorials/ingest-batch-data.md) 方法に関するチュートリアルをお読みください。

または、様々なデスクトップクライアントアプリケーションを使用してクエリサービスに接続し検証する [&#x200B; 方法を学習して &#x200B;](../clients/overview.md) データ分析機能を強化することもできます。
