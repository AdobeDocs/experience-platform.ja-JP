---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；データ重複排除；重複排除；
solution: Experience Platform
title: クエリサービスでのデータ重複排除
type: Tutorial
description: このドキュメントでは、エクスペリエンスイベント、購入、指標の 3 つの一般的な使用例の重複を排除するための、サブ選択と完全なサンプルクエリの例について説明します。
exl-id: 46ba6bb6-67d4-418b-8420-f2294e633070
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 15%

---

# でのデータ重複排除 [!DNL Query Service]

Adobe Experience Platform [!DNL Query Service] は、データの重複排除をサポートします。 データの重複排除は、行内のデータの一部のみが重複した情報なので、行全体を計算から削除する必要がある場合や、特定のフィールドのセットを無視する場合に実行できます。

重複排除は、通常、 `ROW_NUMBER()` 関数は、注文時間の ID（または ID の組み合わせ）のウィンドウをまたいで機能します。この関数は、重複が検出された回数を表す新しいフィールドを返します。 時間は、多くの場合、 [!DNL Experience Data Model] (XDM) `timestamp` フィールドに入力します。

When the value of `ROW_NUMBER()` が `1`の場合は、元のインスタンスを参照します。 一般に、使用したいインスタンスがこのインスタンスになります。 多くの場合、集計カウントの実行など、上位の `SELECT` で重複排除が実行されるサブ選択内でおこなわれます。

重複排除の使用例は、グローバルにすることも、単一のユーザーに制限することも、 `identityMap`.

このドキュメントでは、次の 3 つの一般的な使用例に対して重複排除を実行する方法について説明します。エクスペリエンスのイベント、購入および指標。

各例には、範囲、ウィンドウキー、重複排除メソッドの概要、および完全な SQL クエリが含まれます。

## エクスペリエンスイベント {#experience-events}

エクスペリエンスイベントが重複している場合は、行全体を無視する可能性が高くなります。

>[!CAUTION]
>
>の多くのデータセット [!DNL Experience Platform]( Adobe Analytics Data Connector で作成されたものを含む ) には、既にエクスペリエンスイベントレベルの重複排除が適用されています。 したがって、このレベルの重複排除を再適用する必要はなく、最適用するとクエリの速度が低下します。
>
>データセットのソースを理解し、エクスペリエンスイベントレベルで重複排除が既に適用されているかどうかを把握することが重要です。 ストリーミングされるすべてのデータセット ( 例えば、Adobe Targetからのデータセット ) の場合、 **遺言** これらのデータソースには「少なくとも 1 回」のセマンティクスが含まれるので、Experience-Event-level の重複排除を適用する必要があります。

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

重複の購入がある場合は、 [!DNL Experience Event] 行に含めるが、購入に関連付けられたフィールド ( `commerce.orders` 指標 ) を参照してください。 購入には、購入 ID 用の特別なフィールド ( `commerce.order.purchaseID`.

次を使用することをお勧めします。 `purchaseID` は XDM 内の購入 ID の標準のセマンティックフィールドなので、訪問者の範囲内で使用します。 重複する購入データは、クエリの方がグローバルスコープを使用するよりも速く、複数の訪問者 ID をまたいで購入 ID が重複する可能性は低いので、訪問者の範囲は削除する場合に推奨されます。

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

>[!NOTE]
>
>元の Analytics データに訪問者 ID 間で重複した購入 ID がある場合は、 **may** すべての訪問者に対して購入 ID の重複カウントを実行する必要があります。 購入 ID が存在しない場合、このメソッドでは、クエリをできるだけ早く保つために、代わりにイベント ID を使用する条件を含める必要があります。

### 完全な例

次の例では、条件句を使用して、購入 ID が存在しない場合にイベント ID を使用します。

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

オプションの一意の ID を使用する指標があり、その ID の重複が表示される場合は、その指標値を無視して、残りのエクスペリエンスイベントを保持する必要が生じます。

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

このドキュメントでは、データの重複排除の例と、クエリサービス内でデータの重複排除を実行する方法について説明しました。 クエリサービスを使用してクエリを記述する際のベストプラクティスについては、 [クエリガイドの記述](../best-practices/writing-queries.md).
