---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: その他の関数
topic: developer guide
translation-type: tm+mt
source-git-commit: d7ec6240864916d3b54db8bd641f4917a38f9480

---


# その他の関数

次の関数は、プロファイルクエリ言語(PQL)のその他の関数です。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## レット

この関 `let` 数を使用すると、式を変数として保存し、後でクエリで使用できます。

**形式**

```sql
let {VARIABLE} = {EXPRESSION}
```

**例**

次のPQLクエリは、合計が$100を超え$1000未満のUSDでのトランザクションとの製品合計をすべて取得します。

```sql
let S = (sum X.commerce.order.priceTotal over X from xEvent where X.commerce.order.currencyCode = "USD") in (S > 100 and S < 1000)
```

## 次の手順

これで、その他の機能について学習したので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
