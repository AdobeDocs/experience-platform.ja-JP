---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マップ関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 6%

---


# マップ関数

プロファイルクエリ言語(PQL)オファー機能を使用すると、マップとのやり取りが容易になります。 その他のPQL機能の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照してください。

## Get

この `get` 関数は、特定のキーのマップの値を取得するために使用されます。

**形式**

```sql
{MAP}.get({STRING})
```

**例**

次のPQLクエリは、キーのIDマップの値を取得し `example@example.com`ます。

```sql
identityMap.get("example@example.com")
```

## キー

この `keys` 関数は、特定のマップのすべてのキーを取得するために使用します。

**形式**

```sql
{MAP}.keys()
```

**例**

次のPQLクエリは、マップのすべてのキーを取得し `identityMap`ます。

```sql
identityMap.keys()
```

## 値

この `values` 関数は、指定したマップのすべての値を取得するために使用されます。

**形式**

```sql
{MAP}.values()
```

**例**

次のPQLクエリは、マップのすべての値を取得し `identityMap`ます。

```sql
identityMap.values()
```

## 次の手順

マップ関数について学習したら、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、 [プロファイルクエリ言語の概要を参照してください](./overview.md)。
