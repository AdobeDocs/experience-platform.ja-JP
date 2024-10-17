---
solution: Experience Platform
title: PQL オブジェクト関数
description: Profile Query Language（PQL）は、オブジェクトとのインタラクションを簡単にする機能を提供します。
exl-id: e65257d8-5bc8-46c8-8487-33bc7ce4059b
source-git-commit: c4d034a102c33fda81ff27bee73a8167e9896e62
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 52%

---

# オブジェクト関数

[!DNL Profile Query Language] （PQL）は、オブジェクトとのインタラクションを簡単にする機能を提供します。 その他のPQL関数について詳しくは、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

## null である

`isNull` 関数は、オブジェクト参照がブール値として存在しないことを判別します。

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

`isNotNull` 関数は、オブジェクト参照がブール値として存在することを判別します。

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
