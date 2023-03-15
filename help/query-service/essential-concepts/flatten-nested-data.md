---
keywords: Experience Platform;クエリサービス;クエリサービス;ネストされたデータ構造;ネストされたデータ;統合;ネストされたデータの統合;
title: BI ツールで使用するネストされたデータ構造の統合
description: このドキュメントでは、サードパーティの BI ツールをクエリサービスで使用する際に、セッション中にすべてのテーブルとビューの XDM スキーマを統合する方法について説明します。
exl-id: 7e534c0a-db6c-463e-85da-88d7b2534ece
source-git-commit: 1c590350c9d519dba60375b92de7bbbbd77961dc
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 100%

---

# サードパーティの BI ツールで使用するネストされたデータ構造の統合

Adobe Experience Platform クエリサービスは、サードパーティの BI ツールを使用してデータベースに接続する際に `FLATTEN` 設定をサポートします。この機能を使用すると、サードパーティの BI ツールでネストされたデータ構造を統合し、使いやすさを向上させ、データの取得、分析、変換およびレポートに必要なワークロードを軽減できます。

[!DNL Tableau] や [!DNL Power BI] などの多くの BI ツールは、ネストされたデータ構造をネイティブにサポートしていません。`FLATTEN` 設定により、統合バージョンを指定するためにデータの上に SQL ビューを作成する必要がなくなります。また、アドホックスキーマを使用する場合、クエリサービス `CTAS` または `INSERT INTO` ジョブを使用してデータセットを統合バージョンに複製する必要もなくなります。

`FLATTEN` 設定により、各リーフフィールドの構造をテーブルのルートにプルし、元の名前空間にちなんでフィールドに名前を付けることができます。これにより、ドット表記を使用して、フィールドのコンテキストを維持しながら、フィールドをそのエクスペリエンスデータモデル（XDM）パスに一致させることができます。

## 前提条件

`FLATTEN` 設定を使用するには、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [XDM システム](../../xdm/home.md)：XDM と Experience Platform での実装の概要。

   * [アドホックスキーマの作成](../../xdm/tutorials/ad-hoc.md)：単一のデータセットでのみ使用するために名前空間が設定されているフィールドを持つ XDM スキーマは、アドホックスキーマと呼ばれます。アドホックスキーマは、Experience Platform の様々なデータ取り込みワークフローで使用され、特定の種類のソース接続を作成します。

* [サンドボックス](../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを個別の仮想環境に分割する仮想サンドボックスを提供し、デジタル体験アプリケーションの開発および進化を支援します。

* [ネストされたデータ構造](./nested-data-structures.md)：このドキュメントでは、ネストされたデータ構造を含む複雑なデータタイプを含むデータセットを作成、処理または変換する方法の例について説明します。

## FLATTEN 設定を使用したデータベースへの接続 {#connect-with-flatten}

`FLATTEN` を設定すると、ネストされたデータ構造が別々の列に統合され、属性名が行値を保持する列名になります。ネストされたデータ構造をサポートしない BI ツールでデータを操作する場合、この設定によりアドホックスキーマの使いやすさが向上し、必要なワークロードが軽減されます。

選択したサードパーティクライアントでクエリサービスに接続する際は、`FLATTEN` 設定をデータベース名に追加します。特定の BI ツールの接続方法について詳しくは、[クエリサービスへのクライアント接続の概要](../clients/overview.md)に記載されている各ドキュメントを参照してください。接続文字列には、次の内容を含める必要があります。

* サンドボックス名。
* コロン + `all` または特定のデータセット ID、ビュー ID またはデータベース名。
* 疑問符（?） + `FLATTEN` キーワード。

入力は次の形式にする必要があります。

```terminal
{sandbox_name}:{all/ID/database_name}?FLATTEN
```

接続文字列の例を以下に示します。

```terminal
prod:all?FLATTEN
```

## 例 {#example}

このガイドで使用されるスキーマの例では、`commerce` オブジェクト構造と `productListItems` 配列を使用する標準フィールドグループの[!UICONTROL コマースの詳細]を採用しています。[[!UICONTROL コマースの詳細]フィールドグループ](../../xdm/field-groups/event/commerce-details.md)について詳しくは、XDM のドキュメントを参照してください。スキーマ構造の表現については、次の画像を参照してください。

![`commerce` および `productListItems` 構造を含むコマースの詳細フィールドグループのスキーマ図。](../images/essential-concepts/commerce-details.png)

BI ツールがネストされたデータ構造をサポートしていない場合、ネストされたフィールドにシリアル化された値（スキーマの例の `commerce` や `productListItems` など）が含まれていると、参照が困難になる可能性があります。これらの値は、単一のエンコードされた `commerce` 文字列フィールドの一部として表示される場合があり、現実的に使用できないわけではありません。

次の値は、不適切な形式のネストされたフィールドの `commerce.order.priceTotal`（3018.0）、`commerce.order.purchaseID`（c9b5aff9-25de-450b-98f4-4484a2170180）および `commerce.purchases.value`（1.0）を表します。

```terminal
("(3018.0,c9b5aff9-25de-450b-98f4-4484a2170180)","(1.0)")
```

`FLATTEN` 設定を使用すると、ドット表記と元のパス名を使用して、スキーマ内の個別のフィールドまたはネストされたデータ構造のセクション全体にアクセスできます。`commerce` フィールドグループを使用したこの形式の例を以下に示します。

```terminal
commerce.order.priceTotal
commerce.order.purchaseID
commerce.purchases.value
```

`FLATTEN` 設定には、他のデータ構造を処理する際に特定の制限があります。完全な詳細については、[制限事項の節](#limitations)を参照してください。

### クエリのフィールドでの引用符の使用 {#quotation-marks}

フラット化されたルートフィールドは、XDM パスと一致するようにドット表記を使用するようになりました。クエリで使用する場合、フィールドを引用符（&quot; &quot;）で囲む必要があります。

以下の SQL 例は、ネストされたクエリの元の状態を示しています。

```sql
SELECT YEAR(timestamp) AS year,
       SUM(commerce.order.priceTotal) AS revenue
FROM purchase_events_dataset
WHERE commerce.purchases.value > 0
GROUP BY 1;
```

フラット化されたデータフィールドを使用する場合は、ドット表記を使用してクエリを記述し、以下に示すように引用符で囲みます。

```sql
SELECT YEAR(timestamp) AS year,
       SUM("commerce.order.priceTotal") AS revenue
FROM purchase_events_dataset
WHERE "commerce.purchases.value" > 0
GROUP BY 1;
```

## 制限事項 {#limitations}

`FLATTEN` 設定では、現在、次のデータ構造をフラット化しません。

| データ構造 | 制限事項 |
|---|---|
| 配列 | 配列にアクセスするには、明示的な配列インデックスまたは `EXPLODE` 関数を使用します。 |
| マップ | マップにアクセスするには、文字列キーを使用してマップの下の値にアクセスします。 |

マップと配列の両方の制限を解決するには、Power BI の詳細オプション／SQL 文などの BI ツールの生の SQL 編集を使用する必要があります。

マップと配列の制限の両方を解決するには、生の SQL 編集などの BI ツールが必要です。SQL 文セクションに [Power BI の詳細オプションを使用してカスタム SQL クエリを入力](../clients/power-bi.md#import-tables-using-custom-sql)する方法については、ガイドを参照してください。

## 次の手順

このドキュメントでは、サードパーティの BI ツールで使用するために、ネストされたデータ構造を統合する方法について説明しました。クライアントをまだ接続していない場合は、[クライアント接続の概要](../clients/overview.md)で、サポートされているサードパーティクライアントのリストを参照してください。
