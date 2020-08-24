---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ブール関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 84a5b992639c1cabfdeaec5262964c9873826592
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 88%

---


# ブール関数

Boolean functions are used to perform boolean logic on different elements in [!DNL Profile Query Language] (PQL).  More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

## And

`and` 関数は、論理積を作成するために使用されます。

**形式**

```sql
{QUERY} and {QUERY}
```

**例**

次の PQL クエリは、住んでいる国がカナダで、生まれた年が 1985 年のすべての人を返します。

```sql
homeAddress.countryISO = "CA" and person.birthYear = 1985
```

## Or

`or` 関数は、論理和を作成するために使用されます。

**形式**

```sql
{QUERY} or {QUERY}
```

**例**

次の PQL クエリは、住んでいる国がカナダか、生まれた年が 1985 年のすべての人を返します。

```sql
homeAddress.countryISO = "CA" or person.birthYear = 1985
```

## Not

`not`（または `!`）関数は、論理否定を作成するために使用されます。

**形式**

```sql
not ({QUERY})
!({QUERY})
```

**例**

次の PQL クエリは、住んでいる国がカナダでないすべての人を返します。

```sql
not (homeAddress.countryISO = "CA")
```

## If

`if` 関数は、指定した条件が true かどうかに応じて式を解決するために使用されます。

**形式**

```sql
if ({TEST_EXPRESSION}, {TRUE_EXPRESSION}, {FALSE_EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{TEST_EXPRESSION}` | テストされているブール式。 |
| `{TRUE_EXPRESSION}` | `{TEST_EXPRESSION}` が true の場合に値が使用される式。 |
| `{FALSE_EXPRESSION}` | `{TEST_EXPRESSION}` が false の場合に値が使用される式。 |

**例**

次の PQL クエリは、住んでいる国がカナダの場合は値を `1` に設定し、住んでいる国がカナダでない場合は `2` に設定します。

```sql
if (homeAddress.countryISO = "CA", 1, 2)
```

## 次の手順

ここで学習したブール関数は、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
