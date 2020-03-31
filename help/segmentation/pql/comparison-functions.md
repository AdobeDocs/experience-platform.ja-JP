---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 比較関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0

---


# 比較関数

比較関数は、異なる式と値を比較し、返すか、それに従って使 `true` 用さ `false` れます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## 次と等しい

(等し `=` い)関数は、ある値または式が別の値または式と等しいかどうかを確認します。

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

(等し `!=` くない)関数は、ある値または式が別の値または **式** と等しくないかどうかを確認します。

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

(より `>` 大きい)関数は、最初の値が2番目の値より大きいかどうかを確認するために使用します。

**形式**

```sql
{EXPRESSION} > {EXPRESSION} 
```

**例**

次のPQLクエリでは、1月または2月に誕生日がない人を定義します。

```sql
person.birthMonth > 2
```

## 次よりも大きいか等しい

1つ `>=` 目の値が2つ目の値以上かどうかを確認するには、（より大きいか等しい）関数を使用します。

**形式**

```sql
{EXPRESSION} >= {EXPRESSION} 
```

**例**

次のPQLクエリでは、1月または2月に誕生日がない人を定義します。

```sql
person.birthMonth >= 3
```

## より小さい

比較 `<` 関数（小なり）は、最初の値が2番目の値より小さいかどうかを調べるために使用されます。

**形式**

```sql
{EXPRESSION} < {EXPRESSION} 
```

**例**

次のPQLクエリは、1月の誕生日を持つ人を定義します。

```sql
person.birthMonth < 2
```

## 次よりも小さいか等しい

比較 `<=` 関数（より小さいか等しい）は、最初の値が2番目の値以下かどうかを確認するために使用されます。

**形式**

```sql
{EXPRESSION} <= {EXPRESSION} 
```

**例**

次のPQLクエリは、1月または2月の誕生日を持つ人を定義します。

```sql
person.birthMonth <= 2
```

## 次の手順

これで比較機能について学習できたので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
