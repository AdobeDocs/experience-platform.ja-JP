---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: その他の関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 31%

---


# その他の関数

次の関数は、 [!DNL Profile Query Language] (PQL)のその他の関数です。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## Let

この `let` 関数を使用すると、式を変数として保存し、後でクエリで使用できます。

**書式**

```sql
let {VARIABLE} = {EXPRESSION}
```

**例**

次のPQLクエリは、USDで表したトランザクションでの製品合計の合計をすべて取得します。合計は$100を超え、$1000未満です。

```sql
let S = (sum X.commerce.order.priceTotal over X from xEvent where X.commerce.order.currencyCode = "USD") in (S > 100 and S < 1000)
```

## 次の手順

これで、その他の機能について学習できたので、PQLクエリ内で使用できます。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
