---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: その他の関数
topic: developer guide
translation-type: tm+mt
source-git-commit: d7ec6240864916d3b54db8bd641f4917a38f9480
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 3%

---


# その他の関数

次の関数は、プロファイルクエリ言語(PQL)のその他の関数です。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## Let

この `let` 関数を使用すると、式を変数として保存し、後でクエリで使用できます。

**形式**

```sql
let {VARIABLE} = {EXPRESSION}
```

**例**

次のPQLクエリは、USDで表したトランザクションでの製品合計の合計をすべて取得します。合計は$100を超え、$1000未満です。

```sql
let S = (sum X.commerce.order.priceTotal over X from xEvent where X.commerce.order.currencyCode = "USD") in (S > 100 and S < 1000)
```

## 次の手順

これで、その他の機能について学習できたので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
