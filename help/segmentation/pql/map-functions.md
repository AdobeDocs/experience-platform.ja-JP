---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: map 関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 84a5b992639c1cabfdeaec5262964c9873826592
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 85%

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
