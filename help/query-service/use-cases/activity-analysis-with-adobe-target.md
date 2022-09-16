---
title: Adobe Targetでのアクティビティ分析
description: このドキュメントでは、クエリサービスを使用して、Adobe Targetデータを使用して作成されたデータセットから実用的なインサイトを作成する方法を説明します。
exl-id: a5181ee2-1e1c-405d-8dfe-5a32c28ac9f1
source-git-commit: d573c01a0aa9989f581796a0be4aec6904ffc569
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 12%

---

# Adobe Targetでのアクティビティ分析

Adobe Experience Platformでは、Experience Data Model(XDM) フィールドを使用してAdobe Targetからデータを取り込み、クエリサービスで使用するデータセットを作成できます。 Adobe Targetはコンテンツをカスタマイズし、ユーザーエクスペリエンスをパーソナライズするように設計されているので、これらのデータセットに対してクエリを実行すると、SQL を通じてユーザーアクティビティを分析し、高度にパーソナライズされ、焦点を絞ったインサイトを提供します。

このドキュメントでは、顧客の行動と特性に基づく一般的な使用例を示す、様々な SQL クエリ例を示します。

## はじめに

次の各使用例では、パラメーター化された SQL クエリの例が、カスタマイズ用のテンプレートとして提供されています。 が表示される場所にパラメーターを入力 `{ }` を参照してください。

## 高レベルの XDM フィールド部分マッピング

次の表に、一般的な Target フィールドと、それらがマッピング先の対応する XDM フィールドを示します。

>[!NOTE]
>
>の使用 `[ ]` XDM フィールド内には、配列が含まれます。

| ターゲットフィールド名 | XDM フィールド名 | 備考 |
|---|---|---|
| `mboxName` | `_experience.target.mboxname` | N/A |
| アクティビティ ID | `_experience.target.activities.activityID` | 該当なし |
| エクスペリエンス ID | `_experience.target.activities[].activityEvents[]._experience.target.activity.activityevent.context.experienceID` | 該当なし |
| Segment ID | `_experience.target.activities[].activityEvents[].segmentEvents[].segmentID._id` | 該当なし |
| イベント範囲 | `_experience.target.activities[].activityEvents[].eventScope` | このフィールドは、新規訪問者と新規訪問を追跡します。 |
| 手順 ID | `_experience.target.activities[].activityEvents[]._experience.target.activity.activityevent.context.stepID` | このフィールドは、Adobe Campaignのカスタムステップ ID です。 |
| 価格合計 | commerce.order.priceTotal | 該当なし |

>[!IMPORTANT]
>
>Target データを使用して自動的に作成されたデータセットの名前は「Adobe Target Experience Events」です。 このデータセットをクエリで使用する場合は、名前を使用します `adobe_target_experience_events`.

## 目標

ユーザーアクティビティを分析することで、特定のオーディエンスのコンテンツをパーソナライズし、個々のエンティティに対して様々なバージョンのコンテンツをテストできます。 さらに、特定の期間または個々のユーザーに対して特定のアクティビティを分析することで、個々のアクティビティのパフォーマンスをより明確に把握できます。 この組み合わせ分析の結果を利用して、個々のアクティビティのパフォーマンスを把握することができます。

次のパーソナライゼーションの使用例は、Adobe Targetのデータを使用して作成し、ユーザーアクティビティに焦点を当てて、ビジネスアプリケーションに対する顧客の行動に関する有益なインサイトを作成します。

このガイドでは、使用例を通じて以下の主要概念を説明します。

* 指定した日のアクティビティ ID のパフォーマンス（数、詳細、関連するエクスペリエンス ID など）を把握する。
* アクティビティの訪問者とイベントの範囲を決定する。
* エクスペリエンス ID、セグメント ID、アクティビティ ID の訪問者数、訪問数、インプレッション情報を収集する。

### 指定した日の 1 時間ごとのアクティビティ数を生成

```sql
SELECT
  Hour,
  ActivityID,
  COUNT(ActivityID) AS Instances
FROM
(
  SELECT
    date_format(from_utc_timestamp(timestamp, 'America/New_York'), 'yyyy-MM-dd HH') AS Hour,
    EXPLODE(_experience.target.activities.activityID) AS ActivityID
  FROM adobe_target_experience_events
  WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}') AND
    _experience.target.activities IS NOT NULL
)
GROUP BY Hour, ActivityID
ORDER BY Hour DESC, Instances DESC
LIMIT 24
```

### 特定の特定のに関する 1 時間ごとの詳細を生成

```sql
SELECT
  date_format(from_utc_timestamp(timestamp, 'America/New_York'), 'yyyy-MM-dd HH') AS Hour,
  _experience.target.activities.activityID AS ActivityID,
  COUNT(ActivityID) AS Instances
FROM adobe_target_experience_events
WHERE
  array_contains( _experience.target.activities.activityID, {Activity ID} ) AND
    TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}') AND
  _experience.target.activities IS NOT NULL
GROUP BY Hour, ActivityID
ORDER BY Hour DESC
LIMIT 24
```

