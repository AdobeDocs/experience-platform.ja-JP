---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 配列、リスト、セット関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0

---


# 配列、リスト、セット関数

プロファイルクエリ言語(PQL)オファーは、配列、リスト、文字列とのやり取りを容易にする機能です。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## イン

この関 `in` 数は、項目が配列のメンバーか配列のメンバーかを判断するために使用されます。リスト

**形式**

```sql
{VALUE} in {ARRAY}
```

**例**

次のPQLクエリは、3月、6月または9月に誕生日を持つ人を定義します。

```sql
person.birthMonth in [3, 6, 9]
```

## 次に含まれない

この関 `notIn` 数は、項目が配列または要素のメンバでないかどうかを判断するために使用されます。リスト

>[!NOTE] また、こ `notIn` の関 *数は* 、どちらの値もnullに等しくないことを保証します。 したがって、結果は関数の完全な否定ではありま `in` せん。

**形式**

```sql
{VALUE} notIn {ARRAY}
```

**例**

次のPQLクエリでは、3月、6月、または9月以外の誕生日を持つユーザーを定義します。

```sql
person.birthMonth notIn [3, 6, 9]
```

## 交差

この関数 `intersects` は、2つの配列またはリストに少なくとも1つの共通メンバがあるかどうかを判断するために使用されます。

**形式**

```sql
{ARRAY}.intersects({ARRAY})
```

**例**

次のPQLクエリは、お気に入りの色に赤、青、緑のうち少なくとも1つが含まれる人を定義します。

```sql
person.favoriteColors.intersects(["red", "blue", "green"])
```

## 積集合

この関 `intersection` 数は、2つの配列または配列の共通メンバーを特定するために使用されます。リスト

**形式**

```sql
{ARRAY}.intersection({ARRAY})
```

**例**

次のPQLクエリは、人物1と人物2の両方がお気に入りの赤、青、緑の色を持つかどうかを定義します。

```sql
person1.favoriteColors.intersection(person2.favoriteColors) = ["red", "blue", "green"]
```

## サブセット

この関 `subsetOf` 数は、特定のアレイ（アレイA）が別のアレイ（アレイB）のサブセットであるかどうかを判定するために使用されます。 つまり、配列A内のすべての要素が配列Bの要素であるということです。

**形式**

```sql
{ARRAY}.subsetOf({ARRAY})
```

**例**

次のPQLクエリは、お気に入りの都市をすべて訪問した人を定義します。

```sql
person.favoriteCities.subsetOf(person.visitedCities)
```

## スーパーセット

この関 `supersetOf` 数は、特定の配列（配列A）が別の配列（配列B）のスーパーセットであるかを判定するために使用されます。 つまり、その配列Aには配列Bのすべての要素が含まれます。

**形式**

```sql
{ARRAY}.supersetOf({ARRAY})
```

**例**

次のPQLクエリは、寿司とピザを少なくとも1回は食べたことのある人を定義しています。

```sql
person.eatenFoods.supersetOf(["sushi", "pizza"])
```

## 含む

この関数 `includes` は、配列または項目に特定の項目が含まれているかどうかをリストするために使用されます。

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

この関数 `distinct` は、配列または重複からパラメータ値を削除するために使用されます。リスト

**形式**

```sql
{ARRAY}.distinct()
```

**例**

次のPQLクエリは、複数の店舗で注文した人を指定します。

```sql
person.orders.storeId.distinct().count() > 1
```

## グループ化

この関 `groupBy` 数は、配列やリストの値を、その値に基づいてグループに分割するために使用されます。式

**形式**

```sql
{ARRAY}.groupBy({EXPRESSION)
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | グループ化するリストまたは配列。 |
| `{EXPRESSION}` | 返された式または配列内の各項目をマップするリスト。 |

**例**

次のPQLクエリは、注文が格納されたすべての注文をグループ化します。

```sql
orders.groupBy(storeId)
```

## フィルター

この関数 `filter` は、配列やリストをフィルタリングするために使用します。

**形式**

```sql
{ARRAY}.filter({EXPRESSION})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | フィルタ処理するリストまたは配列。 |
| `{EXPRESSION}` | フィルターに使用する式です。 |

**例**

次のPQLクエリは、21歳以上のすべての人を定義します。

```sql
person.filter(age >= 21)
```

## マップ

この関 `map` 数は、特定の配列内の各項目に式を適用して新しい配列を作成するために使用します。

**形式**

```sql
array.map(expression)
```

**例**

次のPQLクエリは、新しい数値の配列を作成し、元の数値の値を2乗します。

```sql
numbers.map(square)
```

## 配列の `n` 最初

この関 `topN` 数は、渡された数値式 `N` に基づいて昇順で並べ替えられた場合、配列の最初の項目を返すために使用されます。

**形式**

```sql
{ARRAY}.topN({VALUE}, {AMOUNT})
```

| 引数 | 説明 |
| --------- | ----------- |
| `{ARRAY}` | 並べ替えるリストまたは配列。 |
| `{VALUE}` | 配列または配列を並べ替えるプロパティリスト。 |
| `{AMOUNT}` | 返す項目数。 |

**例**

次のPQLクエリは、最も高い価格で上位5件の注文を返します。

```sql
orders.topN(price, 5)
```

## 配列の `n` 最後

この関 `bottomN` 数は、渡された数値式 `N` に基づいて昇順で並べ替えられた場合、配列の最後の項目を返すために使用されます。

**形式**

```sql
{ARRAY}.bottomN({VALUE}, {AMOUNT})
```

| 引数 | 説明 |
| --------- | ----------- | 
| `{ARRAY}` | 並べ替えるリストまたは配列。 |
| `{VALUE}` | 配列または配列を並べ替えるプロパティリスト。 |
| `{AMOUNT}` | 返す項目数。 |

**例**

次のPQLクエリは、最も低い価格で上位5件の注文を返します。

```sql
orders.bottomN(price, 5)
```

## 最初の項目

この関 `head` 数は、配列または配列内の最初の項目を返すために使用されます。リスト

**形式**

```sql
{ARRAY}.head()
```

**例**

次のPQLクエリは、最も高い価格の上位5件の注文のうち最初の注文を返します。 関数の詳細につい `topN` ては、配列の最初の節を [参照 `n` してください](#first-n-in-array) 。

```sql
orders.topN(price, 5).head()
```

## 次の手順

これで、配列、リスト、設定機能の学習が終わり、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
