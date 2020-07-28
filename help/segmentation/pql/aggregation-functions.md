---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 集計関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 91%

---


# 集計関数

Aggregation functions are used to group together multiple values within [!DNL Profile Query Language] (PQL) arrays to form a single summary value. その他の PQL 関数の詳細については、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

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
