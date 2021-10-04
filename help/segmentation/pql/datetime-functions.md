---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；日付と時間関数；日時関数；日時；日付；時間；
solution: Experience Platform
title: PQL 日付と時間関数
topic-legacy: developer guide
description: 日付および時間関数は、プロファイルクエリ言語（PQL）内の値に対して日付と時間の操作を実行するために使用されます。
exl-id: 8cbffcb6-1c25-454f-8f02-eca602318e5e
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 89%

---

# 日付および時間関数

日付および時間関数 は、[!DNL Profile Query Language](PQL) 内の値に対して日付と時刻の操作を実行するために使用されます。 その他の PQL 関数の詳細については、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

## Current month

この`currentMonth`関数は、現在の月を整数で返します。

**形式**

```sql
currentMonth()
```

**例**

次の PQL クエリは、その人の生まれた月が現在の月かどうかを確認します。

```sql
person.birthMonth = currentMonth()
```

## Get month

この`getMonth`関数は、指定されたタイムスタンプに基づいて月を整数で返します。

**形式**

```sql
{TIMESTAMP}.getMonth()
```

**例**

次の PQL クエリは、その人の生まれた月が 6 月かどうかを確認します。

```sql
person.birthdate.getMonth() = 6
```

## Current year

この`currentYear`関数は、現在の年を整数で返します。

**形式**

```sql
currentYear()
```

**例**

次の PQL クエリは、製品が現在の年に販売されたかどうかを確認します。

```sql
product.saleYear = currentYear()
```

## Get year

この`getYear`関数は、指定されたタイムスタンプに基づいて、年を整数で返します。

**形式**

```sql
{TIMESTAMP}.getYear()
```

**例**

次の PQL クエリは、その人の生まれた年が 1991、1992 、1993 、1994 、または 1995 年に該当するかどうかを確認します。

```sql
person.birthday.getYear() in [1991, 1992, 1993, 1994, 1995]
```

## Current day of month

この`currentDayOfMonth`関数は、現在の月の日を整数で返します。

**形式**

```sql
currentDayOfMonth()
```

**例**

次の PQL クエリは、その人の生まれた日がの現在の月の日と一致するかどうかを確認します。

```sql
person.birthDay = currentDayOfMonth()
```

## Get day of month

この`getDayOfMonth`関数は、指定されたタイムスタンプに基づいて、日を整数で返します。

**形式**

```sql
{TIMESTAMP}.getDayOfMonth()
```

**例**

次の PQL クエリは、その品目が月の最初の 15 日以内に販売されたかどうかを確認します。

```sql
product.sale.getDayOfMonth() <= 15
```

## Occurs

この`occurs`関数は、指定されたタイムスタンプ関数と一定期間を比較します。

**形式**

この`occurs`関数は、次のいずれかの形式を使用して記述できます。

```sql
{TIMESTAMP} occurs {COMPARISON} {INTEGER} {TIME_UNIT} {DIRECTION} {TIME}
{TIMESTAMP} occurs {DIRECTION} {TIME}
{TIMESTAMP} occurs (on) {TIME}
{TIMESTAMP} occurs between {TIME} and {TIME}
```

| 引数 | 説明 |
| --------- | ----------- |
| `{COMPARISON}` | 比較演算子。次のいずれかの演算子を使用できます。`>`、`>=`、`<`、`<=`、`=`、 `!=`。比較関数の詳細については、「[比較関数ドキュメント](./comparison-functions.md)」を参照し てください。 |
| `{INTEGER}` | 負でない整数。 |
| `{TIME_UNIT}` | 時間の単位。次のいずれかの単語を指定できます。`millisecond(s)`、`second(s)`、`minute(s)`、`hour(s)`、`day(s)`、`week(s)`、`month(s)`、`year(s)`、`decade(s)`、`century`、`centuries`、`millennium`、`millennia`。 |
| `{DIRECTION}` | 日付を比較するタイミングを説明する前置詞。次のいずれかの単語を指定できます。`before`、`after`、`from`。 |
| `{TIME}` | タイムスタンプリテラル（`today`、`now`、`yesterday`、`tomorrow`）、相対時間単位（`this`、`last`または`next`ののいずれかに時間単位が続く）、またはタイムスタンプ属性を指定できます。 |

>[!NOTE]
>
>この単語の使用`on`はオプションです。`timestamp occurs on date(2019,12,31)`など、一部の組み合わせの読みやすさを向上させるために用意されています。

**例**

次の PQL クエリは、品目が先週販売されたかどうかを確認します。

```sql
product.saleDate occurs last week
```

次の PQL クエリは、品目が 2015 年 1 月 8 日から 2017 年 7 月 1 日の間に販売されたかどうかを確認します。

```sql
product.saleDate occurs between date(2015, 1, 8) and date(2017, 7, 1)
```

## Now

`now`は、PQL 実行のタイムスタンプを表す予約語です。

**例**

次の PQL クエリは、製品が 3 時間前に販売されたかどうかを確認します。

```sql
product.saleDate occurs = 3 hours before now
```

## Today

`today`は、PQL 実行日の開始を表す予約語です。

**例**

次の PQL クエリは、ある人の生まれた日が 3 日前かどうかを確認します。

```sql
person.birthday occurs = 3 days before today
```

## 次の手順

日付および時間関数について学習したので、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。
