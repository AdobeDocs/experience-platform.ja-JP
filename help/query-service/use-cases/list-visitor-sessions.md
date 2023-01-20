---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；experienceevent クエリ；experienceevent クエリ；ExperienceEvent クエリ；
title: ユーザーのページビューのリスト
description: エクスペリエンスイベントを使用して、指定したユーザーが使用した最新 100 ページのリストを作成するクエリの記述方法を説明します。
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 6%

---

# ユーザーのページビューのリスト

このドキュメントでは、指定したユーザーがページビューをリストするのに必要な SQL の例を示します。 Adobe Experience Platformクエリサービスを使用すると、 [!DNL Experience Events] を使用して、様々なユースケースを取り込むことができます。 エクスペリエンスイベントは、エクスペリエンスデータモデル (XDM)ExperienceEvent クラスで表されます。ユーザーが Web サイトまたはサービスを操作したときに、不変で集計されないシステムのスナップショットを取り込みます。 エクスペリエンスイベントは、時間ドメイン分析にも使用できます。 詳しくは、 [次の手順の節](#next-steps) を含むその他の使用例 [!DNL Experience Events] 訪問者レポートを生成するために使用します。

XDM と [!DNL Experience Events] は [[!DNL XDM System] 概要](../../xdm/home.md). クエリサービスと [!DNL Experience Events]を使用すると、ユーザー間の行動傾向を効果的に追跡できます。 次のドキュメントは、 [!DNL Experience Events].

## 目的

次の例では、指定したユーザーが最近閲覧した 100 ページを一覧表示します。

```sql
SELECT 
timestamp, 
web.webReferrer.type as referrerType, 
web.webReferrer.URL as referrer, 
web.webPageDetails.name as pageName, 
_experience.analytics.event1to100.event1.value as A, 
_experience.analytics.event1to100.event2.value as B, 
_experience.analytics.event1to100.event3.value as C, 
web.webPageDetails.pageviews.value as pageViews
FROM your_analytics_table 
WHERE endUserIds._experience.aaid.id = '457C3510571E5930-69AA721C4CBF9339' 
ORDER BY timestamp 
LIMIT 100;
```

このクエリの結果は、次のように表示されます。

```console
      timestamp       |  referrerType  |                            referrer                                |                 pageName            |  A  |  B  |  C  | pageViews
----------------------+----------------+--------------------------------------------------------------------+-------------------------------------+-----+-----+-----+--------------
2019-11-08 17:15:28.0 | typed_bookmark |                                                                    |                                     |     |     |     |
2019-11-08 17:53:05.0 | social         | http://www.reddit.com                                              | Home                                |     |     |     |          1.0
2019-11-08 17:53:45.0 | typed_bookmark |                                                                    | Kids                                |     |     |     |          1.0
2019-11-08 19:22:34.0 | typed_bookmark |                                                                    |                                     |     |     |     |          
2019-11-08 20:01:12.0 | search_engine  | http://www.google.com/search?ie=UTF-8&q=laundry parkas&cid=sem:115 | Home                                |     |     |     |          1.0 
2019-11-08 20:01:57.0 | typed_bookmark |                                                                    | Kids                                |     |     |     |          1.0
2019-11-08 20:03:36.0 | typed_bookmark |                                                                    | Search Results                      | 1.0 |     |     |          1.0
2019-11-08 20:04:30.0 | typed_bookmark |                                                                    | Product Details: Pemmican Power Bar |     |     |     |          1.0
2019-11-08 20:05:27.0 | typed_bookmark |                                                                    | Shopping Cart: Cart Details         |     |     |     |          1.0
2019-11-08 20:06:07.0 | typed_bookmark |                                                                    | Shopping Cart: Shipping Information |     |     |     |          1.0
2019-11-08 20:07:02.0 | typed_bookmark |                                                                    | Shopping Cart: Billing Information  |     |     | 1.0 |          1.0
2019-11-08 20:07:52.0 | typed_bookmark |                                                                    | Shopping Cart: Order Review         |     |     |     |          1.0
2019-11-08 20:08:45.0 | typed_bookmark |                                                                    | Order Confirmation                  |     |     |     |          1.0
2019-11-08 20:09:24.0 | typed_bookmark |                                                                    | Home                                |     |     |     |          1.0
2019-11-08 20:10:03.0 | typed_bookmark |                                                                    | Editorial Page: Camping Essentials  |     |     |     |          1.0
2019-11-08 20:11:01.0 | typed_bookmark |                                                                    | Account Registration|Form           |     |     |     |          1.0
2019-11-08 20:11:38.0 | typed_bookmark |                                                                    | Seasonal Sale                       |     |     |     |          1.0
2019-11-08 20:12:10.0 | typed_bookmark |                                                                    | Blog: Iris Sagan                    |     |     |     |          1.0
2019-11-08 20:13:09.0 | typed_bookmark |                                                                    | Product Details: UltraTech Socks    |     |     |     |          1.0
2019-11-08 20:14:05.0 | typed_bookmark |                                                                    | Seasonal Sale                       |     |     |     |          1.0
```

## 次の手順 {#next-steps}

このドキュメントでは、 [!DNL Experience Events] 指定したユーザーとしてのページビューをリスト表示します。

その他の訪問者ベースの使用例については、次の使用例を参照してください。

- [ページビュー数別に整理された訪問者のリストを取得します。](./visitors-by-number-of-page-views.md)
- [訪問者のロールアップレポートを表示します。](./roll-up-report-of-a-visitor.md)
- [イベントのトレンドレポートを日別に作成します。](./trended-report-of-events.md)
