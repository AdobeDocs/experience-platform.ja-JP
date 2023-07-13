---
solution: Experience Platform
title: PQL 集計関数
description: 集約関数は、プロファイルクエリ言語（PQL）配列内で複数の値をグループ化して単一の要約値を形成するために使用されます。
exl-id: 6c0c0f6d-98c5-4b5d-b440-3e5e18c0f34b
source-git-commit: dbb7e0987521c7a2f6512f05eaa19e0121aa34c6
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 93%

---

# 集計関数

集計関数は、[!DNL Profile Query Language]（PQL）配列内の複数の値をグループ化して、単一の要約値を形成するために使用されます。その他の PQL 関数について詳しくは、 [[!DNL Profile Query Language] 概要](./overview.md).

## カウント

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
