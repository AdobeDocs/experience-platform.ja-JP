---
solution: Experience Platform
title: PQL マップ関数
description: Profile Query Language（PQL）には、マップとのやり取りを容易にする機能が用意されています。
exl-id: f23616f2-c0dd-40ce-8cfc-c757542fbd05
source-git-commit: c4d034a102c33fda81ff27bee73a8167e9896e62
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 46%

---

# マップ関数

[!DNL Profile Query Language] （PQL）には、マップとのやり取りを容易にする機能が用意されています。 その他のPQL関数について詳しくは、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

## 取得

`get` 関数は、指定されたキーのマップの値をオブジェクトとして取得するために使用されます。

**形式**

```sql
{MAP}.get({STRING})
```

**例**

次の PQL クエリは、キー `example@example.com` の ID マップの値を取得します。

```sql
identityMap.get("example@example.com")
```

## keys

`keys` 関数は、特定のマップのすべてのキーを配列またはリストとして取得するために使用します。

**形式**

```sql
{MAP}.keys()
```

**例**

次の PQL クエリは、マップ `identityMap` のすべてのキーを取得します。

```sql
identityMap.keys()
```

## values

`values` 関数は、特定のマップのすべての値を配列またはリストとして取得するために使用されます。

**形式**

```sql
{MAP}.values()
```

**例**

次の PQL クエリは、マップ `identityMap` のすべての値を取得します。

```sql
identityMap.values()
```

## 次の手順

ここでは、map 関数について学習しました。これで、map 関数を PQL クエリで使用できます。その他の PQL 関数について詳しくは、「[プロファイルクエリ言語の概要](./overview.md)」を参照してください。
