---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；ql;PQL;プロファイルクエリ言語；フィルタ関数；フィルタ；
solution: Experience Platform
title: PQLフィルタ関数
topic-legacy: developer guide
description: フィルター関数は、プロファイルクエリ言語（PQL）で配列内のデータをフィルターするために使用します。
exl-id: 09d66be3-30dc-4488-84a1-cfd09c44470d
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 81%

---

# フィルター関数

フィルター関数 は[!DNL Profile Query Language] (PQL)の配列内のデータをフィルタするために使用されます。 他のPQL関数の詳細については、[[!DNL Profile Query Language] 概要](./overview.md)を参照してください。

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
