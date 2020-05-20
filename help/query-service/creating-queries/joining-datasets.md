---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの結合
topic: queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507
workflow-type: tm+mt
source-wordcount: '53'
ht-degree: 3%

---


# データセットの結合

データセットを結合すると、クエリに他のデータセットのデータを含めることができます。 次の例では、カスタムのオペレーティングシステムデータセットを使用して、を `operatingsystemID``operatingsystem` 値にマッピングします。

データセット:
- your_analytics_table
- custom_operating_system_lookup

上位50オペレーティングシステムに関する `SELECT` 文を、ページ表示数別に作成します。

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