---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ重複排除 - 重複
topic: queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# データ重複排除 - 重複 [!DNL Query Service]

Adobe Experience Platform [!DNL Query Service] では、行全体を重複から削除する必要がある場合や、行のデータの一部だけが計算に含まれるので、特定のフィールドのセットを無視する場合に、データ重複排除 - 重複がサポートされます。 重複排除 - 重複の一般的なパターンでは、ウィンドウ全体でID `ROW_NUMBER()` 関数、またはIDのペアを順番に(XDM [!DNL Experience Data Model]`timestamp` フィールドを使用して)使用し、重複が検出された回数を表す新しいフィールドを返します。 この値がの場合、は元のインスタンス `1`を参照し、ほとんどの場合は使用するインスタンスを参照します。他のインスタンスはすべて無視します。 これは、多くの場合、集計カウントの実行のように高いレベルで重複排除 - 重複が行われる下位選択の中で行われ `SELECT` ます。

## 使用例

重複排除 - 重複の使用例の中には、日付範囲全体でグローバルなものや、内の単一の訪問者またはエンドユーザーIDに制限されるものもあり `identityMap`ます。

次のドキュメントでは、3つの一般的な使用例の重複を除外するための、下位選択と完全なサンプルクエリの例を概説します。
- [ExperienceEvents](#experienceevents)
- [購入](#purchases)
- [指標](#metrics)

### ExperienceEvents {#experienceevents}

重複のExperienceEventsの場合は、行全体を無視するとよいでしょう。

>[!CAUTION]
>
>の多くのデータセット [!DNL Experience Platform]（Adobe Data Connectorで作成されたものを含む）には、既にExperienceEventレベルの重複排除 - 重複が適用されています。 したがって、このレベルの重複排除 - 重複を再適用する必要がなく、クエリが遅くなります。 DataSetのソースを理解し、ExperienceEventレベルでの重複排除 - 重複が既に適用されているかどうかを知ることが重要です。 ストリーム化されるすべてのデータセット(例えば、Adobe Targetからのデータセット)に対しては、「少なくとも1回」のセマンティックがあるので、ExperienceEventレベルの重複排除 - 重複を適用する必要があります。

**範囲：** グローバル

**ウィンドウキー：** id

#### 下位選択

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

重複の購入がある場合は、ExperienceEvent行のほとんどをそのまま残し、購入に結び付けられたフィールド（指標など）は無視するとよいでしょう。 `commerce.orders` 購入の場合、購入IDには特別なフィールドがあります。 このフィールドは `commerce.order.purchaseID`です。

**範囲：** 訪問者

**ウィンドウキー：** identityMap[$名前空間].id &amp; commerce.order.purchaseID

#### 下位選択

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

オプションの一意のIDを使用する指標があり、そのIDの重複が表示される場合は、その指標値を無視して、ExperienceEventの残りの部分を保持する必要があります。 XDMでは、ほとんどすべての指標で、重複排除 - 重複に使用できるオプションの `Measure``id` フィールドを含むデータ型が使用されます。

**範囲：** 訪問者

**ウィンドウキー：** identityMap[$名前空間].idおよび測定オブジェクトのID

#### 下位選択

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
