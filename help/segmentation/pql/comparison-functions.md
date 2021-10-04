---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；比較関数；比較；
solution: Experience Platform
title: PQL 比較関数
topic-legacy: developer guide
description: 比較関数は、異なる式と値を比較するために使用され、それに応じて「true」または「false」を返します。
exl-id: 15f106c7-b88b-4042-b925-703e2a309573
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 85%

---

# 比較関数

比較関数は、異なる式と値を比較するために使用され、それに応じて`true`または`false`を返します。その他の PQL 関数の詳細については、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

## Equals

`=`（equals）関数は、ある値または式が別の値または式と等しいかどうかを確認します。

**形式**

```sql
{EXPRESSION} = {VALUE}
```

**例**

次の PQL クエリは、自宅住所の国がカナダにあるかどうかを確認します。

```sql
homeAddress.countryISO = "CA"
```

## Not equal

`!=`（not equal）関数は、ある値または式が別の値または式と等しく&#x200B;**ない**&#x200B;かどうかを確認します。

**形式**

```sql
{EXPRESSION} != {VALUE}
```

**例**

次の PQL クエリは、自宅住所の国がカナダにないかどうかを確認します。

```sql
homeAddress.countryISO != "CA"
```

## Greater than

`>`（greater than）関数は、最初の値が 2 番目の値より大きいかどうかを確認するために使用します。

**形式**

```sql
{EXPRESSION} > {EXPRESSION} 
```

**例**

次の PQL クエリでは、誕生日が 1 月または 2 月に該当しない人を定義します。

```sql
person.birthMonth > 2
```

## Greater than or equal to

`>=`（greater than or equal to）は、1 つ目の値が 2 つ目の値以上かどうかを確認するために使用されます。

**形式**

```sql
{EXPRESSION} >= {EXPRESSION} 
```

**例**

次の PQL クエリでは、誕生日が 1 月または 2 月に該当しない人を定義します。

```sql
person.birthMonth >= 3
```

## Less than

`<`（less than）比較関数は、最初の値が 2 番目の値より小さいかどうかを調べるために使用されます。

**形式**

```sql
{EXPRESSION} < {EXPRESSION} 
```

**例**

次の PQL クエリは、誕生日が 1 月に該当する人を定義します。

```sql
person.birthMonth < 2
```

## Less than or equal to

`<=`（less than or equal to）比較関数は、最初の値が 2 番目の値以下かどうかを確認するために使用されます。

**形式**

```sql
{EXPRESSION} <= {EXPRESSION} 
```

**例**

次の PQL クエリは、誕生日が 1 月または 2 月に該当する人を定義します。

```sql
person.birthMonth <= 2
```

## 次の手順

比較機能について学習したので、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。
