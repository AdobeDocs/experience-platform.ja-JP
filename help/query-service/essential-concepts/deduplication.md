---
keywords: Experience Platform;ホーム;人気のトピック;クエリサービス;Query Service;データ重複排除;重複排除;
solution: Experience Platform
title: クエリサービスでのデータ重複排除
type: Tutorial
description: このドキュメントでは、エクスペリエンスイベント、購入および指標の 3 つの一般的なユースケースの重複を排除するための、サブ選択と完全なサンプルクエリの概要を説明します。
exl-id: 46ba6bb6-67d4-418b-8420-f2294e633070
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 100%

---

# [!DNL Query Service] でのデータ重複排除

Adobe Experience Platform [!DNL Query Service] では、データ重複排除をサポートしています。 データ重複排除は、行内のデータの一部のみが重複した情報なので、行全体を計算から除外したり特定のフィールドセットを無視したりする必要がある場合に行われる可能性があります。

重複排除では、通常、時間の経過と共に ID（または ID のペア）の期間にわたって `ROW_NUMBER()` 関数を使用する必要があります。この関数は、重複が検出された回数を表す新しいフィールドを返します。 時間は、多くの場合、[!DNL Experience Data Model]（XDM）`timestamp` フィールドを使用して表されます。

`ROW_NUMBER()` の値が `1` となっている場合は、元のインスタンスを指します。 一般に、使用を希望するインスタンスになります。 多くの場合、集計カウントの実行など、上位の `SELECT` で重複排除が実行されるサブ選択内でおこなわれます。

重複排除のユースケースは、グローバルな場合もあれば、`identityMap` 内の単一のユーザー ID またはエンドユーザー ID に制限される場合もあります。

このドキュメントでは、エクスペリエンスイベント、購入および指標の 3 つの一般的なユースケースについて重複排除を実行する方法の概要を説明します。

各例には、範囲、期間キー、重複排除方法の概要および完全な SQL クエリが含まれます。

## エクスペリエンスイベント {#experience-events}

エクスペリエンスイベントが重複する場合は、おそらく行全体を無視することになるでしょう。

>[!CAUTION]
>
>Adobe Analytics データコネクタで生成されたものを含め、[!DNL Experience Platform] の多くのデータセットには、エクスペリエンスイベントレベルの重複排除が既に適用されています。したがって、このレベルの重複排除を再適用する必要はなく、最適用するとクエリの速度が低下します。
>
>データセットのソースを理解し、エクスペリエンスイベントレベルの重複排除が既に適用されているかどうかを把握することが重要です。ストリーミングされるすべてのデータセット（例えば Adobe Target のデータセット）の場合は、エクスペリエンスイベントレベルの重複排除を適用することが&#x200B;**必要になり**&#x200B;ます。これらのデータソースには「少なくとも 1 回」のセマンティクスがあるからです。

**範囲：**&#x200B;グローバル

**期間キー：**`id`

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

重複した購入がある場合は、おそらく、[!DNL Experience Event] 行のほとんどを残し、購入に関連付けられたフィールド（`commerce.orders` 指標など）を無視することになるでしょう。購入には、購入 ID 用の特別なフィールド `commerce.order.purchaseID` が含まれています。

XDM における購入 ID の標準のセマンティックフィールドになるので、訪問者の範囲内で `purchaseID` を使用することをお勧めします。クエリの方がグローバルスコープを使用するよりも速く、複数の訪問者 ID にわたって購入 ID が重複する可能性は低いので、重複する購入データの削除には訪問者の範囲が推奨されます。

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
>元の Analytics データに、複数の訪問者 ID をまたいで重複した購入 ID がある場合は、訪問者全体で購入 ID の重複カウントの実行が必要になる&#x200B;**可能性があります**。 購入 ID が存在しない場合、この方法では、代わりにイベント ID を使用してクエリをできるだけ高速に保つ条件を含める必要があります。

### 完全な例

次の例では、購入 ID が存在しない場合に、条件句を使用してイベント ID を使用しています。

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

オプションの一意の ID を使用している指標があり、その ID の重複が現れる場合は、おそらく、その指標値を無視して残りのエクスペリエンスイベントを保持する必要があるでしょう。

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

このドキュメントでは、データ重複排除の例を示し、クエリサービス内でデータ重複排除を実行する方法の概要を説明しました。 クエリサービスを使用してクエリを記述する際のベストプラクティスについて詳しくは、[クエリ作成ガイド](../best-practices/writing-queries.md)を参照してください。
