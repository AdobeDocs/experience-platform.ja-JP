---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;Experienceevent クエリ；Experienceevent クエリ；Experience Event クエリ；
title: 特定の訪問者のロールアップレポートの表示
description: 次のドキュメントでは、Adobe Experience Platform クエリサービスのエクスペリエンスイベントに関連するクエリの例を示します。
exl-id: 1348503f-65c1-41f9-b111-1284a49449a1
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 1%

---

# 特定の訪問者のロールアップレポートの表示

このドキュメントでは、特定のユーザーに対して複数の Analytics プロパティからデータを集計し、そのデータを 1 つのレポートに表示する SQL の例を示します。 Adobe Experience Platform クエリサービスを使用すると、[!DNL Experience Events] を使用してさまざまなユースケースをキャプチャするクエリを作成できます。 エクスペリエンスイベントは、エクスペリエンスデータモデル（XDM） ExperienceEvent クラスで表されます。このクラスは、ユーザーが web サイトまたはサービスとやり取りする際に、システムの不変スナップショットと非集計スナップショットをキャプチャします。 エクスペリエンスイベントは、タイムドメイン分析にも使用できます。 訪問者レポートの生成に関するユースケースについて [、](#next-steps) 次の手順の節 [!DNL Experience Events] を参照してください。

XDM と [!DNL Experience Events] について詳しくは、[[!DNL XDM System]  概要 ](../../xdm/home.md) を参照してください。 クエリサービスと [!DNL Experience Events] を組み合わせることで、ユーザー間の行動のトレンドを効果的に追跡できます。 次のドキュメントでは、[!DNL Experience Events] を含むクエリの例を示します。

## 目的

次の SQL の例では、指定したユーザーの様々な分析値の集計レポートを表示する方法を示しています。

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

クエリ結果が次のテーブルに表示されます。

```console
               id                 | pageViews |   A   |   B   |   C   | viewedParkas
|----------------------------------+-----------+-------+-------+-------+--------------
457C3510571E5930-69AA721C4CBF9339 |     706.0 | 83.0  |  7.0  | 38.0  |          22
```

## 次の手順 {#next-steps}

このドキュメントでは、[!DNL Experience Events] でクエリサービスを使用して、指定されたユーザーの分析値の集計レポートを表示する方法をより深く理解しました。

その他の訪問者ベースの使用例については、次の使用例を参照してください。

- [ページビュー数で整理された訪問者のリストを取得します。](./visitors-by-number-of-page-views.md)
- [訪問者の以前のセッションをリストします。](./list-visitor-sessions.md)
- [日別のイベントのトレンドレポートを作成します。](./trended-report-of-events.md)
