---
title: クエリサービスでのデータアセット組織のベストプラクティス
description: このドキュメントでは、クエリサービスで使用しやすくするために、データを整理する論理的な方法の概要を説明します。
exl-id: 12d6af99-035a-4f80-b7c0-c6413aa50697
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 0%

---

# クエリサービスでのデータアセットの整理

このドキュメントでは、Adobe Experience Platform クエリサービスで使用するデータセット、ビュー、一時テーブルなど、データアセットを整理するためのベストプラクティスに関するガイダンスを提供します。 データの構造を作成する方法と、この情報にアクセス、更新、削除する方法について説明します。

データアセットの成長に応じて、Experience Platform [!DNL Data Lake] 内でデータアセットを論理的に整理することが重要です。 クエリサービスは、サンドボックス内のデータアセットを論理的にグループ化できる SQL 構成を拡張します。 この編成方法を使用すると、スキーマを物理的に移動しなくても、スキーマ間でデータアセットを共有できます。

## はじめに

このドキュメントを続行する前に、[ クエリサービス ](../home.md) の機能を十分に理解し、[ ユーザーインターフェイスガイド ](../ui/user-guide.md) を読む必要があります。

## クエリサービスでのデータの整理

次の例は、標準の SQL 構文を使用してデータを論理的に整理するためにAdobe Experience Platform クエリサービスを通じて使用できる構成を示しています。 まず、データポイントのコンテナとして機能するデータベースを作成する必要があります。 データベースには 1 つ以上のスキーマを含め、各スキーマには、データアセット（データセット、ビュー、一時テーブルなど）への参照を 1 つ以上含めることができます。 これらの参照には、データセット間の関係や関連付けが含まれます。

クエリサービス UI を使用して SQL クエリを作成する方法について詳しくは、[ クエリエディターユーザーガイド ](../ui/user-guide.md) を参照してください。

サンドボックス内のデータセットを論理的に整理する次の SQL 構成がサポートされています。

```SQL
CREATE DATABASE databaseA;
CREATE SCHEMA databaseA.schema1;
CREATE table t1 ...;
CREATE view v1 ...;
ALTER TABLE t1 ADD PRIMARY KEY (c1) NOT ENFORCED;
ALTER TABLE t2 ADD FOREIGN KEY (c1) REFERENCES t1(c1) NOT ENFORCED;
```

例（簡潔にするために少し切り捨てられています）は、スキーマ `databaseA` を含むこの方法 `schema1` 示しています。

## スキーマへのデータアセットの関連付け

データアセットのコンテナとして機能するスキーマを作成したら、標準の SQL ALTER TABLE 構文を使用して、各データセットをデータベース内の 1 つ以上のスキーマに関連付けることができます。

次の例では、`dataset1`、`dataset2`、`dataset3` および `v1` を前の例で作成した `databaseA.schema1` コンテナに追加します。

```SQL
ALTER TABLE dataset1 ADD SCHEMA databaseA.schema1;
 
ALTER TABLE dataset2 ADD SCHEMA databaseA.schema1;
 
ALTER TABLE dataset3 ADD SCHEMA databaseA.schema1;
 
ALTER VIEW v1  ADD SCHEMA databaseA.schema1;
```

## データコンテナからデータアセットへのアクセス

データベース名を適切に修飾することにより、[!DNL PostgreSQL] クライアントは SHOW キーワードを使用して作成した任意のデータ構造に接続できます。 SHOW キーワードの詳細については、SQL 構文ドキュメント内の [SHOW セクション ](../sql/syntax.md#show) を参照してください。

「all」は、サンドボックス内のすべてのデータベースとスキーマコンテナを含むデフォルトのデータベース名です。 [!DNL PostgreSQL] を使用して `dbname="all"` 接続を作成する場合、作成した **任意** データベースおよびスキーマにアクセスして、データを論理的に整理できます。

`dbname="all"` 下のすべてのデータベースを一覧表示すると、3 つの使用可能なデータベースが表示されます。

```sql
SHOW DATABASES;
  
name     
|---------
databaseA
databaseB
databaseC
```

`dbname="all"` の下のすべてのスキーマをリストすると、サンドボックス内のすべてのデータベースに関連する 3 つのスキーマが表示されます。

```SQL
SHOW SCHEMAS;
  
database       | schema
|----------------------
databaseA      | schema1
databaseA      | schema2
databaseB      | schema3
```

[!DNL PostgreSQL] を使用して `dbname="databaseA"` 接続を作成すると、次の例に示すように、その特定のデータベースに関連付けられている任意のスキーマにアクセスできます。

```sql
SHOW DATABASES;
  
name     
|---------
databaseA
 

SHOW SCHEMAS;
  
database       | schema
|----------------------
databaseA      | schema1
databaseA      | schema2
```

ドット表記を使用すると、選択したデータベースに接続されている特定のスキーマに関連付けられたすべてのテーブルにアクセスできます。 `DBNAME = databaseA.schema1;` に接続すると、その特定のスキーマ（`schema1`）に関連付けられているすべてのテーブルが表示されます。 これにより、どのデータセットにどのテーブルが含まれているかに関する情報が提供されます。

```sql
SHOW DATABASES;
  
name     
|---------
databaseA


SHOW SCHEMAS;
  
database       | schema
|----------------------
databaseA      | schema1


SHOW tables;
name       | type
|----------------------
dataset1| table
dataset2| table
dataset3| table
```

## データコンテナからのデータアセットの更新または削除

組織（またはサンドボックス）のデータアセットの量が増えると、データコンテナからデータアセットを更新または削除する必要が生じます。 個々のアセットは、ドット表記を使用して適切なデータベースとスキーマ名を参照することで、組織コンテナから削除できます。 最初の例で `t1` に追加したテーブルとビュー（それぞれ `v1` と `databaseA.schema1`）は、次の例の構文を使用して削除されます。

```sql
ALTER TABLE databaseA.schema2.t1 REMOVE SCHEMA databaseA.schema2;
ALTER VIEW databaseA.schema2.v1 REMOVE SCHEMA databaseA.schema2;
```

### データアセットの削除

[DROP TABLE](../sql/syntax.md#drop-table) 関数は、組織内のすべてのデータベースにテーブルへの参照が 1 つ存在する場合にのみ、[!DNL Data Lake] ージからデータアセットを物理的に削除します。

```sql
DROP TABLE databaseA.schema2.t1;
```

### データアセットコンテナの削除

標準の SQL 関数を使用して、データベースとスキーマの両方を削除することもできます。

#### データベースの削除

データベースに関連付けられているデータアセットへの参照が他にある場合、データベースを削除しようとすると、関数はエラーをスローします。

```sql
DROP DATABASE databaseA;
```

#### スキーマの削除

スキーマを削除する際に注意すべき 3 つの重要な考慮事項があります。

- スキーマを削除しても、テーブル、ビュー、一時テーブルなどのデータアセットは物理的には削除されません。
- ターゲットスキーマで参照されているデータアセットがあり、モードが RESTRICT の場合、例外がスローされます。
- ターゲットスキーマで参照されているデータアセットがあり、モードが CASCADE の場合、スキーマコンテナで参照されているすべてのデータアセットが削除された後、スキーマコンテナが削除されます。

```sql
DROP SCHEMA databaseA.schema2;
```

## 次の手順

このドキュメントでは、Adobe Experience Platform クエリサービスで使用するデータアセットの編成と構造に関するベストプラクティスについて、より深く理解しました。 クエリサービスのベストプラクティスについては、[ データ重複排除に関するドキュメント ](../key-concepts/deduplication.md) を引き続き参照することをお勧めします。
