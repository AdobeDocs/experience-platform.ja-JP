---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ブール関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 6%

---


# ブール関数

Boolean関数は、プロファイルクエリ言語(PQL)の異なる要素に対してBooleanロジックを実行するために使用します。  その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## AND

この `and` 関数は、論理的な関数の作成に使用されます。

**形式**

```sql
{QUERY} and {QUERY}
```

**例**

次のPQLクエリは、1985年のカナダと生年を母国とするすべての人々を返します。

```sql
homeAddress.countryISO = "CA" and person.birthYear = 1985
```

## または

この `or` 関数は、論理的な分離を作成するために使用されます。

**形式**

```sql
{QUERY} or {QUERY}
```

**例**

次のPQLクエリは、1985年のカナダまたは生年を母国とするすべての人々を返します。

```sql
homeAddress.countryISO = "CA" or person.birthYear = 1985
```

## NOT

論理否定 `not` の作成には、(または `!`)関数を使用します。

**形式**

```sql
not ({QUERY})
!({QUERY})
```

**例**

次のPQLクエリは、母国をカナダとして持っていないすべての人に返します。

```sql
not (homeAddress.countryISO = "CA")
```

##   

この `if` 関数は、指定した条件が真かどうかに応じて式を解決するために使用されます。

**形式**

```sql
if ({TEST_EXPRESSION}, {TRUE_EXPRESSION}, {FALSE_EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{TEST_EXPRESSION}` | テストするブール式です。 |
| `{TRUE_EXPRESSION}` | がtrueの場合に値を使用する式 `{TEST_EXPRESSION}` 。 |
| `{FALSE_EXPRESSION}` | 値がfalseの場合に値を使用する式 `{TEST_EXPRESSION}` です。 |

**例**

次のPQLクエリは、国がカナダ、国がカナダでない場合 `1` に値をカナダ `2` として設定します。

```sql
if (homeAddress.countryISO = "CA", 1, 2)
```

## 次の手順

ブール関数について学習したので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
