---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの参加
topic: queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# データセットの参加

データセットを結合すると、他のデータセットのデータをデータに含めることができます。クエリ この例では、カスタムのオペレーティングシステムデータセットを使用して、を値にマ `operatingsystemID` ッピングして `operatingsystem` います。

データセット:
- your_analytics_table
- custom_operating_system_lookup

上位50のオペ `SELECT` レーティングシステムに関する文をページ表示数別に作成

```sql
SELECT 
  b.operatingsystem AS OperatingSystem,
  SUM(a.web.webPageDetails.pageviews.value) AS PageViews
FROM your_analytics_table a 
     JOIN custom_operating_system_lookup b 
      ON a._experience.analytics.environment.operatingsystemID = b.operatingsystemid 
WHERE _ACP_YEAR=2018 
GROUP BY OperatingSystem 
ORDER BY PageViews DESC
LIMIT 50;
```

![画像](../images/queries/joining-datasets/select-operating-systems.png)