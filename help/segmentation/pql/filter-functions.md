---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フィルター関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# フィルター関数

フィルタ関数は、プロファイルクエリ言語(PQL)でアレイ内のデータをフィルタするために使用します。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## フィルター

( `[]` フィルタ)関数を使用すると、フィルターを配列に適用し、指定した条件に一致する配列のサブセットを返すことができます。

**形式**

```sql
{ARRAY}[filter]
```

**例**

次のPQLクエリでは、「PS」に等しいSKUを持つ商品が1つ以上あるすべてのイベントを取得します。

```sql
xEvent[productListItems[SKU="PS"]]
```

## Up演算子

( `^` up)演算子を使用すると、上位レベルのフィルターのプロパティを参照できます。

**形式**

```sql
{ARRAY}[{FILTER_1}[{FILTER_2} or ^{PROPERTY}]]
```

| 引数 | 説明 |
| -------- | ----------- |
| `{ARRAY}` | フィルタ処理を行う配列。 |
| `{FILTER_1}` | フィルタリングの外側の層。 |
| `{FILTER_2}` | フィルタリングの内層 |
| `^{PROPERTY}` | フィルターを適用しているプロパティです。 このため、filter1に基づいてプロパティをチェックしてい `^`ます。 |

**例**

次のPQLクエリでは、SKUが「PS」に等しい商品品目が1つ以上あるイベント、 **または性別が女性である人がいるすべての製品を取得します** 。

```sql
xEvent[productListItems[SKU="PS" or ^^.person.gender="female"]]
```

## 次の手順

これでフィルタ機能の学習が終わり、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
