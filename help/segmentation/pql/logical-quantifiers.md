---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 論理量指定子
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0

---


# 論理量詞関数

論理量指定子を使用して、プロファイルクエリ言語(PQL)の配列と条件をアサートできます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## 存在する

この関 `exists` 数は、指定された条件を満たす配列内のアイテムの存在を判断します。

**形式**

```sql
exists {VARIABLE} from {EXPRESSION} where {CONDITION}
exists {VARIABLE} from {EXPRESSION} : {CONDITION}
```

| 引数 | 説明 |
| ---------- | ----------- |
| `{VARIABLE}` | 変数の名前。 |
| `{EXPRESSION}` | チェックするアレイ。 |
| `{CONDITION}` | 返される配列の値をフィルターするオプションの式。 |

**例**

次のPQLクエリでは、価格が$50を超えるイベント、または「PS」のSKUを持つすべての製品を取得します。

```sql
exists E from xEvent where (E.commerce.item.price > 50), I from E.productListItems where I.SKU = "PS"
```

## すべて

この関 `forall` 数は、渡された条件をすべて満たす配列内のすべての項目を判定します。

**形式**

```sql
forall {VARIABLE} from {EXPRESSION} where {CONDITION}
forall {VARIABLE} from {EXPRESSION} : {CONDITION}
```

| 引数 | 説明 |
| ---------- | ----------- |
| `{VARIABLE}` | 変数の名前。 |
| `{EXPRESSION}` | チェックするアレイ。 |
| `{CONDITION}` | 返される配列の値をフィルターするオプションの式。 |

**例**

次のPQLクエリは、価格が$50を超え、SKUが「PS」のイベントをすべて取得します。

```sql
forall E from xEvent where (E.commerce.item.price > 50), I from E.productListItems where I.SKU = "PS"
```

## 次の手順

これで論理量指定子について学習できたので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
