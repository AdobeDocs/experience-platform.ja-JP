---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；オブジェクト関数；オブジェクト；
solution: Experience Platform
title: PQL オブジェクト関数
topic-legacy: developer guide
description: プロファイルクエリ言語（PQL）は、オブジェクトとのやり取りをより簡単にするための機能を備えています。
exl-id: e65257d8-5bc8-46c8-8487-33bc7ce4059b
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 70%

---

# オブジェクト関数

[!DNL Profile Query Language] (PQL) は、オブジェクトとのやり取りをより簡単にする関数を提供します。その他の PQL 関数の詳細については、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

## Is null

`isNull` 関数は、オブジェクト参照が存在しないかどうかを判定します。

**形式**

```sql
{OBJECT}.isNull()
```

**例**

次の PQL クエリは、ユーザーの自宅住所が存在しないかどうかを確認します。

```sql
person.homeAddress.isNull()
```

## isNotNull

`isNotNull` 関数は、オブジェクト参照が存在するかどうかを判定します。

**形式**

```sql
{OBJECT}.isNotNull()
```

**例**

次の PQL クエリは、ユーザーの自宅住所が存在するかどうかを確認します。

```sql
person.homeAddress.isNotNull()
```

## 次の手順

ここで学習したオブジェクト関数は、PQL クエリ内で使用できます。その他の PQL 関数について詳しくは、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。
