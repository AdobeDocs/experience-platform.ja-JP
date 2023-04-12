---
title: クエリサービスでのデータアセットの組織のベストプラクティス
description: このドキュメントでは、クエリサービスで使いやすくするためにデータを整理する論理的な方法について説明します。
exl-id: 12d6af99-035a-4f80-b7c0-c6413aa50697
source-git-commit: fcd44aef026c1049ccdfe5896e6199d32b4d1114
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# クエリサービスでのデータアセットの整理

このドキュメントでは、Adobe Experience Platformクエリサービスで使用するデータアセット（データセット、ビュー、一時テーブルなど）を整理するためのベストプラクティスに関するガイダンスを提供します。 データの構造化方法と、この情報へのアクセス、更新、削除方法に関する情報について説明します。

Platform 内のデータアセットを論理的に整理することが重要です [!DNL Data Lake] 成長するにつれて クエリサービスは、サンドボックス内のデータアセットを論理的にグループ化できる SQL 構成を拡張します。 この編成方法を使用すると、データアセットを物理的に移動する必要なく、スキーマ間でデータアセットを共有できます。

## はじめに

このドキュメントを続行する前に、 [クエリサービス](../home.md) 機能と読み取り [ユーザーインターフェイスガイド](../ui/user-guide.md).

## クエリサービスでのデータの整理

次の例は、Adobe Experience Platformクエリサービスを通じて、標準の SQL 構文を使用してデータを論理的に整理するために使用できる構成を示しています。 最初に、データポイントのコンテナとして機能するデータベースを作成する必要があります。 データベースには 1 つ以上のスキーマを含め、各スキーマには 1 つ以上のデータアセットへの参照（データセット、ビュー、一時テーブルなど）を含めることができます。 これらの参照には、データセット間の関係や関連付けが含まれます。

詳しくは、 [クエリエディターユーザーガイド](../ui/user-guide.md) を参照してください。

サンドボックス内のデータセットを論理的に整理する次の SQL 構成がサポートされています。

```SQL
CREATE DATABASE databaseA;
CREATE SCHEMA databaseA.schema1;
CREATE table t1 ...;
CREATE view v1 ...;
ALTER TABLE t1 ADD PRIMARY KEY (c1) NOT ENFORCED;
ALTER TABLE t2 ADD FOREIGN KEY (c1) REFERENCES t1(c1) NOT ENFORCED;
```

この例（簡潔にするために少し切り捨てられた）は、次の方法を示しています。 `databaseA` スキーマを含む `schema1`.

## データアセットのスキーマへの関連付け

データアセットのコンテナとして機能するスキーマを作成したら、標準の SQL ALTER TABLE 構文を使用して、各データセットをデータベース内の 1 つ以上のスキーマに関連付けることができます。

次の例では、 `dataset1`, `dataset2`, `dataset3` および `v1` から `databaseA.schema1` 前の例で作成したコンテナ。

```SQL
ALTER TABLE dataset1 SET SCHEMA databaseA.schema1;
 
ALTER TABLE dataset2 SET SCHEMA databaseA.schema1;
 
ALTER TABLE dataset3 SET SCHEMA databaseA.schema1;
 
ALTER VIEW v1  SET SCHEMA databaseA.schema1;
```

## データコンテナからのデータアセットへのアクセス

データベース名を適切に絞り込むことで、 [!DNL PostgreSQL] クライアントは、 SHOW キーワードを使用して作成した任意のデータ構造に接続できます。 SHOW キーワードについて詳しくは、 [SQL 構文ドキュメント内の SHOW セクション](../sql/syntax.md#show).

「all」は、サンドボックス内のすべてのデータベースとスキーマコンテナを含むデフォルトのデータベース名です。 を [!DNL PostgreSQL] 接続 `dbname="all"`、 **任意** データを論理的に整理するために作成したデータベースおよびスキーマ。

の下にすべてのデータベースをリスト `dbname="all"` は、3 つの使用可能なデータベースを表示します。

```sql
SHOW DATABASES;
  
name     
---------
databaseA
databaseB
databaseC
```

の下にすべてのスキーマをリストする `dbname="all"` は、サンドボックス内の各データベースに関連する 3 つのスキーマを表示します。

```SQL
SHOW SCHEMAS;
  
database       | schema
----------------------
databaseA      | schema1
databaseA      | schema2
databaseB      | schema3
```

を [!DNL PostgreSQL] 接続 `dbname="databaseA"`を使用すると、次の例に示すように、特定のデータベースに関連付けられている任意のスキーマにアクセスできます。

```sql
SHOW DATABASES;
  
name     
---------
databaseA
 

SHOW SCHEMAS;
  
database       | schema
----------------------
databaseA      | schema1
databaseA      | schema2
```

ドット表記を使用すると、選択したデータベースに接続された特定のスキーマに関連付けられたすべてのテーブルにアクセスできます。 に接続する `DBNAME = databaseA.schema1;`、その特定のスキーマに関連付けられたすべてのテーブル (`schema1`) が表示されます。 これは、どのテーブルが含まれるデータセットに関する情報を提供します。

```sql
SHOW DATABASES;
  
name     
---------
databaseA


SHOW SCHEMAS;
  
database       | schema
----------------------
databaseA      | schema1


SHOW tables;
name       | type
----------------------
dataset1| table
dataset2| table
dataset3| table
```

## データコンテナでのデータアセットの更新または削除

組織（またはサンドボックス）内のデータアセットの量が増えるにつれ、データコンテナのデータアセットを更新または削除する必要が生じます。 ドット表記を使用して適切なデータベースおよびスキーマ名を参照することで、個々のアセットを組織コンテナから削除できます。 テーブルとビュー (`t1` および `v1` ) を `databaseA.schema1` 最初の例では、次の例の構文を使用してが削除されます。

```sql
ALTER TABLE databaseA.schema2.t1 REMOVE SCHEMA databaseA.schema2;
ALTER VIEW databaseA.schema2.v1 REMOVE SCHEMA databaseA.schema2;
```

### データアセットを削除

この [ドロップテーブル](../sql/syntax.md#drop-table) 関数は、 [!DNL Data Lake] テーブルへの単一の参照が組織内のすべてのデータベースに存在する場合。

```sql
DROP TABLE databaseA.schema2.t1;
```

### データアセットコンテナの削除

標準の SQL 関数を使用して、データベースとスキーマの両方を削除することもできます。

#### データベースの削除

データベースに関連付けられたデータアセットへの他の参照がある場合、この関数は、データベースを削除しようとするとエラーをスローします。

```sql
DROP DATABASE databaseA;
```

#### スキーマの削除

スキーマを削除する際に注意すべき重要な点は次の 3 つです。

- スキーマを削除しても、テーブル、ビュー、一時テーブルなどのデータアセットは物理的に削除されません。
- ターゲットスキーマ内で参照されるデータアセットがあり、モードが RESTRICT の場合、例外がスローされます。
- ターゲットスキーマ内で参照されるデータアセットがあり、モードが CASCADE の場合、スキーマコンテナによって参照されるすべてのデータアセットが削除され、スキーマコンテナが削除されます。

```sql
DROP SCHEMA databaseA.schema2;
```

## 次の手順

このドキュメントでは、Adobe Experience Platformクエリサービスで使用するデータアセットの構成と構造に関するベストプラクティスについて、より深く理解できました。 クエリサービスのベストプラクティスについては、 [データ重複排除ドキュメント](../essential-concepts/deduplication.md).
