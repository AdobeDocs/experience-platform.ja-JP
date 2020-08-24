---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: その他の関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 84a5b992639c1cabfdeaec5262964c9873826592
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 79%

---


# その他の関数

The following function is a miscellaneous function for [!DNL Profile Query Language] (PQL). More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

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
