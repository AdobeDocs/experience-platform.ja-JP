---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；演算関数；算術演算；
solution: Experience Platform
title: PAL 演算関数
topic-legacy: developer guide
description: 演算関数は、プロファイルクエリ言語（PQL）の値に対する基本的な計算を実行するために使用されます。
exl-id: 3540ef7c-dbe4-4302-a414-3cf85618f870
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 87%

---

# 演算関数

数論的関数は、[!DNL Profile Query Language]（PQL）の値に対する基本的な計算を実行するために使用されます。その他の PQL 関数について詳しくは、 [[!DNL Profile Query Language] 概要](./overview.md).

## 加算

`+`（加算）関数は、2 つの引数式の合計を見つけるために使用されます。

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

`*`（乗算）関数は、2 つの引数式の積を求めるために使用されます。

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

`-`（減算）関数は、2 つの引数式の違いを見つけるために使用されます。

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

`/`（除算）関数は、2 つの引数式の商を見つけるために使用されます。

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

`%`（モジュロ/剰余）関数は、2 つの引数式を除算した後の剰余を見つけるために使用されます。

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
