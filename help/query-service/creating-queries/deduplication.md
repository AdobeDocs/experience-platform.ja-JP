---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ重複排除
topic: queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 75%

---


# データ重複排除 - 重複 [!DNL Query Service]

Adobe Experience Platform [!DNL Query Service] supports data deduplication when it may be required to remove an entire row from a calculation or ignore a specific set of fields because only part of the data in the row is a duplicate. The common pattern for deduplication involves using the `ROW_NUMBER()` function across a window for an ID, or pair of IDs, over ordered time (using the [!DNL Experience Data Model] (XDM) `timestamp` field) to return a new field that represents the number of times a duplicate has been detected. この値が `1` の場合は元のインスタンスを参照します。ほとんどの場合は、使用したいインスタンスを参照し、他のすべてのインスタンスを無視します。多くの場合、集計カウントの実行など、上位の `SELECT` で重複排除が実行されるサブ選択内でおこなわれます。

## 使用例

重複排除の使用例の中には、日付範囲全体でグローバルなものや、`identityMap` 内の単一の訪問者またはエンドユーザー ID に制限されるものもあります。

次のドキュメントでは、3 つの一般的な使用例の重複を除外するための、サブ選択と完全なサンプルクエリの例について説明します。
- [ExperienceEvents](#experienceevents)
- [購入](#purchases)
- [指標](#metrics)

### ExperienceEvents {#experienceevents}

ExperienceEvents が重複した場合、行全体を無視する可能性が高くなります。

>[!CAUTION]
>
>Many DataSets in [!DNL Experience Platform], including those produced by the Adobe Analytics Data Connector, already have ExperienceEvent-level deduplication applied. したがって、このレベルの重複排除を再適用する必要はなく、最適用するとクエリの速度が低下します。データセットのソースを理解し、ExperienceEvent レベルで重複排除が既に適用されているかどうかを把握することが重要です。ストリーミングされるすべてのデータセット（例：Adobe Target からのデータセット）に対して、ExperienceEvent レベルで重複排除を適用する必要があります。これらのデータソースには「1 回以上」のセマンティクスが含まれているためです。

**範囲：**&#x200B;グローバル

**ウィンドウキー：** id

#### サブ選択

```sql
SELECT *,
  ROW_NUMBER()
    OVER (PARTITION BY id
          ORDER BY timestamp ASC
    ) AS id_dup_num
FROM experience_events
```

#### 完全な例

```sql
SELECT COUNT(*) AS num_events FROM (
  SELECT *,
    ROW_NUMBER()
      OVER (PARTITION BY id
            ORDER BY timestamp ASC
      ) AS id_dup_num
  FROM experience_events
) WHERE id_dup_num = 1
```

### 購入 {#purchases}

重複の購入がある場合は、ExperienceEvent 行のほとんどを残しておき、購入に関連付けられたフィールド（`commerce.orders` 指標など）は無視することができます。購入の場合、購入 ID 用の特別なフィールドがあります。このフィールドは `commerce.order.purchaseID` です。

**範囲：**&#x200B;訪問者

**ウィンドウキー：** identityMap[$NAMESPACE].id &amp; commerce.order.purchaseID

#### サブ選択

```sql
SELECT *,
  IF(LENGTH(commerce.`order`.purchaseID) > 0,
    ROW_NUMBER()
      OVER (PARTITION BY identityMap['ECID'].id, commerce.`order`.purchaseID
            ORDER BY timestamp ASC
      ),
    1) AS purchaseID_dup_num
FROM experience_events
```

#### 完全な例

```sql
SELECT SUM(commerce.purchases.value) AS num_purchases FROM (
  SELECT *,
    ROW_NUMBER()
      OVER (PARTITION BY id
            ORDER BY timestamp ASC
      ) AS id_dup_num,
    IF(LENGTH(commerce.`order`.purchaseID) > 0,
      ROW_NUMBER()
        OVER (PARTITION BY identityMap['ECID'].id, commerce.order.purchaseID
              ORDER BY timestamp ASC
        ),
      1) AS purchaseID_dup_num
  FROM experience_events
) WHERE id_dup_num = 1 AND purchaseID_dup_num = 1
```

### 指標 {#metrics}

オプションの一意の ID を使用する指標があり、その ID の重複が表示される場合は、その指標値を無視して、残りの ExperienceEvent を保持する必要があります。XDM では、ほとんどすべての指標で、重複排除に使用できるオプションの `Measure` フィールドを含む `id` データ型を使用します。

**範囲：**&#x200B;訪問者

**ウィンドウキー：** identityMap[$NAMESPACE].id および測定オブジェクトの ID

#### サブ選択

```sql
SELECT *,
  IF(LENGTH(application.launches.id) > 0,
    ROW_NUMBER()
      OVER (PARTITION BY identityMap['ECID'].id, application.launches.id
            ORDER BY timestamp ASC
      ),
    1) AS launchesID_dup_num
FROM experience_events
```

#### 完全な例

```sql
SELECT SUM(application.launches.value) AS num_launches FROM (
  SELECT *,
    ROW_NUMBER()
      OVER (PARTITION BY id
            ORDER BY timestamp ASC
      ) AS id_dup_num,
    IF(LENGTH(application.launches.id) > 0,
      ROW_NUMBER()
        OVER (PARTITION BY identityMap['ECID'].id, application.launches.id
              ORDER BY timestamp ASC
        ),
      1) AS launchesID_dup_num
  FROM experience_events
) WHERE id_dup_num = 1 AND launchesID_dup_num = 1
```
