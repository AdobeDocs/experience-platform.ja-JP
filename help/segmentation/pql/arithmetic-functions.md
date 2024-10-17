---
solution: Experience Platform
title: PAL 演算関数
description: 数論的関数は、Profile Query Language（PQL）の値に対する基本的な計算を実行するために使用されます。
exl-id: 3540ef7c-dbe4-4302-a414-3cf85618f870
source-git-commit: c4d034a102c33fda81ff27bee73a8167e9896e62
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 51%

---

# 演算関数

数論的関数は、[!DNL Profile Query Language] （PQL）の値に対する基本的な計算を実行するために使用されます。 その他のPQL関数について詳しくは、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

## 追加

2 つの引数式の合計を数値として求めるには、`+` （加算）関数を使用します。

**形式**

```sql
{NUMBER} + {NUMBER}
```

**例**

次の PQL クエリは、2 つの異なる製品の価格を合計します。

```sql
product1.price + product2.price
```

## 乗算

`*` （乗算）関数は、2 つの引数式の積を数値として検索するために使用されます。

**形式**

```sql
{NUMBER} * {NUMBER}
```

**例**

次の PQL クエリは、在庫の製品と製品の価格を見つけて、製品の総額を見つけます。

```sql
product.inventory * product.price
```

## 減算

`-` （減算）関数は、2 つの引数式の差を数値として求めるために使用されます。

**形式**

```sql
{NUMBER} - {NUMBER}
```

**例**

次の PQL クエリは、2 つの異なる製品の価格の違いを見つけます。

```sql
product1.price - product2.price
```

## 除算

`/` （除算）関数は、2 つの引数式の商を数値として求めるために使用します。

**形式**

```sql
{NUMBER} / {NUMBER}
```

**例**

次の PQL クエリは、品目ごとの平均コストを確認するために、販売された製品の合計と獲得された合計金額の比率を求めます。

```sql
totalProduct.price / totalProduct.sold
```

## 剰余

`%` （modulo/remainder）関数は、2 つの引数式を数値として除算した後の剰余を求めるために使用します。

**形式**

```sql
{NUMBER} % {NUMBER}
```

**例**

次の PQL クエリは、年齢が 5 で割り切れるかどうかを確認します。

```sql
person.age % 5 = 0
```

## 次の手順

算術関数について学習したので、それらを PQL クエリ内で使用できます。その他の PQL 関数の詳細については、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。
