---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；データ重複排除 - 重複;重複排除 - 重複;
solution: Experience Platform
title: クエリサービスでのデータ重複排除 - 重複
topic-legacy: queries
type: Tutorial
description: このドキュメントでは、エクスペリエンスイベント、購入、指標の3つの一般的な使用例を重複排除するための、サブセレクトおよび完全なサンプルクエリの例について説明します。
exl-id: 46ba6bb6-67d4-418b-8420-f2294e633070
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 18%

---

# [!DNL Query Service]のデータ重複排除 - 重複

Adobe Experience Platform[!DNL Query Service]はデータ重複排除 - 重複をサポートしています。 データの重複排除 - 重複は、行全体を演算から削除する必要がある場合や、行のデータの一部だけが重複情報なので、特定のフィールドのセットを無視する場合に行われます。

一般的に、重複排除 - 重複では、ウィンドウ全体で`ROW_NUMBER()`関数を使用して、ID（またはIDのペア）を順番に求め、重複が検出された回数を表す新しいフィールドを返します。 時間は、[!DNL Experience Data Model] (XDM) `timestamp`フィールドを使用して表されます。

`ROW_NUMBER()`の値が`1`の場合、元のインスタンスを参照します。 一般に、これは使用したいインスタンスです。 多くの場合、集計カウントの実行など、上位の `SELECT` で重複排除が実行されるサブ選択内でおこなわれます。

重複排除 - 重複の使用例は、グローバルにすることも、1人のユーザーまたは`identityMap`内のエンドユーザーIDに制限することもできます。

このドキュメントでは、次の3つの一般的な使用例に対する重複排除 - 重複の実行方法を概説します。エクスペリエンスのイベント、購入および指標。

各例には、範囲、ウィンドウ・キー、重複排除 - 重複・メソッドの概要、および完全なSQLクエリが含まれます。

## エクスペリエンスイベント {#experience-events}

重複エクスペリエンスのイベントの場合、行全体を無視するとよいでしょう。

>[!CAUTION]
>
>[!DNL Experience Platform]内の多くのデータセット(Adobe AnalyticsのData Connectorが作成するものを含む)には、既にエクスペリエンスイベントレベルの重複排除 - 重複が適用されています。 したがって、このレベルの重複排除を再適用する必要はなく、最適用するとクエリの速度が低下します。
>
>データセットのソースを理解し、エクスペリエンスイベントレベルの重複排除 - 重複が既に適用されているかどうかを把握することが重要です。 ストリーム化されるデータセット(例えば、Adobe Targetからのデータセット)については、****&#x200B;エクスペリエンスイベントレベルの重複排除 - 重複を適用する必要があります。これらのデータソースには「少なくとも1回」のセマンティックがあるからです。

**範囲：**&#x200B;グローバル

**ウィンドウキー：** `id`

### 重複排除 - 重複の例

```sql
SELECT *,
  ROW_NUMBER()
    OVER (PARTITION BY id
          ORDER BY timestamp ASC
    ) AS id_dup_num
FROM experience_events
```

### 完全な例

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

## 購入 {#purchases}

重複を購入した場合は、エクスペリエンスイベントの行のほとんどをそのまま残し、購入に結び付けられたフィールド（`commerce.orders`指標など）は無視することが推奨されます。 購入には、購入IDの特別なフィールド(`commerce.order.purchaseID`)が含まれます。

**範囲：**&#x200B;訪問者

**ウィンドウキー：** identityMap[$NAMESPACE].id &amp; commerce.order.purchaseID

### 重複排除 - 重複の例

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

### 完全な例

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

## 指標 {#metrics}

オプションの一意のIDを使用する指標があり、そのIDの重複が表示される場合は、その指標値を無視して、エクスペリエンスイベントの残りの部分を維持する必要があります。

XDM では、ほとんどすべての指標で、重複排除に使用できるオプションの `Measure` フィールドを含む `id` データ型を使用します。

**範囲：**&#x200B;訪問者

**ウィンドウキー：** identityMap[$NAMESPACE].id および測定オブジェクトの ID

### 重複排除 - 重複の例

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

### 完全な例

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

## 次の手順

このドキュメントでは、クエリサービス内でのデータ重複排除 - 重複の実行方法、およびデータ重複排除 - 重複の例を説明しました。 クエリサービスを使用してクエリを記述する際のベストプラクティスについては、『[クエリの記述ガイド](./writing-queries.md)』を参照してください。
