---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 集計関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 21%

---


# 集計関数

集計関数は、 [!DNL Profile Query Language] (PQL)アレイ内の複数の値をグループ化して単一の要約値を形成するために使用します。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## 回数

この `count` 関数は、渡された配列内の要素数を返します。

**書式**

```sql
{ARRAY}.count()
```

**例**

次のPQLクエリは、配列内の注文件数を返します。

```sql
orders.count()
```

## Sum

この `sum` 関数は、配列内で選択されたすべての値の合計を返します。

**書式**

```sql
{ARRAY}.sum()
```

**例**

次のPQLクエリは、すべての注文価格の合計を返します。

```sql
orders.sum(order.price)
```

## 平均

この `average` 関数は、配列内で選択されたすべての値の算術平均を返します。

**書式**

```sql
{ARRAY}.average()
```

**例**

次のPQLクエリは、すべての注文の平均価格を返します。

```sql
orders.average(order.price)
```

## 最小

この `min` 関数は、配列内で選択されたすべての値の中で最も小さい値を返します。

**書式**

```sql
{ARRAY}.min()
```

**例**

次のPQLクエリは、すべての注文の中で最も安い価格を返します。

```sql
orders.min(order.price)
```

## 最大

この `max` 関数は、配列内で選択されたすべての値の中で最も大きい値を返します。

**書式**

```sql
{ARRAY}.max()
```

**例**

次のPQLクエリは、すべての注文の中で最高の価格を返します。

```sql
orders.max(order.price)
```

## 次の手順

集約関数の学習が終わったので、PQLクエリ内で使用できます。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
