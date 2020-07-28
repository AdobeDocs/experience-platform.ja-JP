---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 論理量指定子
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 93%

---


# 論理量指定子関数

Logical quantifiers can be used to assert conditions with arrays in [!DNL Profile Query Language] (PQL). その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## exists

`exists` 関数は、指定された条件が満たされれば、配列内に項目が存在していると判定します。

**形式**

```sql
exists {VARIABLE} from {EXPRESSION} where {CONDITION}
exists {VARIABLE} from {EXPRESSION} : {CONDITION}
```

| 引数 | 説明 |
| ---------- | ----------- |
| `{VARIABLE}` | 変数の名前です。 |
| `{EXPRESSION}` | チェック対象となる配列です。 |
| `{CONDITION}` | 返される配列の値をフィルタリングする式（省略可能）です。 |

**例**

次の PQL クエリでは、価格が 50 ドルを超える、または SKU が「PS」であるすべてのイベントを取得します。

```sql
exists E from xEvent where (E.commerce.item.price > 50), I from E.productListItems where I.SKU = "PS"
```

## forall

`forall` 関数は、指定されたすべての条件を満たす配列内の項目をすべて判定します。

**形式**

```sql
forall {VARIABLE} from {EXPRESSION} where {CONDITION}
forall {VARIABLE} from {EXPRESSION} : {CONDITION}
```

| 引数 | 説明 |
| ---------- | ----------- |
| `{VARIABLE}` | 変数の名前です。 |
| `{EXPRESSION}` | チェック対象となる配列です。 |
| `{CONDITION}` | 返される配列の値をフィルタリングする式（省略可能）です。 |

**例**

次の PQL クエリでは、価格が 50 ドルを超え、かつ SKU が「PS」であるすべてのイベントを取得します。

```sql
forall E from xEvent where (E.commerce.item.price > 50), I from E.productListItems where I.SKU = "PS"
```

## 次の手順

ここで学習した論理量指定子は、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
