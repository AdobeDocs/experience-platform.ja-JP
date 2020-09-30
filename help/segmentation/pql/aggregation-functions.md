---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;Segmentation Service;pql;PQL;Profile Query Language;aggregation functions;aggregation;
solution: Experience Platform
title: 集計関数
topic: developer guide
description: '集計関数 は、プロファイルクエリ言語(PQL)配列内の複数の値をグループ化して単一の概要値を形成するために使用します。 '
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 79%

---


# 集計関数

集計関数 are used to group together multiple values within [!DNL Profile Query Language] (PQL) arrays to form a single summary value. More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

## Count

`count` 関数は、渡された配列内の要素数を返します。

**形式**

```sql
{ARRAY}.count()
```

**例**

次の PQL クエリは、配列内の注文の数を返します。

```sql
orders.count()
```

## Sum

`sum` 関数は、配列内の選択された値すべての合計を返します。

**形式**

```sql
{ARRAY}.sum()
```

**例**

次の PQL クエリは、すべての注文の価格の合計を返します。

```sql
orders.sum(order.price)
```

## Average

`average` 関数は、配列内の選択された値すべての算術平均を返します。

**形式**

```sql
{ARRAY}.average()
```

**例**

次の PQL クエリは、すべての注文の平均価格を返します。

```sql
orders.average(order.price)
```

## Minimum

`min` 関数は、配列内の選択された値すべての最小値を返します。

**形式**

```sql
{ARRAY}.min()
```

**例**

次の PQL クエリは、すべての注文の最低価格を返します。

```sql
orders.min(order.price)
```

## Maximum

`max` 関数は、配列内の選択された値すべての最大値を返します。

**形式**

```sql
{ARRAY}.max()
```

**例**

次の PQL クエリは、すべての注文の最高価格を返します。

```sql
orders.max(order.price)
```

## 次の手順

これで、集計関数について学習し、PQL クエリ内で使用できるようになりました。その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
