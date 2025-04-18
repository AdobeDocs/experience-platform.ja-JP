---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;Experienceevent クエリ；Experienceevent クエリ；Experience Event クエリ；
title: イベントのトレンドレポートの作成
description: エクスペリエンスイベントを使用するクエリを記述して、指定した日付範囲のイベントのトレンドレポートを日付別にグループ化して作成する方法を説明します。
exl-id: 8f7ed5b5-c265-4a1e-a360-4293d1e86e97
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 6%

---

# イベントのトレンドレポートの作成

このドキュメントでは、特定の日付範囲での日別イベントのトレンドレポートを作成するために必要な SQL の例を示します。 Adobe Experience Platform クエリサービスを使用すると、[!DNL Experience Events] を使用してさまざまなユースケースをキャプチャするクエリを作成できます。 エクスペリエンスイベントは、エクスペリエンスデータモデル（XDM） ExperienceEvent クラスで表されます。このクラスは、ユーザーが web サイトまたはサービスとやり取りする際に、システムの不変スナップショットと非集計スナップショットをキャプチャします。 エクスペリエンスイベントは、タイムドメイン分析にも使用できます。 訪問者レポートの生成に関するユースケースについて [!DNL Experience Events]、[ 次の手順の節 ](#next-steps) を参照してください。

レポートを使用すると、Experience Platform データにアクセスして、組織の戦略的なビジネスインサイトに役立てることができます。 これらのレポートを使用すると、Experience Platform データを様々な方法で調べ、主要な指標をわかりやすい形式で表示し、結果として得られるインサイトを共有できます。

XDM と [!DNL Experience Events] について詳しくは、[[!DNL XDM System]  概要 ](../../xdm/home.md) を参照してください。 クエリサービスと [!DNL Experience Events] を組み合わせることで、ユーザー間の行動のトレンドを効果的に追跡できます。 次のドキュメントでは、[!DNL Experience Events] を含むクエリの例を示します。

## 目標

次の例では、指定した日付範囲のイベントのトレンドレポートを日付別にグループ化して作成します。特に、この SQL の例では、`A`、`B`、`C` などの様々な Analytics 値を合計し、1 か月におけるパークの閲覧回数を合計します。

データセットで見つかっ [!DNL Experience Event] タイムスタンプ列は UTC 形式です。 この例では、`from_utc_timestamp()` 関数を使用してタイムスタンプを UTC から EDT に変換し、`date_format()` 関数を使用して日付を残りのタイムスタンプから分離します。

```sql
SELECT 
date_format( from_utc_timestamp(timestamp, 'EDT') , 'yyyy-MM-dd') as Day,
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
WHERE TIMESTAMP >= to_timestamp('2019-03-01') AND TIMESTAMP <= to_timestamp('2019-03-31')
GROUP BY Day 
ORDER BY Day ASC, pageViews DESC;
```

このクエリの結果は、次のようになります。

```console
     Day     | pageViews |   A    |   B   |    C    | viewedParkas
-------------+-----------+--------+-------+---------+--------------
 2019-03-01  |   55317.0 | 8503.0 | 804.0 | 1578.0  |           73
 2019-03-02  |   55302.0 | 8600.0 | 854.0 | 1528.0  |           86
 2019-03-03  |   54613.0 | 8162.0 | 795.0 | 1568.0  |          100
 2019-03-04  |   54501.0 | 8479.0 | 832.0 | 1509.0  |          100
 2019-03-05  |   54941.0 | 8603.0 | 816.0 | 1514.0  |           73
 2019-03-06  |   54817.0 | 8434.0 | 855.0 | 1538.0  |           76
 2019-03-07  |   55201.0 | 8604.0 | 843.0 | 1517.0  |           64
 2019-03-08  |   55020.0 | 8490.0 | 849.0 | 1536.0  |           99
 2019-03-09  |   43186.0 | 6736.0 | 643.0 | 1150.0  |           52
 2019-03-10  |   48471.0 | 7542.0 | 772.0 | 1272.0  |           70
 2019-03-11  |   56307.0 | 8721.0 | 818.0 | 1571.0  |           81
 2019-03-12  |   55374.0 | 8653.0 | 843.0 | 1501.0  |           59
 2019-03-13  |   55046.0 | 8509.0 | 887.0 | 1556.0  |           65
 2019-03-14  |   55518.0 | 8551.0 | 848.0 | 1516.0  |           77
 2019-03-15  |   55329.0 | 8575.0 | 818.0 | 1607.0  |           96
 2019-03-16  |   55030.0 | 8651.0 | 815.0 | 1542.0  |           66
 2019-03-17  |   55143.0 | 8435.0 | 774.0 | 1572.0  |           65
 2019-03-18  |   54065.0 | 8211.0 | 816.0 | 1574.0  |          111
 2019-03-19  |   55097.0 | 8395.0 | 771.0 | 1498.0  |           86
 2019-03-20  |   55198.0 | 8472.0 | 863.0 | 1583.0  |           82
 2019-03-21  |   54978.0 | 8490.0 | 820.0 | 1580.0  |           83
 2019-03-22  |   55464.0 | 8561.0 | 820.0 | 1559.0  |           83
 2019-03-23  |   55384.0 | 8482.0 | 800.0 | 1139.0  |           82
 2019-03-24  |   55295.0 | 8594.0 | 841.0 | 1382.0  |           78
 2019-03-25  |   42069.0 | 6365.0 | 606.0 | 1509.0  |           62
 2019-03-26  |   49724.0 | 7629.0 | 724.0 | 1553.0  |           44
 2019-03-27  |   55111.0 | 8524.0 | 804.0 | 1524.0  |           94
 2019-03-28  |   55030.0 | 8439.0 | 822.0 | 1554.0  |           73
 2019-03-29  |   55281.0 | 8601.0 | 854.0 | 1580.0  |           73
 2019-03-30  |   55162.0 | 8538.0 | 846.0 | 1534.0  |           79
 2019-03-31  |   55437.0 | 8486.0 | 807.0 | 1649.0  |           68
 (31 rows)
```

## 次の手順 {#next-steps}

このドキュメントを参照することで、[!DNL Experience Events] でクエリサービスを使用してユーザー間の行動のトレンドを効果的に追跡する方法をより深く理解できます。

[!DNL Experience Events] を使用するその他の訪問者ベースのユースケースについては、次のドキュメントを参照してください。

- [ページビュー数で整理された訪問者のリストを取得します。](./visitors-by-number-of-page-views.md)
- [訪問者の以前のセッションをリストします。](./list-visitor-sessions.md)
- [訪問者のロールアップレポートを表示します。](./roll-up-report-of-a-visitor.md)
