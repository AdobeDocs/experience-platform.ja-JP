---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；その他の関数；その他
solution: Experience Platform
title: PQL その他の関数
description: 次の関数は、プロファイルクエリ言語（PQL）のその他の関数です。
exl-id: a6ed31a2-a649-4dc8-89b1-48c1170b7f16
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 68%

---

# その他の関数

次の関数は、 [!DNL Profile Query Language] (PQL) を参照してください。 その他の PQL 関数について詳しくは、 [[!DNL Profile Query Language] 概要](./overview.md).

## Let

`let` 関数を使用すると、式を変数として保存し、後からクエリで使用できます。

**形式**

```sql
let {VARIABLE} = {EXPRESSION}
```

**例**

次の PQL クエリは、合計が $100 以上 $1,000 未満のトランザクションでの製品合計すべてを、米ドルで取得します。

```sql
let S = (sum X.commerce.order.priceTotal over X from xEvent where X.commerce.order.currencyCode = "USD") in (S > 100 and S < 1000)
```

## 次の手順

これで、その他の関数について学習し、PQL クエリ内で使用できるようになりました。その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
