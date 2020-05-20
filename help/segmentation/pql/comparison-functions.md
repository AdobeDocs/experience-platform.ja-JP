---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 比較関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 10%

---


# 比較関数

比較関数は、異なる式と値を比較し、戻り値 `true` または `false` それに応じて返します。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## 次と等しい

( `=` 等しい)関数は、ある値または式が別の値または式と等しいかどうかを確認します。

**形式**

```sql
{EXPRESSION} = {VALUE}
```

**例**

次のPQLクエリは、自宅住所の国がカナダにあるかどうかを確認します。

```sql
homeAddress.countryISO = "CA"
```

## 等しくない

( `!=` 等しくない)関数は、ある値または式が別の値または式と等し **** くないかどうかをチェックします。

**形式**

```sql
{EXPRESSION} != {VALUE}
```

**例**

次のPQLクエリは、自宅住所の国がカナダにないかどうかを確認します。

```sql
homeAddress.countryISO != "CA"
```

## より大きい

1つ目の値 `>` が2つ目の値より大きいかどうかを確認するには、（より大きい）関数を使用します。

**形式**

```sql
{EXPRESSION} > {EXPRESSION} 
```

**例**

次のPQLクエリは、誕生日が1月または2月に該当しない人を定義します。

```sql
person.birthMonth > 2
```

## 次よりも大きいか等しい

1つ目の値 `>=` が2つ目の値以上かどうかを確認するには、（より大きいか等しい）関数を使用します。

**形式**

```sql
{EXPRESSION} >= {EXPRESSION} 
```

**例**

次のPQLクエリは、誕生日が1月または2月に該当しない人を定義します。

```sql
person.birthMonth >= 3
```

## より小さい

比較関数 `<` （より小さい）を使用して、1つ目の値が2つ目の値より小さいかどうかを確認します。

**形式**

```sql
{EXPRESSION} < {EXPRESSION} 
```

**例**

次のPQLクエリは、1月の誕生日を持つユーザーを定義します。

```sql
person.birthMonth < 2
```

## 次よりも小さいか等しい

比較関数 `<=` （より小さいか等しい）を使用して、1つ目の値が2つ目の値以下かどうかを確認します。

**形式**

```sql
{EXPRESSION} <= {EXPRESSION} 
```

**例**

次のPQLクエリは、1月または2月の誕生日を持つユーザーを定義します。

```sql
person.birthMonth <= 2
```

## 次の手順

比較機能について学んだので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
