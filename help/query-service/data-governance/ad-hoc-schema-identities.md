---
title: アドホックデータセットでのプライマリ Id の設定
description: Adobe Experience Platform クエリサービスでは、SQL ALTER TABLE コマンドを使用して、アドホックスキーマデータセットフィールドの ID またはプライマリ ID を直接設定できます。 この文書では、ALTER TABLE コマンドを使用してプライマリ ID またはセカンダリ ID を設定する方法について説明します。
exl-id: b8e6b87e-c6e5-4688-a936-a3a1510a3c5b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 1%

---

# アドホックデータセットへのプライマリ ID の設定

Adobe Experience Platform クエリサービスでは、SQL `ALTER TABLE` コマンドの制約を使用して、データセット列をプライマリまたはセカンダリ ID としてマークできます。 この機能を使用すると、フラグが設定されたフィールドとデータプライバシー要件を確実に一致させることができます。 このコマンドを使用すると、プライマリとセカンダリの両方の ID テーブル列の制約を追加または削除できます。SQL を直接使用します。

## はじめに

データセット列をプライマリまたはセカンダリ ID としてラベル付けするには、`ALTER TABLE` SQL コマンドを理解し、データプライバシー要件を十分に理解している必要があります。 このドキュメントを続行する前に、次のドキュメントを確認してください。

* [`ALTER TABLE` コマンドの SQL 構文ガイド &#x200B;](../sql/syntax.md)
* 詳しくは、[&#x200B; データガバナンスの概要 &#x200B;](../../data-governance/home.md) を参照してください。

## 制約を追加 {#add-constraints}

`ALTER TABLE` コマンドを使用すると、データセット列にユーザーの ID としてラベルを付け、SQL を使用して関連メタデータを更新することで、そのラベルをプライマリ ID として使用できます。 これは、Experience Platform UI を使用してスキーマから直接作成するのではなく、SQL を使用してデータセットを作成する場合に特に便利です。 コマンドを使用すると、Experience Platform内のデータ操作がデータ使用ポリシーに準拠していることを確認できます。

**例**

次の例では、既存の `t1` テーブルに制約を追加します。 `id` 列の値は、`IDFA` 名前空間の下でプライマリ ID としてマークされるようになりました。 ID 名前空間は、フィールドが表す ID データのタイプを宣言するキーワードです。

```sql
ALTER TABLE t1 ADD CONSTRAINT PRIMARY IDENTITY (id) NAMESPACE 'IDFA';
```

2 番目の例では、`id` 列がセカンダリ ID としてマークされていることを確認します。

```sql
ALTER TABLE t1 ADD CONSTRAINT IDENTITY(id) NAMESPACE 'IDFA';
```

## ドロップ制約 {#drop-constraints}

`ALTER TABLE` コマンドを使用して、テーブルの列から制約を削除することもできます。

**例**

次の例では、`c1` 列に既存の `t1` テーブルのプライマリ ID というラベルを付ける必要がなくなります。

```sql
ALTER TABLE t1 DROP CONSTRAINT PRIMARY IDENTITY (c1) ;
```

以下に示すように、ID 制約を削除する場合も同じ構文を使用します。

```sql
ALTER TABLE t1 DROP CONSTRAINT IDENTITY (c1) ;
```

## ID を表示

コマンドラインインターフェイスからメタデータコマンド `show identities` を使用して、ID として割り当てられているすべての属性を含むテーブルを表示します。

```shell
> show identities;
```

返されるテーブルの例を以下に示します。

```console
 tableName | columnName | datatype | namespace | ifPrimary
-----------+------------+----------+-----------+----------
(0 rows)
```

## XDM の制限 {#limitations}

次のリストでは、XDM を使用する場合に既存のデータセットで ID を更新する際の重要な考慮事項について説明します。

* 列を ID として指定するには、列のメタデータとして保持する名前空間も定義する **必要** があります。
* XDM は、名前空間属性での列名の指定をサポートしていません。
* スキーマで `identityMap` XDM フィールドを使用する場合、ルートまたは最上位の `identityMap` オブジェクトは **必須** ID またはプライマリ ID としてラベル付けされる必要があります。
