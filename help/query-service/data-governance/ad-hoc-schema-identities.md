---
title: アドホックプライマリセットでのデータ ID の設定
description: Adobe Experience Platformクエリサービスを使用すると、SQL ALTER TABLE コマンドを使用して、アドホックスキーマデータセットフィールドの ID またはプライマリ ID を直接設定できます。 このドキュメントでは、ALTER TABLE コマンドを使用して、プライマリ ID またはセカンダリ ID を設定する方法を説明します。
source-git-commit: bf51fc3e0c9635c0555f87f3389fb4a9542c092d
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 1%

---

# アドホックデータセットにプライマリ ID を設定する

Adobe Experience Platformクエリサービスを使用すると、SQL の制約を使用して、データセット列をプライマリ ID またはセカンダリ ID としてマークできます `ALTER TABLE` コマンドを使用します。 この機能を使用すると、フラグ付けされたフィールドがデータプライバシー要件と一致するようにできます。 このコマンドを使用すると、プライマリ ID テーブル列とセカンダリ ID テーブル列の両方に対する制約を SQL を介して直接追加または削除できます。

## はじめに

データセット列をプライマリ ID またはセカンダリ ID としてラベル付けするには、 `ALTER TABLE` SQL コマンドを使用し、データのプライバシー要件を十分に理解しておく必要があります。 このドキュメントを続行する前に、次のドキュメントを確認してください。

* [の SQL 構文ガイド `ALTER TABLE` command](../sql/syntax.md).
* [データガバナンスの概要](../../data-governance/home.md) を参照してください。

## 制約を追加 {#add-constraints}

この `ALTER TABLE` コマンドを使用すると、データセット列に個人の ID としてラベルを付け、SQL を使用して関連付けられたメタデータを更新することで、そのラベルをプライマリ ID として使用できます。 これは、Platform UI を使用してスキーマから直接データセットを作成するのではなく、SQL を使用してデータセットを作成する場合に特に便利です。 コマンドを使用すると、Platform 内のデータ操作がデータ使用ポリシーに準拠していることを確認できます。

**例**

次の使用例は、既存の `t1` 表。 の値 `id` 列は、の下でプライマリ ID としてマークされるようになりました。 `IDFA` 名前空間。 ID 名前空間は、フィールドが表す ID データの種類を宣言するキーワードです。

```sql
ALTER TABLE t1 ADD CONSTRAINT PRIMARY IDENTITY (id) NAMESPACE 'IDFA';
```

2 つ目の例では、 `id` 列はセカンダリ ID としてマークされます。

```sql
ALTER TABLE t1 ADD CONSTRAINT IDENTITY(id) NAMESPACE 'IDFA';
```

## ドロップ制約 {#drop-constraints}

制約は、 `ALTER TABLE` コマンドを使用します。

**例**

次の例では、 `c1` 列には、既存の `t1` 表。

```sql
ALTER TABLE t1 DROP CONSTRAINT PRIMARY IDENTITY (c1) ;
```

以下に示すように、ID 制約を削除する場合も同じ構文が使用されます。

```sql
ALTER TABLE t1 DROP CONSTRAINT IDENTITY (c1) ;
```

## ID を表示

metadata コマンドを使用します。 `show identities` コマンドラインインタフェースから、id として割り当てられているすべての属性を持つテーブルを表示します。

```shell
> show identities;
```

次に、返されるテーブルの例を示します。

```console
 tableName | columnName | datatype | namespace | ifPrimary
-----------+------------+----------+-----------+----------
(0 rows)
```

## XDM の制限 {#limitations}

次のリストでは、XDM を使用する際に既存のデータセット内の ID を更新する際に考慮すべき重要な事項について説明します。

* 列を ID として指定するには、次の手順を実行します。 **必須** また、列のメタデータとして保持する名前空間も定義します。
* XDM では、namespace 属性で列名を指定することはできません。
* スキーマが `identityMap` XDM フィールド（ルートまたは最上位レベル） `identityMap` object **必須** は、id またはプライマリ id としてラベル付けされます。
