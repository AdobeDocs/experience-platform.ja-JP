---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；ql;PQL;プロファイルクエリ言語；文字列関数；文字列；
solution: Experience Platform
title: PQL文字列関数
topic: developer guide
description: プロファイルクエリ言語（PQL）には、文字列の操作が簡単になる関数が用意されています。
translation-type: tm+mt
source-git-commit: b3defc3e33a55855e307ab70b9797d985d5719e3
workflow-type: tm+mt
source-wordcount: '784'
ht-degree: 94%

---


# 文字列関数

[!DNL Profile Query Language] (PQL)オファーは、文字列とのやり取りを簡単にするための機能です。他のPQL関数の詳細については、[[!DNL Profile Query Language] 概要](./overview.md)を参照してください。

## like

`like` 関数は、文字列が指定のパターンと一致するかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1} like {STRING_2}
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列列と照合される式です。式の作成に使用できる特殊文字として、`%` と `_` の 2 つがサポートされています。 <ul><li>`%` は、0 個以上の文字を表すために使用されます。</li><li>`_` は、1 文字を表すために使用されます。</li></ul> |

**例**

次の PQL クエリでは、「es」というパターンを含むすべての市区町村を取得します。

```sql
city like "%es%"
```

## startsWith

`startsWith` 関数は、文字列が指定の部分文字列で始まるかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.startsWith({STRING_2}, {BOOLEAN})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列内で検索する文字列です。 |
| `{BOOLEAN}` | チェックで大文字と小文字が区別されるかどうかを指定するオプションのパラメーターです。デフォルトでは true に設定されています。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の名前が「Joe」で始まるかどうかを判定します。

```sql
person.name.startsWith("Joe")
```

## doesNotStartWith

`doesNotStartWith` 関数は、文字列が指定の部分文字列で始まらないかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.doesNotStartWith({STRING_2}, {BOOLEAN})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列内で検索する文字列です。 |
| `{BOOLEAN}` | チェックで大文字と小文字が区別されるかどうかを指定するオプションのパラメーターです。デフォルトでは true に設定されています。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の名前が「Joe」で始まらないかどうかを判断します。

```sql
person.name.doesNotStartWith("Joe")
```

## endsWith

`endsWith` 関数は、文字列が指定の部分文字列で終わるかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.endsWith({STRING_2}, {BOOLEAN})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列内で検索する文字列です。 |
| `{BOOLEAN}` | チェックで大文字と小文字が区別されるかどうかを指定するオプションのパラメーターです。デフォルトでは true に設定されています。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の E メールアドレスが「.com」で終わるかどうかを判定します。

```sql
person.emailAddress.endsWith(".com")
```

## doesNotEndWith

`doesNotEndWith` 関数は、文字列が指定の部分文字列で終わらないかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.doesNotEndWith({STRING_2}, {BOOLEAN})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列内で検索する文字列です。 |
| `{BOOLEAN}` | チェックで大文字と小文字が区別されるかどうかを指定するオプションのパラメーターです。デフォルトでは true に設定されています。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の E メールアドレスが「.com」で終わらないかどうかを判定します。

```sql
person.emailAddress.doesNotEndWith(".com")
```

## contains

`contains` 関数は、文字列が指定の部分文字列を含んでいるかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.contains({STRING_2}, {BOOLEAN})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列内で検索する文字列です。 |
| `{BOOLEAN}` | チェックで大文字と小文字が区別されるかどうかを指定するオプションのパラメーターです。デフォルトでは true に設定されています。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の E メールアドレスが「2010@gm」という文字列を含んでいるかどうかを判定します。

```sql
person.emailAddress.contains("2010@gm")
```

## doesNotContain

`doesNotContain` 関数は、文字列が指定の部分文字列を含んでいないかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.doesNotContain({STRING_2}, {BOOLEAN})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列内で検索する文字列です。 |
| `{BOOLEAN}` | チェックで大文字と小文字が区別されるかどうかを指定するオプションのパラメーターです。デフォルトでは true に設定されています。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の E メールアドレスが「2010@gm」という文字列を含んでいないかどうかを判定します。

```sql
person.emailAddress.doesNotContain("2010@gm")
```

## equals

`equals` 関数は、文字列が指定の文字列に等しいかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.equals({STRING_2})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列と比較する文字列です。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の名前が「John」かどうかを判定します。

```sql
person.name.equals("John")
```

## notEqualTo

`notEqualTo` 関数は、文字列が指定の文字列に等しくないかどうかを判定するために使用されます。

**形式**

```sql
{STRING_1}.notEqualTo({STRING_2})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{STRING_1}` | チェックの実行対象となる文字列です。 |
| `{STRING_2}` | 最初の文字列と比較する文字列です。 |

**例**

次の PQL クエリでは、大文字と小文字を区別したうえで、人の名前が「John」でないかどうかを判定します。

```sql
person.name.notEqualTo("John")
```

## matches

`matches` 関数は、文字列が特定の正規表現と一致するかどうかを判定するために使用されます。正規表現でのパターンマッチングについて詳しくは、[こちらのドキュメント](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)を参照してください。

**形式**

```sql
{STRING_1}.matches(STRING_2})
```

**例**

次の PQL クエリでは、大文字と小文字を区別せずに、人の名前が「John」で始まるかどうかを判定します。

```sql
person.name.matches("(?i)^John")
```

## 正規表現グループ

`regexGroup` 関数は、指定された正規表現に基づいて特定の情報を抽出するために使用されます。

**形式**

```sql
{STRING}.regexGroup({EXPRESSION})
```

**例**

次の PQL クエリは、E メールアドレスからドメイン名を抽出するために使用します。

```sql
emailAddress.regexGroup("@(\w+)", 1)
```

## 次の手順

ここで学習した文字列関数は、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

