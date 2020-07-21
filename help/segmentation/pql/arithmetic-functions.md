---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 演算関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 18%

---


# 演算関数

演算関数は、 [!DNL Profile Query Language] (PQL)の値に対して基本的な演算を実行するために使用します。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## Add

2つの引数式の合計を求めるには、 `+` （加算）関数を使用します。

**書式**

```sql
{NUMBER} + {NUMBER}
```

**例**

次のPQLクエリは、2つの異なる製品の価格を合計しています。

```sql
product1.price + product2.price
```

## 乗算

2つの引数式の積を求めるには、 `*` （乗算）関数を使用します。

**書式**

```sql
{NUMBER} * {NUMBER}
```

**例**

次のPQLクエリは、在庫の積と製品の価格を検索して、製品の総価値を見つけます。

```sql
product.inventory * product.price
```

## 減算

2つの引数式の違いを見つけるには、 `-` （減算）関数を使用します。

**書式**

```sql
{NUMBER} - {NUMBER}
```

**例**

次のPQLクエリは、2つの異なる製品の価格の違いを見つけ出します。

```sql
product1.price - product2.price
```

## 除算

2つの引数式の商を求めるのに `/` （除算）関数を使用します。

**書式**

```sql
{NUMBER} / {NUMBER}
```

**例**

次のPQLクエリは、販売された製品の合計と、品目あたりの平均原価を確認するための獲得金額の合計の商を見つけます。

```sql
totalProduct.price / totalProduct.sold
```

## 剰余

2つの引数式を除算した後の剰余を求めるには、 `%` (modulo/remainder)関数を使用します。

**書式**

```sql
{NUMBER} % {NUMBER}
```

**例**

次のPQLクエリは、その人の年齢を5で割り切れるかどうかをチェックします。

```sql
person.age % 5 = 0
```

## 次の手順

これで算術関数の学習が終わり、PQLクエリ内で使用できます。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
