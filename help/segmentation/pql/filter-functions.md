---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フィルター関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 19%

---


# フィルター関数

フィルタ関数は、 [!DNL Profile Query Language] (PQL)のアレイ内のデータをフィルタするために使用します。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## フィルター

( `[]` フィルタ)関数を使用すると、フィルターを配列に適用し、指定した条件に一致する配列のサブセットを返すことができます。

**書式**

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

**書式**

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

これでフィルタ機能の学習が終わり、PQLクエリ内で使用できます。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
