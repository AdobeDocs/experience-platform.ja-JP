---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクト関数
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 31%

---


# オブジェクト関数

[!DNL Profile Query Language] (PQL)オファーは、オブジェクトとのやり取りを簡単にするために機能します。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。

## Nullである

この `isNull` 関数は、オブジェクト参照が存在しないかどうかを判定します。

**書式**

```sql
{OBJECT}.isNull()
```

**例**

次のPQLクエリは、ユーザーの自宅の住所が存在しないかどうかを確認します。

```sql
person.homeAddress.isNull()
```

## ヌルでない

この `isNotNull` 関数は、オブジェクト参照が存在するかどうかを判定します。

**書式**

```sql
{OBJECT}.isNotNull()
```

**例**

次のPQLクエリは、ユーザーの自宅の住所が存在するかどうかを確認します。

```sql
person.homeAddress.isNotNull()
```

## 次の手順

オブジェクトの機能について学習したので、PQLクエリ内で使用できます。 その他の PQL 関数について詳しくは、[プロファイルクエリ言語の概要](./overview.md)を参照してください。