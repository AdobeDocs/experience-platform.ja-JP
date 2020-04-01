---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ重複排除 - 重複
topic: queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# データ重複排除 - 重複(クエリサービス)

Adobe Experience Platformクエリサービスは、データの重複排除 - 重複をサポートします。これは、行の一部のみが重複なので、行全体を演算から削除するか、特定のフィールドのセットを無視する必要がある場合があります。 重複排除 - 重複の一般的なパターンでは、 `ROW_NUMBER()``timestamp` IDやIDの組み合わせを順番に(Experience Data Model(XDM)フィールドを使用して)関数を使用し、重複が検出された回数を表す新しいフィールドを返します。 この値がの場合、 `1`は元のインスタンス、およびほとんどの場合、使用するインスタンスを参照し、他のすべてのインスタンスは無視します。 これは、多くの場合、重複排除 - 重複カウントを実行するように高いレベルで集計が行われる下位選択の中で `SELECT` 行われます。

## 使用例

重複排除 - 重複の使用例の中には、日付範囲全体でグローバルなものや、内の単一の訪問者またはエンドユーザーIDに制限されるものもありま `identityMap`す。

次のドキュメントでは、3つの一般的な使用例の重複を除外するための、サブセレクトと完全なサンプルクエリの例について説明します。
- [ExperienceEvents](#experienceevents)
- [購入](#purchases)
- [指標](#metrics)

### ExperienceEvents {#experienceevents}

ExperienceEvents重複の場合、行全体を無視するとよいでしょう。

>[!CAUTION] Adobe Analytics Data Connectorで作成されたものを含む、Experience Platformの多くのデータセットには、既にExperienceEventレベルの重複排除 - 重複が適用されています。 したがって、このレベルの重複排除 - 重複を再適用する必要はなく、クエリが遅くなります。 データセットのソースを理解し、ExperienceEventレベルでの重複排除 - 重複が既に適用されているかどうかを知ることが重要です。 ストリーミングされるすべてのデータセット(例えば、アドビのターゲットからのデータセット)に対して、ExperienceEventレベルの重複排除 - 重複を適用する必要があります。これらのデータソースには「少なくとも1回」のセマンティックが含まれているからです。

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

重複の購入がある場合は、ExperienceEvent行のほとんどを残しておき、購入に関連付けられたフィールド（指標など）は無視すること `commerce.orders` が考えられます。 購入の場合、購入IDの特別なフィールドがあります。 このフィールドはで `commerce.order.purchaseID`す。

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

オプションの一意のIDを使用する指標があり、そのIDの重複が表示される場合は、その指標値を無視して、残りのExperienceEventを保持する必要があります。 XDMでは、ほとんどすべての指標で、重複排除 - 重複に使 `Measure` 用できるオプションのフィールドを含む `id` データ型が使用されます。

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
