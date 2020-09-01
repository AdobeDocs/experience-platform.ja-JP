---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;Segmentation Service;pql;PQL;Profile Query Language;object functions;object;
solution: Experience Platform
title: オブジェクト関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 80%

---


# オブジェクト関数

[!DNL Profile Query Language] (PQL)オファーは、オブジェクトとのやり取りを簡単にするために機能します。 More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

## Isnull

`isNull` 関数は、オブジェクト参照が存在しないかどうかを判定します。

**形式**

```sql
{OBJECT}.isNull()
```

**例**

次の PQL クエリは、ユーザーの自宅住所が存在しないかどうかを確認します。

```sql
person.homeAddress.isNull()
```

## isNotNull

`isNotNull` 関数は、オブジェクト参照が存在するかどうかを判定します。

**形式**

```sql
{OBJECT}.isNotNull()
```

**例**

次の PQL クエリは、ユーザーの自宅住所が存在するかどうかを確認します。

```sql
person.homeAddress.isNotNull()
```

## 次の手順

ここで学習したオブジェクト関数は、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。