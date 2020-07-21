---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マップ関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 26%

---


# マップ関数

[!DNL Profile Query Language] (PQL)オファーは、マップとのやり取りを容易にする機能です。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## Get

この `get` 関数は、特定のキーのマップの値を取得するために使用されます。

**書式**

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

**書式**

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

**書式**

```sql
{MAP}.values()
```

**例**

次のPQLクエリは、マップのすべての値を取得し `identityMap`ます。

```sql
identityMap.values()
```

## 次の手順

マップ関数について学習したら、PQLクエリ内で使用できます。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。
