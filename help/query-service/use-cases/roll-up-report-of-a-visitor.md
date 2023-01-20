---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；experienceevent クエリ；experienceevent クエリ；ExperienceEvent クエリ；
title: 特定の訪問者のロールアップレポートの表示
description: 次のドキュメントは、Adobe Experience Platform Query Service の Experience Events に関するクエリの例を示しています。
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 1%

---

# 特定の訪問者のロールアップレポートの表示

このドキュメントでは、特定のユーザーの複数の分析プロパティからデータを集計し、そのデータを 1 つのレポートで一緒に確認する SQL の例を提供します。 Adobe Experience Platformクエリサービスを使用すると、 [!DNL Experience Events] を使用して、様々なユースケースを取り込むことができます。 エクスペリエンスイベントは、エクスペリエンスデータモデル (XDM)ExperienceEvent クラスで表されます。ユーザーが Web サイトまたはサービスを操作したときに、不変で集計されないシステムのスナップショットを取り込みます。 エクスペリエンスイベントは、時間ドメイン分析にも使用できます。 詳しくは、 [次の手順の節](#next-steps) を含むその他の使用例 [!DNL Experience Events] 訪問者レポートを生成するために使用します。

XDM と [!DNL Experience Events] は [[!DNL XDM System] 概要](../../xdm/home.md). クエリサービスと [!DNL Experience Events]を使用すると、ユーザー間の行動傾向を効果的に追跡できます。 次のドキュメントは、 [!DNL Experience Events].

## 目的

次の SQL の例は、指定したユーザーの様々な分析値の集計レポートを表示する方法を示しています。

```sql
SELECT 
endUserIds._experience.aaid.id, 
SUM(web.webPageDetails.pageviews.value) as pageViews, 
SUM(_experience.analytics.event1to100.event1.value) as A, 
SUM(_experience.analytics.event1to100.event2.value) as B, 
SUM(_experience.analytics.event1to100.event3.value) as C,
SUM(
    CASE 
    WHEN _experience.analytics.customDimensions.evars.evar1 = 'parkas' 
    THEN 1 
    ELSE 0 
    END) as viewedParkas
FROM your_analytics_table 
WHERE endUserIds._experience.aaid.id = '457C3510571E5930-69AA721C4CBF9339' 
GROUP BY endUserIds._experience.aaid.id
ORDER BY pageViews DESC;
```

クエリ結果は、次の表に表示されます。

```console
               id                 | pageViews |   A   |   B   |   C   | viewedParkas
----------------------------------+-----------+-------+-------+-------+--------------
457C3510571E5930-69AA721C4CBF9339 |     706.0 | 83.0  |  7.0  | 38.0  |          22
```

## 次の手順 {#next-steps}

このドキュメントでは、 [!DNL Experience Events] ：指定したユーザーの analytics 値の集計レポートを表示します。

その他の訪問者ベースの使用例については、次の使用例を参照してください。

- [ページビュー数別に整理された訪問者のリストを取得します。](./visitors-by-number-of-page-views.md)
- [訪問者の以前のセッションのリストを表示します。](./list-visitor-sessions.md)
- [イベントのトレンドレポートを日別に作成します。](./trended-report-of-events.md)
