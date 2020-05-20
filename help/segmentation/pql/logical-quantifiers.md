---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 論理量指定子
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 5%

---


# 論理的な数量化関数

論理量指定子は、プロファイルクエリ言語(PQL)の配列と条件をアサートするために使用できます。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## 存在する

この `exists` 関数は、指定された条件を満たす配列内の項目の存在を判断します。

**形式**

```sql
exists {VARIABLE} from {EXPRESSION} where {CONDITION}
exists {VARIABLE} from {EXPRESSION} : {CONDITION}
```

| 引数 | 説明 |
| ---------- | ----------- |
| `{VARIABLE}` | 変数の名前。 |
| `{EXPRESSION}` | チェック対象のアレイ。 |
| `{CONDITION}` | 返される配列の値をフィルターするオプションの式。 |

**例**

次のPQLクエリでは、価格が$50を超えるイベント、またはSKUが「PS」であるすべての製品を取得します。

```sql
exists E from xEvent where (E.commerce.item.price > 50), I from E.productListItems where I.SKU = "PS"
```

## すべて

この `forall` 関数は、配列内のすべての項目を特定の条件を満たすものから判断します。

**形式**

```sql
forall {VARIABLE} from {EXPRESSION} where {CONDITION}
forall {VARIABLE} from {EXPRESSION} : {CONDITION}
```

| 引数 | 説明 |
| ---------- | ----------- |
| `{VARIABLE}` | 変数の名前。 |
| `{EXPRESSION}` | チェック対象のアレイ。 |
| `{CONDITION}` | 返される配列の値をフィルターするオプションの式。 |

**例**

次のPQLクエリでは、価格が$50を超え、SKUが「PS」のイベントをすべて取得します。

```sql
forall E from xEvent where (E.commerce.item.price > 50), I from E.productListItems where I.SKU = "PS"
```

## 次の手順

論理量指定子について学んだので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
