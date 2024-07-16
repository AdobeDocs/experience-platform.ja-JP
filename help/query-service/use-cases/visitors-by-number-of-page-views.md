---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;Experienceevent クエリ；Experienceevent クエリ；Experience Event クエリ；
title: ページビュー数別の訪問者のリスト
description: エクスペリエンスイベントを使用してクエリを記述し、ページビュー数で整理された訪問者のリストを取得する方法について説明します。
exl-id: 6e8eed0c-838e-4cd0-ae8c-453114fbf4ea
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 1%

---

# ページビュー数別の訪問者のリスト

このドキュメントでは、ページビュー数で整理された訪問者のリストを取得するために必要な SQL の例を示します。 Adobe Experience Platform クエリサービスを使用すると、[!DNL Experience Events] を使用してさまざまなユースケースをキャプチャするクエリを作成できます。 エクスペリエンスイベントは、エクスペリエンスデータモデル（XDM） ExperienceEvent クラスで表されます。このクラスは、ユーザーが web サイトまたはサービスとやり取りする際に、システムの不変スナップショットと非集計スナップショットをキャプチャします。 エクスペリエンスイベントは、タイムドメイン分析にも使用できます。 訪問者レポートの生成に関するユースケースについて [!DNL Experience Events]、[ 次の手順の節 ](#next-steps) を参照してください。

XDM と [!DNL Experience Events] について詳しくは、[[!DNL XDM System]  概要 ](../../xdm/home.md) を参照してください。 クエリサービスと [!DNL Experience Events] を組み合わせることで、ユーザー間の行動のトレンドを効果的に追跡できます。 次のドキュメントでは、[!DNL Experience Events] を含むクエリの例を示します。

## 目的

次の例では、最も多くページを表示したユーザーの 10 個の ID をリストするレポートを作成します。

```sql
SELECT 
endUserIds._experience.aaid.id, 
SUM(web.webPageDetails.pageviews.value) as pageViews 
FROM your_analytics_table
GROUP BY endUserIds._experience.aaid.id 
ORDER BY pageViews DESC
LIMIT 10;
```

クエリ結果が次のテーブルに表示されます。

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

このドキュメントでは、[!DNL Experience Events] でクエリサービスを使用して、最も多くのページを表示したユーザーをリストする方法をより深く理解しました。

その他の訪問者ベースの使用例については、次の使用例を参照してください。

- [訪問者の以前のセッションをリストします。](./list-visitor-sessions.md)
- [訪問者のロールアップレポートを表示します。](./roll-up-report-of-a-visitor.md)
- [日別のイベントのトレンドレポートを作成します。](./trended-report-of-events.md)
