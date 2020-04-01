---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 日付と時間関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc

---


# 日付と時間関数

日付と時間の関数は、プロファイルクエリ言語(PQL)内の値に対して日付と時間の操作を実行するために使用されます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## 現在の月

この関 `currentMonth` 数は、現在の月を整数で返します。

**形式**

```sql
currentMonth()
```

**例**

次のPQLクエリは、個人の生誕月が現在の月かどうかを確認します。

```sql
person.birthMonth = currentMonth()
```

## 月の取得

渡さ `getMonth` れたタイムスタンプに基づいて月を整数で返します。

**形式**

```sql
{TIMESTAMP}.getMonth()
```

**例**

次のPQLクエリは、その人の生誕月が6月かどうかを確認します。

```sql
person.birthdate.getMonth() = 6
```

## 現在の年

この関 `currentYear` 数は、現在の年を整数で返します。

**形式**

```sql
currentYear()
```

**例**

次のPQLクエリは、製品が今年中に販売されたかどうかを確認します。

```sql
product.saleYear = currentYear()
```

## 年を取得

渡さ `getYear` れたタイムスタンプに基づいて、年を整数で返します。

**形式**

```sql
{TIMESTAMP}.getYear()
```

**例**

次のPQLクエリは、1991年、1992年、1993年、1994年、または1995年に生誕年に該当するかどうかを確認します。

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

次のPQLクエリは、その人の生誕日がその月の現在の日と一致するかどうかを確認します。

```sql
person.birthDay = currentDayOfMonth()
```

## 月の日を取得

指定し `getDayOfMonth` たタイムスタンプに基づいて、日を整数で返します。

**形式**

```sql
{TIMESTAMP}.getDayOfMonth()
```

**例**

次のPQLクエリは、その品目が月の最初の15日以内に販売されたかどうかを確認します。

```sql
product.sale.getDayOfMonth() <= 15
```

## 発生

この関 `occurs` 数は、指定されたtimestamp関数と一定期間を比較します。

**形式**

この関 `occurs` 数は、次のいずれかの形式を使用して記述できます。

```sql
{TIMESTAMP} occurs {COMPARISON} {INTEGER} {TIME_UNIT} {DIRECTION} {TIME}
{TIMESTAMP} occurs {DIRECTION} {TIME}
{TIMESTAMP} occurs (on) {TIME}
{TIMESTAMP} occurs between {TIME} and {TIME}
```

| 引数 | 説明 |
| --------- | ----------- |
| `{COMPARISON}` | 比較演算子。 次のいずれかの演算子を使用できます。 `>`、 `>=`、、 `<`、 `<=`、 `=`、 `!=`、 比較関数の詳細については、「比較関数」ドキュメントを参照し [てください](./comparison-functions.md)。 |
| `{INTEGER}` | 負でない整数です。 |
| `{TIME_UNIT}` | 時間の単位。 次のいずれかの単語を指定できます。 `millisecond(s)`そう、そう、そう、そう、そ `second(s)`う、そう、そう、 `minute(s)``hour(s)``day(s)``week(s)``month(s)``year(s)``decade(s)``century``centuries``millennium``millennia`そう、そう、そう、そう、こう、続けて来た。 |
| `{DIRECTION}` | 日付を比較するタイミングを説明する前置詞。 次のいずれかの単語を指定できます。 `before``after`、 `from` |
| `{TIME}` | タイムスタンプリテラル(`today`、、、 `now`、 `yesterday`、 `tomorrow`)、相対時間単位(いずれか、 `this`または `last``next` 後に時間単位が続く)、タイムスタンプ属性を指定できます。 |

>[!NOTE] この単語の使用はオプ `on` ションです。 など、一部の組み合わせの読みやすさを向上させるために用意されていま `timestamp occurs on date(2019,12,31)`す。

**例**

次のPQLクエリは、品目が先週販売されたかどうかを確認します。

```sql
product.saleDate occurs last week
```

次のPQLクエリは、品目が2015年1月8日から2017年7月1日の間に販売されたかどうかを確認します。

```sql
product.saleDate occurs between date(2015, 1, 8) and date(2017, 7, 1)
```

## 今すぐ

`now` は、PQL実行のタイムスタンプを表す予約語です。

**例**

次のPQLクエリは、商品が3時間前に販売されたかどうかを確認します。

```sql
product.saleDate occurs = 3 hours before now
```

## 今日

`today` は、PQL実行日の開始を表す予約語です。

**例**

次のPQLクエリは、人の誕生日が3日前かどうかを確認します。

```sql
person.birthday occurs = 3 days before today
```

## 次の手順

これで、日付と時間の関数について学習できたので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
