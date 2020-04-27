---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ブール関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc

---


# ブール関数

Boolean関数は、プロファイルクエリ言語(PQL)の異なる要素に対してBooleanロジックを実行するために使用されます。  その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## AND

この関 `and` 数は、論理接続を作成するために使用されます。

**形式**

```sql
{QUERY} and {QUERY}
```

**例**

次のPQLクエリは、1985年のカナダと生年を母国とするすべての人々を返却します。

```sql
homeAddress.countryISO = "CA" and person.birthYear = 1985
```

## または

この関 `or` 数は、論理的な分離を作成するために使用されます。

**形式**

```sql
{QUERY} or {QUERY}
```

**例**

次のPQLクエリは、1985年のカナダまたは生年を母国とするすべての人々を返却します。

```sql
homeAddress.countryISO = "CA" or person.birthYear = 1985
```

## NOT

(ま `not` たは `!`)関数を使用して、論理否定を作成します。

**形式**

```sql
not ({QUERY})
!({QUERY})
```

**例**

次のPQLクエリは、母国がカナダでないすべての人を返します。

```sql
not (homeAddress.countryISO = "CA")
```

## If

この関数 `if` は、指定した条件が真かどうかに応じて式を解決するために使用されます。

**形式**

```sql
if ({TEST_EXPRESSION}, {TRUE_EXPRESSION}, {FALSE_EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{TEST_EXPRESSION}` | テストするブール式。 |
| `{TRUE_EXPRESSION}` | がtrueの場合に値を使用する式 `{TEST_EXPRESSION}` を指定します。 |
| `{FALSE_EXPRESSION}` | 式の値がfalseの場合に使用 `{TEST_EXPRESSION}` されます。 |

**例**

次のPQLクエリは、国がカナダで `1` 、国がカナダでない場合 `2` に値を設定します。

```sql
if (homeAddress.countryISO = "CA", 1, 2)
```

## 次の手順

これで、ブール関数について学習したので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
