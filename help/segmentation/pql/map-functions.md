---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マップ関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 92f92f480f29f7d6440f4e90af3225f9a1fcc3d0

---


# マップ関数

プロファイルクエリ言語(PQL)オファーは、マップとのやり取りを容易にする機能です。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。

## 取得

この関 `get` 数は、特定のキーのマップの値を取得するために使用されます。

**形式**

```sql
{MAP}.get({STRING})
```

**例**

次のPQLクエリは、キーのIDマップの値を取得します `example@example.com`。

```sql
identityMap.get("example@example.com")
```

## キー

この関 `keys` 数は、特定のマップのすべてのキーを取得するために使用されます。

**形式**

```sql
{MAP}.keys()
```

**例**

次のPQLクエリは、マップのすべてのキーを取得しま `identityMap`す。

```sql
identityMap.keys()
```

## 値

この関 `values` 数は、特定のマップのすべての値を取得するために使用されます。

**形式**

```sql
{MAP}.values()
```

**例**

次のPQLクエリは、マップのすべての値を取得しま `identityMap`す。

```sql
identityMap.values()
```

## 次の手順

これで、マップ関数の学習が終わったので、PQLクエリ内で使用できます。 その他のPQL関数の詳細については、「 [プロファイルクエリ言語の概要](./overview.md)」を参照。
