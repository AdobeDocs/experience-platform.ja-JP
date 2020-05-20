---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 日付と時間関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 4%

---


# 日付と時間関数

日付と時間の関数は、プロファイルクエリ言語(PQL)内の値に対して日付と時間の演算を実行するために使用します。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## 現在の月

この `currentMonth` 関数は、現在の月を整数で返します。

**形式**

```sql
currentMonth()
```

**例**

次のPQLクエリは、その人の生まれた月が現在の月かどうかを確認します。

```sql
person.birthMonth = currentMonth()
```

## 月を取得

この `getMonth` 関数は、渡されたタイムスタンプに基づいて月を整数で返します。

**形式**

```sql
{TIMESTAMP}.getMonth()
```

**例**

次のPQLクエリは、その人の生まれた月が6月かどうかを確認します。

```sql
person.birthdate.getMonth() = 6
```

## 現在の年

この `currentYear` 関数は、現在の年を整数で返します。

**形式**

```sql
currentYear()
```

**例**

次のPQLクエリは、その製品が今年中に販売されたかどうかを確認します。

```sql
product.saleYear = currentYear()
```

## 年の取得

この `getYear` 関数は、渡されたタイムスタンプに基づいて、年を整数で返します。

**形式**

```sql
{TIMESTAMP}.getYear()
```

**例**

次のPQLクエリは、1991年、1992年、1993年、1994年、または1995年に生誕年が該当するかどうかを確認します。

```sql
person.birthday.getYear() in [1991, 1992, 1993, 1994, 1995]
```

## 現在の日付

この `currentDayOfMonth` 関数は、月の現在の日を整数で返します。

**形式**

```sql
currentDayOfMonth()
```

**例**

次のPQLクエリは、その人の生まれた日がその月の現在の日と一致するかどうかを確認します。

```sql
person.birthDay = currentDayOfMonth()
```

## 日付の取得

この `getDayOfMonth` 関数は、渡されたタイムスタンプに基づいて日を整数で返します。

**形式**

```sql
{TIMESTAMP}.getDayOfMonth()
```

**例**

次のPQLクエリは、その月の最初の15日以内に製品が販売されたかどうかを確認します。

```sql
product.sale.getDayOfMonth() <= 15
```

## 発生

この `occurs` 関数は、指定したタイムスタンプ関数と一定期間を比較します。

**形式**

この `occurs` 関数は、次のいずれかの形式を使用して記述できます。

```sql
{TIMESTAMP} occurs {COMPARISON} {INTEGER} {TIME_UNIT} {DIRECTION} {TIME}
{TIMESTAMP} occurs {DIRECTION} {TIME}
{TIMESTAMP} occurs (on) {TIME}
{TIMESTAMP} occurs between {TIME} and {TIME}
```

| 引数 | 説明 |
| --------- | ----------- |
| `{COMPARISON}` | 比較演算子。 次の演算子のいずれかを使用できます。 `>`、 `>=`、 `<`、 `<=`、 `=`、 `!=`、 比較関数の詳細については、「 [比較関数ドキュメント](./comparison-functions.md)」を参照してください。 |
| `{INTEGER}` | 負でない整数。 |
| `{TIME_UNIT}` | 時間の単位。 次のいずれかの単語を使用できます。 `millisecond(s)`, `second(s)`, `minute(s)`, `hour(s)`, `day(s)`, `week(s)`, `month(s)`, `year(s)`, `decade(s)`, , `century``centuries``millennium``millennia`, , |
| `{DIRECTION}` | 日付をいつ比較するかを説明する前置詞。 次のいずれかの単語を使用できます。 `before`、 `after`、 `from`. |
| `{TIME}` | タイムスタンプリテラル(`today`、 `now`、 `yesterday`、 `tomorrow`)、相対時間単位(時間単位の1つ、 `this`または `last``next` その後に時間単位が続く)、またはタイムスタンプ属性を使用できます。 |

>[!NOTE] この単語の使用は任意 `on` です。 例えば、一部の組み合わせで読みやすさが向上し `timestamp occurs on date(2019,12,31)`ます。

**例**

次のPQLクエリは、商品が先週販売されたかどうかを確認します。

```sql
product.saleDate occurs last week
```

次のPQLクエリは、2015年1月8日～ 2017年7月1日の間に品目が販売されたかどうかを確認します。

```sql
product.saleDate occurs between date(2015, 1, 8) and date(2017, 7, 1)
```

## 今すぐ

`now` は、PQL実行のタイムスタンプを表す予約語です。

**例**

次のPQLクエリは、商品が正確に3時間前に販売されたかどうかを確認します。

```sql
product.saleDate occurs = 3 hours before now
```

## 今日

`today` は、PQL実行日の開始のタイムスタンプを表す予約語です。

**例**

次のPQLクエリは、人の誕生日が3日前かどうかをチェックします。

```sql
person.birthday occurs = 3 days before today
```

## 次の手順

これで日付と時間の関数について学習できたので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
