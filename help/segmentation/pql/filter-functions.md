---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;Segmentation Service;pql;PQL;Profile Query Language;filter functions;filter;
solution: Experience Platform
title: フィルター関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 87%

---


# フィルター関数

Filter functions are used to filter data within arrays in [!DNL Profile Query Language] (PQL). More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

## フィルター

`[]`（フィルター）関数を使用すると、フィルターを配列に適用し、指定した条件に一致する配列のサブセットを返すことができます。

**形式**

```sql
{ARRAY}[filter]
```

**例**

次の PQL クエリは、「PS」と等しい SKU を持つ 1 つ以上の製品を持つすべてのイベントを取得します。

```sql
xEvent[productListItems[SKU="PS"]]
```

## Up 演算子

`^`（up）演算子を使用すると、フィルターの上位レベルのプロパティを参照できます。

**形式**

```sql
{ARRAY}[{FILTER_1}[{FILTER_2} or ^{PROPERTY}]]
```

| 引数 | 説明 |
| -------- | ----------- |
| `{ARRAY}` | フィルターを適用する配列。 |
| `{FILTER_1}` | フィルタリングの外層。 |
| `{FILTER_2}` | フィルタリングの内層。 |
| `^{PROPERTY}` | フィルターを適用するプロパティ。`^` のため、filter1 に基づいてプロパティをチェックしています。 |

**例**

次の PQL クエリでは、「PS」に等しい SKU を持つ製品品目が 1 つ以上あるイベント、**または**&#x200B;性別が女性である個人を持つすべてのイベントを取得します。

```sql
xEvent[productListItems[SKU="PS" or ^^.person.gender="female"]]
```

## 次の手順

ここで学習したフィルター関数は、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。
