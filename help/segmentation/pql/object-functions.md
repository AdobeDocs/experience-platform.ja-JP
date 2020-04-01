---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクト関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0

---


# オブジェクト関数

プロファイルクエリ言語(PQL)オファーは、オブジェクトとのやり取りをより簡単にするための機能です。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## Nullである

この関 `isNull` 数は、オブジェクト参照が存在しないかどうかを判定します。

**形式**

```sql
{OBJECT}.isNull()
```

**例**

次のPQLクエリは、ユーザーの自宅住所が存在しないかどうかを確認します。

```sql
person.homeAddress.isNull()
```

## nullでない

この関数 `isNotNull` は、オブジェクト参照が存在するかどうかを判定します。

**形式**

```sql
{OBJECT}.isNotNull()
```

**例**

次のPQLクエリは、ユーザーの自宅住所が存在するかどうかを確認します。

```sql
person.homeAddress.isNotNull()
```

## 次の手順

これで、オブジェクトの機能について学習したので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。