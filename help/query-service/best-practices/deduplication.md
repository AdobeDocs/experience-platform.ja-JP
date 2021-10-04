---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；データ重複排除；重複排除；
solution: Experience Platform
title: クエリサービスでのデータ重複排除
topic-legacy: queries
type: Tutorial
description: このドキュメントでは、エクスペリエンスイベント、購入、指標の 3 つの一般的な使用例の重複を排除する、サブ選択と完全なサンプルクエリの例を説明します。
exl-id: 46ba6bb6-67d4-418b-8420-f2294e633070
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 18%

---

# [!DNL Query Service] でのデータの重複排除

Adobe Experience Platform [!DNL Query Service] は、データの重複排除をサポートしています。 データの重複排除は、行全体を計算から削除する必要がある場合や、行内のデータの一部のみが重複する情報なので、特定のフィールドのセットを無視する必要がある場合に実行できます。

重複排除は、通常、注文時間の経過に伴い、ID（または ID の組み合わせ）のウィンドウ全体で `ROW_NUMBER()` 関数を使用し、重複が検出された回数を表す新しいフィールドを返します。 時刻は、[!DNL Experience Data Model] (XDM) `timestamp` フィールドを使用して表されることが多いです。

`ROW_NUMBER()` の値が `1` の場合は、元のインスタンスを参照します。 一般に、これは使用するインスタンスです。 多くの場合、集計カウントの実行など、上位の `SELECT` で重複排除が実行されるサブ選択内でおこなわれます。

重複排除の使用例は、グローバルにすることも、`identityMap` 内の単一のユーザー ID やエンドユーザー ID に制限することもできます。

このドキュメントでは、次の 3 つの一般的な使用例に対して重複排除を実行する方法について説明します。エクスペリエンスイベント、購入および指標。

各例には、スコープ、ウィンドウ・キー、重複排除メソッドの概要、および完全な SQL クエリが含まれます。

## エクスペリエンスイベント {#experience-events}

エクスペリエンスイベントが重複している場合は、行全体を無視する可能性が高くなります。

>[!CAUTION]
>
>Adobe Analytics Data Connector で作成されたデータセットを含む、 [!DNL Experience Platform] の多くのデータセットには、既にエクスペリエンスイベントレベルの重複排除が適用されています。 したがって、このレベルの重複排除を再適用する必要はなく、最適用するとクエリの速度が低下します。
>
>データセットのソースを理解し、エクスペリエンスイベントレベルで重複排除が既に適用されているかどうかを把握することが重要です。 ストリーミングされるすべてのデータセット ( 例えば、Adobe Targetからのデータセット ) に対して、**エクスペリエンスイベントレベルの重複排除を適用する必要があります。これらのデータソースには「少なくとも 1 回」のセマンティクスがあるからです。**

**範囲：**&#x200B;グローバル

**ウィンドウキー：** `id`

### 重複排除の例

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

重複の購入がある場合、エクスペリエンスイベント行のほとんどを残しておき、購入に関連付けられたフィールド（`commerce.orders` 指標など）は無視することができます。 購入には、購入 ID(`commerce.order.purchaseID`) 用の特別なフィールドが含まれます。

**範囲：**&#x200B;訪問者

**ウィンドウキー：** identityMap[$NAMESPACE].id &amp; commerce.order.purchaseID

### 重複排除の例

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

オプションの一意の ID を使用する指標があり、その ID の重複が表示された場合は、その指標値を無視して、残りのエクスペリエンスイベントを維持する必要があります。

XDM では、ほとんどすべての指標で、重複排除に使用できるオプションの `Measure` フィールドを含む `id` データ型を使用します。

**範囲：**&#x200B;訪問者

**ウィンドウキー：** identityMap[$NAMESPACE].id および測定オブジェクトの ID

### 重複排除の例

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

このドキュメントでは、クエリサービス内でデータの重複排除を実行する方法と、データの重複排除の例について説明しました。 クエリサービスを使用してクエリを記述する際のベストプラクティスの詳細については、『[ クエリの記述に関するガイド ](./writing-queries.md)』を参照してください。