### 特定の日の特定のアクティビティのエクスペリエンス ID のリストを決定する

```sql
SELECT
  Day,
  Activities.activityID,
  ExperienceID,
  COUNT(ExperienceID) AS Instances
FROM
(
  SELECT
    Day,
    Activities,
    EXPLODE(Activities.activityEvents._experience.target.activity.activityevent.context.experienceID) AS ExperienceID
  FROM
  (
    SELECT
      date_format(from_utc_timestamp(timestamp, 'America/New_York'), 'yyyy-MM-dd') AS Day,
      EXPLODE(_experience.target.activities) AS Activities
    FROM adobe_target_experience_events
    WHERE
      TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}') AND
      _experience.target.activities IS NOT NULL
  )
  WHERE Activities.activityID = {activity_id}
)
GROUP BY Day, Activities.activityID, ExperienceID
ORDER BY Day DESC, Instances DESC
LIMIT 20
```

### 指定した日のアクティビティ ID ごとのインスタンス別イベント範囲（訪問者、訪問、インプレッション）のリストを返す

```sql
SELECT
  Day,
  Activities.activityID,
  ExperienceID,
  COUNT(ExperienceID) AS Instances
FROM
(
  SELECT
    Day,
    Activities,
    EXPLODE(Activities.activityEvents._experience.target.activity.activityevent.context.experienceID) AS ExperienceID
  FROM
  (
    SELECT
      date_format(from_utc_timestamp(timestamp, 'America/New_York'), 'yyyy-MM-dd') AS Day,
      EXPLODE(_experience.target.activities) AS Activities
    FROM adobe_target_experience_events
    WHERE
      TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}') AND
      _experience.target.activities IS NOT NULL
  )
  WHERE Activities.activityID = {activity_id}
)
GROUP BY Day, Activities.activityID, ExperienceID
ORDER BY Day DESC, Instances DESC
LIMIT 20
```

### 特定の日のアクティビティごとの訪問者数、訪問数およびインプレッション数を特定

```sql
SELECT
  Day,
  Activities.activityID,
  ExperienceID,
  COUNT(ExperienceID) AS Instances
FROM
(
  SELECT
    Day,
    Activities,
    EXPLODE(Activities.activityEvents._experience.target.activity.activityevent.context.experienceID) AS ExperienceID
  FROM
  (
    SELECT
      date_format(from_utc_timestamp(timestamp, 'America/New_York'), 'yyyy-MM-dd') AS Day,
      EXPLODE(_experience.target.activities) AS Activities
    FROM adobe_target_experience_events
    WHERE
      TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}') AND
      _experience.target.activities IS NOT NULL
  )
  WHERE Activities.activityID = {activity_id}
)
GROUP BY Day, Activities.activityID, ExperienceID
ORDER BY Day DESC, Instances DESC
LIMIT 20
```

### 特定の日のエクスペリエンス ID、セグメント ID、イベント範囲の訪問者数、訪問数、インプレッション数を特定します

```sql
SELECT
  Day,
  Activities.activityID,
  ExperienceID,
  SegmentID._id,
  SUM(CASE WHEN ActivityEvent.eventScope = 'visitor' THEN 1 END) as Visitors,
  SUM(CASE WHEN ActivityEvent.eventScope = 'visit' THEN 1 END) as Visits,
  SUM(CASE WHEN ActivityEvent.eventScope = 'impression' THEN 1 END) as Impressions
FROM
(
  SELECT
    Day,
    Activities,
    ActivityEvent,
    ActivityEvent._experience.target.activity.activityevent.context.experienceID AS ExperienceID,
    EXPLODE(ActivityEvent.segmentEvents.segmentID) AS SegmentID
  FROM
  (
    SELECT
      Day,
      Activities,
      EXPLODE(Activities.activityEvents) AS ActivityEvent
    FROM
    (
      SELECT
        date_format(from_utc_timestamp(timestamp, 'America/New_York'), 'yyyy-MM-dd') AS Day,
        EXPLODE(_experience.target.activities) AS Activities
      FROM adobe_target_experience_events
      WHERE
        TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}') AND
        _experience.target.activities IS NOT NULL
      LIMIT 1000000
    )
    LIMIT 1000000
  )
  LIMIT 1000000
)
GROUP BY Day, Activities.activityID, ExperienceID, SegmentID._id
ORDER BY Day DESC, Activities.activityID, ExperienceID ASC, SegmentID._id ASC, Visitors DESC
LIMIT 20
```

### 指定した日の mbox 名とレコード数を返す

```sql
SELECT
  _experience.target.mboxname,
  COUNT(timestamp) AS records
FROM
  adobe_target_experience_events
WHERE
  TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
  GROUP BY _experience.target.mboxname ORDER BY records DESC
LIMIT 100
```
