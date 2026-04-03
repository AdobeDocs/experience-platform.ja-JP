---
solution: Experience Platform
title: PQL関数の配列、リストおよびセット
description: Profile Query Language（PQL）には、配列、リスト、文字列の操作を簡単にする機能が用意されています。
exl-id: 5ff2b066-8857-4cde-9932-c8bf09e273d3
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '820'
ht-degree: 57%

---

# 配列、リスト、およびセットの関数

[!DNL Profile Query Language] （PQL）には、配列、リスト、文字列の操作を簡単にする関数が用意されています。 その他のPQL関数について詳しくは、[[!DNL Profile Query Language] 概要](./overview.md)を参照してください。

## 次に含まれる

`in`関数は、項目が配列またはリストのメンバーであるかどうかをブール値として判断するために使用されます。

**形式**

```sql
{VALUE} in {ARRAY}
```

**例**

次の PQL クエリは、誕生日が 3 月、6 月または 9 月の人を定義します。

```sql
person.birthMonth in [3, 6, 9]
```

## 次に含まれない

`notIn`関数は、項目が配列またはリストのメンバーでないかどうかをブール値として判断するために使用されます。

>[!NOTE]
>
>この`notIn` 関数は&#x200B;*また*、どの値も NULL ではないことを保証します。したがって、結果は `in` 関数の完全な否定ではありません。

**形式**

```sql
{VALUE} notIn {ARRAY}
```

**例**

次の PQL クエリは、誕生日が 3 月、6 月または 9 月でない人を定義します。

```sql
person.birthMonth notIn [3, 6, 9]
```

## 交わり

`intersects`関数は、2つの配列またはリストに少なくとも1つの共通メンバーがブール値として含まれているかどうかを判断するために使用されます。

**形式**

```sql
{ARRAY}.intersects({ARRAY})
```

**例**

次の PQL クエリは、お気に入りの色に赤、青、緑の1 つ以上が含まれる人を定義します。

```sql
person.favoriteColors.intersects(["red", "blue", "green"])
```

## 積集合

`intersection`関数は、2つの配列またはリストの共通メンバーをリストとして決定するために使用されます。

**形式**

```sql
{ARRAY}.intersection({ARRAY})
```

**例**

次の PQL クエリは、人物 1 と人物 2 の両方が、お気に入りの色を赤、青、および緑としているかどうかを定義します。

```sql
person1.favoriteColors.intersection(person2.favoriteColors) = ["red", "blue", "green"]
```

## サブセット

`subsetOf` 関数は、特定の配列（配列 A）が別の配列（配列 B）のサブセットであるかを判断するために使用されます。言い換えれば、配列A内のすべての要素が配列Bの要素であることをブール値として表します。

**形式**

```sql
{ARRAY}.subsetOf({ARRAY})
```

**例**

次の PQL クエリは、お気に入りの都市をすべて訪問した人を定義します。

```sql
person.favoriteCities.subsetOf(person.visitedCities)
```

## スーパーセット

`supersetOf` 関数は、特定の配列（配列 A）が別の配列（配列 B）のスーパーセットであるかを判断するために使用されます。つまり、この配列Aには、配列Bのすべての要素がブール値として含まれています。

**形式**

```sql
{ARRAY}.supersetOf({ARRAY})
```

**例**

次の PQL クエリは、寿司とピザを 1 回以上食べたことがある人を定義しています。

```sql
person.eatenFoods.supersetOf(["sushi", "pizza"])
```

## 次を含む

`includes`関数は、配列またはリストに特定の項目がブール値として含まれているかどうかを判断するために使用されます。

**形式**

```sql
{ARRAY}.includes({ITEM})
```

**例**

次の PQL クエリは、お気に入りの色に赤が含まれる人物を定義します。

```sql
person.favoriteColors.includes("red")
```

## 個別

`distinct`関数は、配列またはリストから配列として重複する値を削除するために使用されます。

**形式**

```sql
{ARRAY}.distinct()
```

**例**

次の PQL クエリは、複数の店舗で注文した人物を指定します。

```sql
person.orders.storeId.distinct().count() > 1
```

## Group by

`groupBy`関数は、グループ化式の一意の値から配列式の値のパーティションである配列へのマップとして、式の値に基づいて配列またはリストの値をグループに分割するために使用されます。

**形式**

```sql
{ARRAY}.groupBy({EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | グループ化するリストまたは配列。 |
| `{EXPRESSION}` | 返されたリストまたは配列内の各項目をマッピングする式。 |

**例**

次の PQL クエリは、注文がおこなわれた店舗別にすべての注文をグループ化します。

```sql
xEvent[type="order"].groupBy(storeId)
```

## Filter

`filter`関数は、入力に応じて、式を配列またはリストとしてフィルタリングするために使用されます。

**形式**

```sql
{ARRAY}.filter({EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | フィルタリングする配列またはリスト。 |
| `{EXPRESSION}` | フィルタリングに使用する式です。 |

**例**

次の PQL クエリは、21 歳以上のすべての人を定義します。

```sql
person.filter(age >= 21)
```

## Map

`map`関数は、配列として指定された配列内の各項目に式を適用して、新しい配列を作成するために使用されます。

**形式**

```sql
array.map(expression)
```

**例**

次の PQL クエリは、数値の新しい配列を作成し、元の数値の値を 2 乗します。

```sql
numbers.map(square)
```

## First `n` in array {#first-n}

`topN`関数は、指定された数式に基づいて昇順に並べ替えられた配列の最初の`N`項目を配列として返すために使用されます。

**形式**

```sql
{ARRAY}.topN({VALUE}, {AMOUNT})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | 並べ替えるリストまたは配列。 |
| `{VALUE}` | 配列またはリストを並べ替えるプロパティ。 |
| `{AMOUNT}` | 返される項目の数。 |

**例**

次の PQL クエリは、最も金額が高い注文上位 5 件を返します。

```sql
orders.topN(price, 5)
```

## Last `n` in array

`bottomN`関数は、指定された数式に基づいて昇順に並べ替えられた配列の最後の`N`項目を配列として返すために使用されます。

**形式**

```sql
{ARRAY}.bottomN({VALUE}, {AMOUNT})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | 並べ替えるリストまたは配列。 |
| `{VALUE}` | 配列またはリストを並べ替えるプロパティ。 |
| `{AMOUNT}` | 返される項目の数。 |

**例**

次の PQL クエリは、最も金額が低い注文上位 5 件を返します。

```sql
orders.bottomN(price, 5)
```

## 最初の項目

`head`関数は、配列またはリストの最初の項目をオブジェクトとして返すために使用されます。

**形式**

```sql
{ARRAY}.head()
```

**例**

次の PQL クエリは、最も金額が高い注文上位 5 件の最初の項目を返します。`topN` 関数の詳細については、[first `n` in array](#first-n) の節を参照してください 。

```sql
orders.topN(price, 5).head()
```

## 次の手順

これで、配列、リスト、およびセット関数について学習し、PQL クエリ内で使用できるようになりました。その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
