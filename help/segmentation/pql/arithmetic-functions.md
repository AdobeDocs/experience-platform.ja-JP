---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;Segmentation Service;pql;PQL;Profile Query Language;arithmetic functions;arithmetic;
solution: Experience Platform
title: 演算関数
topic: developer guide
description: 演算関数 は、プロファイルクエリ言語(PQL)の値に対して基本的な計算を実行するために使用します。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 85%

---


# 演算関数

演算関数 は、 [!DNL Profile Query Language] (PQL)の値に対して基本的な計算を実行するために使用します。 More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

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
