---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；pql;PQL；プロファイルクエリ言語；マップ関数；マップ；
solution: Experience Platform
title: PQL マップ関数
topic-legacy: developer guide
description: Profile Query Language（PQL）には、マップとのやり取りを容易にする関数が用意されています。
exl-id: f23616f2-c0dd-40ce-8cfc-c757542fbd05
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 81%

---

# マップ関数

[!DNL Profile Query Language]（PQL）の機能を使用すると、マップの操作が容易になります。その他の PQL 関数の詳細については、[[!DNL Profile Query Language]  概要 ](./overview.md) を参照してください。

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
