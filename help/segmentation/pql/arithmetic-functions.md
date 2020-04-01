---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 演算関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0

---


# 演算関数

演算関数は、プロファイルクエリ言語(PQL)の値に対する基本的な計算を実行するために使用されます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## Add

2つ `+` の引数の合計を求めるには、（加算）関数を使用します。式

**形式**

```sql
{NUMBER} + {NUMBER}
```

**例**

次のPQLクエリは、2つの異なる製品の価格を合計します。

```sql
product1.price + product2.price
```

## 乗算

2つ `*` の引数の積を求めるには、乗算関数を使用します。式

**形式**

```sql
{NUMBER} * {NUMBER}
```

**例**

次のPQLクエリは、在庫の製品と製品の価格を見つけて、製品の総価格を見つけます。

```sql
product.inventory * product.price
```

## 減算

2つ `-` の引数の違いを求めるために、（減算）関数が使用されます。

**形式**

```sql
{NUMBER} - {NUMBER}
```

**例**

次のPQLクエリは、2つの異なる製品の価格の違いを見つけます。

```sql
product1.price - product2.price
```

## 除算

（除算） `/` 関数は、2つの引数の式の商を求めます。

**形式**

```sql
{NUMBER} / {NUMBER}
```

**例**

次のPQLクエリは、販売された製品の合計と、品目ごとの平均コストを確認するために獲得した総金額との商を求めます。

```sql
totalProduct.price / totalProduct.sold
```

## 剰余

2つ `%` の引数の式を除算した余りを求めるには、 （剰余/剰余）関数を使用します。

**形式**

```sql
{NUMBER} % {NUMBER}
```

**例**

次のPQLクエリは、その人の年齢を5で割り切れるかどうかを確認します。

```sql
person.age % 5 = 0
```

## 次の手順

これで算術関数の学習が終わり、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
