---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；experienceevent クエリ；experienceevent クエリ；ExperienceEvent クエリ；
title: 訪問者をページビュー数別にリスト表示
description: エクスペリエンスイベントを使用して、ページビュー数別に整理された訪問者のリストを取得するクエリを記述する方法について説明します。
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---

# 訪問者をページビュー数別にリスト表示

このドキュメントでは、ページビュー数別に整理された訪問者のリストを取得するために必要な SQL の例を示します。 Adobe Experience Platformクエリサービスを使用すると、 [!DNL Experience Events] を使用して、様々なユースケースを取り込むことができます。 エクスペリエンスイベントは、エクスペリエンスデータモデル (XDM)ExperienceEvent クラスで表されます。ユーザーが Web サイトまたはサービスを操作したときに、不変で集計されないシステムのスナップショットを取り込みます。 エクスペリエンスイベントは、時間ドメイン分析にも使用できます。 詳しくは、 [次の手順の節](#next-steps) を含むその他の使用例 [!DNL Experience Events] 訪問者レポートを生成するために使用します。

XDM と [!DNL Experience Events] は [[!DNL XDM System] 概要](../../xdm/home.md). クエリサービスと [!DNL Experience Events]を使用すると、ユーザー間の行動傾向を効果的に追跡できます。 次のドキュメントは、 [!DNL Experience Events].

## 目的

次の例では、最も多くのページを閲覧したユーザーの 10 個の ID をリストするレポートを作成します。

```sql
SELECT 
endUserIds._experience.aaid.id, 
SUM(web.webPageDetails.pageviews.value) as pageViews 
FROM your_analytics_table
GROUP BY endUserIds._experience.aaid.id 
ORDER BY pageViews DESC
LIMIT 10;
```

クエリ結果は、次の表に表示されます。

```console
               id                  | pageViews
-----------------------------------+-----------
 457C3510571E5930-69AA721C4CBF9339 |     706.0
 776F85658792C017-6491FE6570382A01 |     700.0
 6BEC9C6AB52E779F-28F5B023113F2C85 |     654.0
 1C0CCFB2DC63611E-6E4A4D4142AEB613 |     642.0
 112EE9A6F3BE29D1-514A6C355A2C9EF6 |     629.0
 CCC75A0E6AC7F2FA-11D58515D370F626 |     624.0
 749F850A44153120-3710C53FA2162349 |     614.0
 2B668C6DDDAF0C505-92EDCC072F7CDDA |     587.0
 7EB7257335935320-101921AF45111FE6 |     586.0
 5F4759CA80DCA9C9-2C0DA93D80D9DBFA |     586.0
(10 rows)
```

## 次の手順 {#next-steps}

このドキュメントでは、 [!DNL Experience Events] をクリックすると、最も多くのページを閲覧したユーザーが一覧表示されます。

その他の訪問者ベースの使用例については、次の使用例を参照してください。

- [訪問者の以前のセッションのリストを表示します。](./list-visitor-sessions.md)
- [訪問者のロールアップレポートを表示します。](./roll-up-report-of-a-visitor.md)
- [イベントのトレンドレポートを日別に作成します。](./trended-report-of-events.md)
