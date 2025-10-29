---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;Experienceevent クエリ；Experienceevent クエリ；Experience Event クエリ；
title: ユーザーのページビューのリスト
description: エクスペリエンスイベントを使用して、指定したユーザーが使用した最新の 100 ページのリストを作成するクエリを記述する方法について説明します。
exl-id: d831910d-d3a4-4a5a-b897-b09f0546dab0
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 8%

---

# ユーザーのページビューのリスト

このドキュメントでは、指定したユーザーがページビューをリストするために必要な SQL の例を示します。 Adobe Experience Platform クエリサービスを使用すると、[!DNL Experience Events] を使用してさまざまなユースケースをキャプチャするクエリを作成できます。 エクスペリエンスイベントは、エクスペリエンスデータモデル（XDM） ExperienceEvent クラスで表されます。このクラスは、ユーザーが web サイトまたはサービスとやり取りする際に、システムの不変スナップショットと非集計スナップショットをキャプチャします。 エクスペリエンスイベントは、タイムドメイン分析にも使用できます。 訪問者レポートの生成に関するユースケースについて [、](#next-steps) 次の手順の節 [!DNL Experience Events] を参照してください。

XDM と [!DNL Experience Events] について詳しくは、[[!DNL XDM System]  概要 &#x200B;](../../xdm/home.md) を参照してください。 クエリサービスと [!DNL Experience Events] を組み合わせることで、ユーザー間の行動のトレンドを効果的に追跡できます。 次のドキュメントでは、[!DNL Experience Events] を含むクエリの例を示します。

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

このクエリの結果は、次のようになります。

```console
      timestamp       |  referrerType  |                            referrer                                |                 pageName            |  A  |  B  |  C  | pageViews
|----------------------+----------------+--------------------------------------------------------------------+-------------------------------------+-----+-----+-----+--------------
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

このドキュメントでは、[!DNL Experience Events] でクエリサービスを使用して、ページビューを指定されたユーザーとしてリストする方法をより深く理解しました。

その他の訪問者ベースの使用例については、次の使用例を参照してください。

- [ページビュー数で整理された訪問者のリストを取得します。](./visitors-by-number-of-page-views.md)
- [訪問者のロールアップレポートを表示します。](./roll-up-report-of-a-visitor.md)
- [日別のイベントのトレンドレポートを作成します。](./trended-report-of-events.md)
