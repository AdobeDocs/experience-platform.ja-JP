---
keywords: Experience Platform；クエリサービス；クエリサービス；ネストされたデータ構造；ネストされたデータ；統合；ネストされたデータの統合；
title: BI ツールで使用するネストされたデータ構造の統合
description: このドキュメントでは、サードパーティの BI ツールをクエリサービスで使用する際に、セッション中にすべてのテーブルとビューの XDM スキーマを統合する方法を説明します。
exl-id: 7e534c0a-db6c-463e-85da-88d7b2534ece
source-git-commit: a7f273383293359cf6adbcc0a508fb19d2789339
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 1%

---

# ネストされたデータ構造を統合して、サードパーティの BI ツールで使用

Adobe Experience Platform Query Service は、 `FLATTEN` 設定を使用して、サードパーティの BI ツールを使用してデータベースに接続する場合に使用します。 この機能では、サードパーティの BI ツールでネストされたデータ構造を統合して、データの使い勝手を向上させ、データの取得、分析、変換およびレポートに必要なワークロードを削減します。

多くの BI ツール [!DNL Tableau] および [!DNL Power BI] ネストされたデータ構造をネイティブでサポートしないでください。 この `FLATTEN` を設定すると、データの上に SQL ビューを作成してフラットバージョンを提供したり、クエリサービスを使用したりする必要がなくなります `CTAS` または `INSERT INTO` アドホックスキーマを使用する際に、データセットをフラットバージョンに複製するジョブ。

この `FLATTEN` を設定すると、各リーフフィールドの構造がテーブルのルートに取り込まれ、元の名前空間の後にフィールドの名前が付けられます。 これにより、フィールドのコンテキストを保持しながら、ドット表記を使用して、フィールドをエクスペリエンスデータモデル (XDM) パスと照合できます。

## 前提条件

の使用 `FLATTEN` の設定では、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [XDM システム](../../xdm/home.md):XDM とその実装の概要を示します。Experience Platform。

   * [アドホックスキーマの作成](../../xdm/tutorials/ad-hoc.md):単一のデータセットでのみ使用するために名前空間化されたフィールドを持つ XDM スキーマは、アドホックスキーマと呼ばれます。 アドホックスキーマは、様々なデータ取り込みワークフローで使用され、Experience Platformや特定の種類のソース接続を作成します。

* [サンドボックス](../../sandboxes/home.md):Experience Platformは、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

* [ネストされたデータ構造](./nested-data-structures.md):このドキュメントでは、ネストされたデータ構造を含む複雑なデータ型のデータセットを作成、処理、変換する方法の例を示します。

## FLATTEN 設定を使用したデータベースへの接続 {#connect-with-flatten}

この `FLATTEN` を設定すると、ネストされたデータ構造が別々の列に統合され、属性名が行値を保持する列名になります。 ネストされたデータ構造をサポートしない BI ツールでデータを操作する場合、この設定により、アドホックスキーマの使いやすさが向上し、必要なワークロードが削減されます。

選択したサードパーティクライアントでクエリサービスに接続する際に、 `FLATTEN` をデータベース名に設定します。 特定の BI ツールの接続方法について詳しくは、 [クエリサービスへのクライアント接続の概要](../clients/overview.md). 接続文字列には、次の値が含まれる必要があります。

* サンドボックス名。
* コロンの後に `all` または特定のデータセット ID、ビュー ID、データベース名を指定します。
* 疑問符 (?) 続いて `FLATTEN` キーワード。

入力は次の形式にする必要があります。

```terminal
{sandbox_name}:{all/ID/database_name}?FLATTEN
```

接続文字列の例を次に示します。

```terminal
prod:all?FLATTEN
```

## 例 {#example}

このガイドで使用されるスキーマの例では、標準フィールドグループを使用しています [!UICONTROL コマースの詳細]を利用した `commerce` オブジェクト構造と `productListItems` 配列。 詳しくは、 XDM のドキュメントを参照してください。 [詳細情報 [!UICONTROL コマースの詳細] フィールドグループ](../../xdm/field-groups/event/commerce-details.md). スキーマ構造の表現は、次の画像で確認できます。

![コマースの詳細フィールドグループのスキーマ図。 `commerce` および `productListItems` 構造。](../images/best-practices/flatten-nested-data/commerce-details.png)

BI ツールでネストされたデータ構造がサポートされていない場合、シリアル化された値 ( `commerce` および `productListItems` を参照 )。 これらの値は、1 つのエンコード済みの `commerce` 文字列フィールドとは現実的に使用できないものです。

次の値は、を表します。 `commerce.order.priceTotal` (3018.0), `commerce.order.purchaseID` (c9b5aff9-25de-450b-98f4-4484a2170180) および `commerce.purchases.value`(1.0) ネストされたフィールドの形式が適切でない。

```terminal
("(3018.0,c9b5aff9-25de-450b-98f4-4484a2170180)","(1.0)")
```

を使用する `FLATTEN` を設定すると、ドット表記と元のパス名を使用して、スキーマ内の別のフィールドや、ネストされたデータ構造のセクション全体にアクセスできます。 この形式の例として、 `commerce` フィールドグループは以下のとおりです。

```terminal
commerce.order.priceTotal
commerce.order.purchaseID
commerce.purchases.value
```

この `FLATTEN` の設定には、他のデータ構造を処理する際に一定の制限があります。 詳細は、 [制限事項](#limitations).

### クエリのフィールドに引用符を使用する {#quotation-marks}

フラット化されたルートフィールドは、XDM パスと一致させるためにドット表記を使用するようになりました。 クエリで使用する場合、フィールドを引用符 (&quot; &quot;) で囲む必要があります。

次の SQL の例は、ネストされたクエリの元の状態を示しています。

```sql
SELECT YEAR(timestamp) AS year,
       SUM(commerce.order.priceTotal) AS revenue
FROM purchase_events_dataset
WHERE commerce.purchases.value > 0
GROUP BY 1;
```

フラット化されたデータフィールドを使用する場合、クエリは次に示すように、ドット表記を使用し、引用符で囲んで記述されます。

```sql
SELECT YEAR(timestamp) AS year,
       SUM("commerce.order.priceTotal") AS revenue
FROM purchase_events_dataset
WHERE "commerce.purchases.value" > 0
GROUP BY 1;
```

## 制限事項 {#limitations}

この `FLATTEN` を設定しても、現在、次のデータ構造はフラット化されません。

| データ構造 | 制限事項 |
|---|---|
| 配列 | 明示的な配列インデックスまたは `EXPLODE` 関数を使用して、配列にアクセスできます。 |
| マップ | マップの下の値にアクセスしてマップにアクセスするには、文字列キーを使用します。 |

マップと配列の両方の制限を解決するには、Power BIの [ 詳細オプション ]/[SQL ステートメント ] など、BI ツール生の SQL 編集を使用する必要があります。

マップと配列の制限の両方を解決するには、生の SQL 編集などの BI ツールが必要です。 方法に関するガイドを参照してください。 [Power BIの詳細オプションを使用してカスタム SQL クエリを入力する](https://experienceleague.adobe.com/docs/experience-platform/query/clients/power-bi.html#import-tables-using-custom-sql) SQL 文の節を参照してください。

## 次の手順

このドキュメントでは、ネストされたデータ構造を統合して、サードパーティの BI ツールで使用する方法について説明しました。 クライアントにまだ接続していない場合は、 [クライアント接続の概要](../clients/overview.md) を参照してください。
