---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；論理量指定子；論理量指定子；
solution: Experience Platform
title: PQL 論理量指定子
description: 論理量指定子を使用すると、プロファイルクエリ言語（PQL）で配列にチェック条件を付けることができます。
exl-id: 8b1c9560-02e2-46e0-9646-c64dd4a15df1
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 79%

---

# 論理量指定子関数

論理量指定子を使用して、 [!DNL Profile Query Language] (PQL) を参照してください。 その他の PQL 関数について詳しくは、 [[!DNL Profile Query Language] 概要](./overview.md).

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
