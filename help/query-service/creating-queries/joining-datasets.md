---
keywords: Experience Platform;home;popular topics;query service;Query service;joining datasets;joining dataset;
solution: Experience Platform
title: データセットの結合
topic: queries
translation-type: tm+mt
source-git-commit: c5d3be4706ca6d6a30e203067db6ddc894b9bfb4
workflow-type: tm+mt
source-wordcount: '53'
ht-degree: 100%

---


# データセットの結合

データセットを結合すると、他のデータセットのデータをクエリに含めることができます。この例では、カスタムのオペレーティングシステムデータセットを使用して、`operatingsystemID` を `operatingsystem` 値にマッピングしています。

データセット：
- your_analytics_table
- custom_operating_system_lookup

上位 50 のオペレーティングシステムに関する `SELECT` 文をページ表示数別に作成します。

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