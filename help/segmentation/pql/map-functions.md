---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;Segmentation Service;pql;PQL;Profile Query Language;map functions;map;
solution: Experience Platform
title: map 関数
topic: developer guide
description: Profile Query Language（PQL）には、マップとのやり取りを容易にする関数が用意されています。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 86%

---


# map 関数

[!DNL Profile Query Language] (PQL)オファーは、マップとのやり取りを容易にする機能です。 More information about other PQL functions can be found in the [[!DNL Profile Query Language] overview](./overview.md).

## get

`get` 関数は、特定のキーのマップの値を取得するために使用されます。

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

`keys` 関数は、特定のマップのすべてのキーを取得するために使用されます。

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

`values` 関数は、特定のマップのすべての値を取得するために使用されます。

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
