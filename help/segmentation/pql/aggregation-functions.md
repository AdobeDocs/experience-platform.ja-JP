---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 集計関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc

---


# 集計関数

集約関数は、プロファイルクエリ言語(PQL)配列内で複数の値をグループ化して単一の要約値を形成するために使用されます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## 回数

この関 `count` 数は、渡された配列内の要素数を返します。

**形式**

```sql
{ARRAY}.count()
```

**例**

次のPQLクエリは、配列内の注文数を返します。

```sql
orders.count()
```

## Sum

この関 `sum` 数は、配列内で選択されたすべての値の合計を返します。

**形式**

```sql
{ARRAY}.sum()
```

**例**

次のPQLクエリは、すべての注文の価格の合計を返します。

```sql
orders.sum(order.price)
```

## 平均

この関 `average` 数は、配列内で選択されたすべての値の算術平均を返します。

**形式**

```sql
{ARRAY}.average()
```

**例**

次のPQLクエリは、すべての注文の平均価格を返します。

```sql
orders.average(order.price)
```

## 最小

この関 `min` 数は、配列内で選択されたすべての値の中で最も小さい値を返します。

**形式**

```sql
{ARRAY}.min()
```

**例**

次のPQLクエリは、すべての注文の最低価格を返します。

```sql
orders.min(order.price)
```

## 最大

この関 `max` 数は、配列内で選択されたすべての値の中で最も大きい値を返します。

**形式**

```sql
{ARRAY}.max()
```

**例**

次のPQLクエリは、すべての注文の最高価格を返します。

```sql
orders.max(order.price)
```

## 次の手順

これで、集約関数の学習が終わったので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
