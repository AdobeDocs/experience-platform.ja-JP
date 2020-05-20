---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 配列、リスト、セット関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0
workflow-type: tm+mt
source-wordcount: '740'
ht-degree: 5%

---


# 配列、リスト、セット関数

プロファイルクエリ言語(PQL)オファーは、配列、リスト、文字列とのやり取りを容易にするための機能です。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## イン

この `in` 関数は、項目が配列のメンバかリストのメンバかを判断するために使用されます。

**形式**

```sql
{VALUE} in {ARRAY}
```

**例**

次のPQLクエリは、3月、6月または9月に誕生日を持つユーザーを定義します。

```sql
person.birthMonth in [3, 6, 9]
```

## 次に含まれない

この `notIn` 関数は、項目が配列またはリストのメンバでないかどうかを判断するために使用されます。

>[!NOTE] また、 `notIn` この関数 *は* 、どちらの値もnullに等しくないことを確認します。 したがって、結果は `in` 関数の否定ではありません。

**形式**

```sql
{VALUE} notIn {ARRAY}
```

**例**

次のPQLクエリは、3月、6月、または9月以外の誕生日を持つユーザーを定義します。

```sql
person.birthMonth notIn [3, 6, 9]
```

## 交差

この `intersects` 関数は、2つの配列またはリストに少なくとも1つの共通メンバが存在するかどうかを判断するために使用します。

**形式**

```sql
{ARRAY}.intersects({ARRAY})
```

**例**

次のPQLクエリは、お気に入りの色に赤、青、緑のいずれか1つ以上が含まれる人を定義します。

```sql
person.favoriteColors.intersects(["red", "blue", "green"])
```

## 積集合

この `intersection` 関数は、2つの配列またはリストの共通メンバーを特定するために使用します。

**形式**

```sql
{ARRAY}.intersection({ARRAY})
```

**例**

次のPQLクエリは、人物1と人物2の両方がお気に入りの色を赤、青、緑にするかどうかを定義します。

```sql
person1.favoriteColors.intersection(person2.favoriteColors) = ["red", "blue", "green"]
```

## サブセット

この `subsetOf` 関数は、特定の配列（配列A）が別の配列（配列B）のサブセットであるかどうかを判定するために使用されます。 つまり、配列Aの要素はすべて配列Bの要素です。

**形式**

```sql
{ARRAY}.subsetOf({ARRAY})
```

**例**

次のPQLクエリは、お気に入りの都市をすべて訪問した訪問者を定義します。

```sql
person.favoriteCities.subsetOf(person.visitedCities)
```

## スーパーセット

この `supersetOf` 関数は、特定の配列（配列A）が別の配列（配列B）のスーパーセットであるかを判断するために使用されます。 つまり、配列Aには配列Bの要素がすべて含まれます。

**形式**

```sql
{ARRAY}.supersetOf({ARRAY})
```

**例**

次のPQLクエリは、寿司やピザを少なくとも1回は食べたことのある人を定義しています。

```sql
person.eatenFoods.supersetOf(["sushi", "pizza"])
```

## 含む

この `includes` 関数は、配列またはリストに特定の項目が含まれているかどうかを調べるために使用されます。

**形式**

```sql
{ARRAY}.includes({ITEM})
```

**例**

次のPQLクエリは、お気に入りの色に赤が含まれる人を定義します。

```sql
person.favoriteColors.includes("red")
```

## 個別

この `distinct` 関数は、配列またはリストから重複値を削除するために使用します。

**形式**

```sql
{ARRAY}.distinct()
```

**例**

次のPQLクエリは、複数のストアで注文を行ったユーザーを指定します。

```sql
person.orders.storeId.distinct().count() > 1
```

## グループ化の基準

この `groupBy` 関数は、配列またはリストの値を式の値に基づいてグループに分割するために使用します。

**形式**

```sql
{ARRAY}.groupBy({EXPRESSION)
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | グループ化する配列またはリスト。 |
| `{EXPRESSION}` | 返される配列またはリスト内の各アイテムをマップする式。 |

**例**

次のPQLクエリは、注文が配置されたストアの注文をすべてグループ化します。

```sql
orders.groupBy(storeId)
```

## フィルター

この `filter` 関数は、式に基づいて配列またはリストをフィルタリングするために使用します。

**形式**

```sql
{ARRAY}.filter({EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | フィルタする配列またはリスト。 |
| `{EXPRESSION}` | フィルタに使用する式です。 |

**例**

次のPQLクエリは、21才以上のすべての人を定義します。

```sql
person.filter(age >= 21)
```

## マップ

この `map` 関数は、特定の配列内の各項目に式を適用して新しい配列を作成するために使用します。

**形式**

```sql
array.map(expression)
```

**例**

次のPQLクエリは、新しい数値の配列を作成し、元の数値の値を2乗します。

```sql
numbers.map(square)
```

## 配列 `n` の最初

この `topN` 関数は、渡された数値式に基づいて昇順で並べ替えられた場合に、配列の最初の `N` 項目を返すために使用されます。

**形式**

```sql
{ARRAY}.topN({VALUE}, {AMOUNT})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | 並べ替える配列またはリスト。 |
| `{VALUE}` | 配列またはリストを並べ替えるプロパティ。 |
| `{AMOUNT}` | 返すアイテムの数。 |

**例**

次のPQLクエリは、最も高い価格で上位5件の注文を返します。

```sql
orders.topN(price, 5)
```

## 配列 `n` の最後

この `bottomN` 関数は、渡された数値式に基づいて昇順で並べ替えられた場合に、配列の最後の `N` 項目を返すために使用されます。

**形式**

```sql
{ARRAY}.bottomN({VALUE}, {AMOUNT})
```

| 引数 | 説明 |
| --------- | ----------- | 
| `{ARRAY}` | 並べ替える配列またはリスト。 |
| `{VALUE}` | 配列またはリストを並べ替えるプロパティ。 |
| `{AMOUNT}` | 返すアイテムの数。 |

**例**

次のPQLクエリは、最も安い価格で上位5件の注文を返します。

```sql
orders.bottomN(price, 5)
```

## 最初の項目

この `head` 関数は、配列またはリスト内の最初の項目を返すために使用されます。

**形式**

```sql
{ARRAY}.head()
```

**例**

次のPQLクエリは、上位5件の注文のうち、最も高い価格の最初の注文を返します。 この `topN` 関数の詳細については、配列の [最初 `n` の節を参照してください](#first-n-in-array) 。

```sql
orders.topN(price, 5).head()
```

## 次の手順

これで、配列、リスト、および設定関数について学習できたので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
