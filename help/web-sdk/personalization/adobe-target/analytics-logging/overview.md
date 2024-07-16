---
title: Platform Web SDK での Target 用Adobe Analytics（A4T）ログ
description: Experience Platform Web SDK を使用して、Adobe Analytics for Target （A4T）データの収集を制御する方法について説明します。
seo-title: Adobe Analytics for Target (A4T) Logging in the Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t；ログ；analytics;sdk;web sdk;
exl-id: f1c90ccd-48a9-4668-b2ac-eacd5bec0b91
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 1%

---

# Platform Web SDK での Target 用Adobe Analytics（A4T）ログ

Adobe Targetをパーソナライゼーションに使用する場合、パフォーマンス測定に使用するシステムを選択できます。 各 [Target アクティビティ ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=ja) を使用すると、Target レポートとAdobe Analytics レポートのどちらかを選択できます。

Analytics レポートを使用している場合、Adobe Targetは次の情報を Analytics に通知する必要があります。

* 訪問者が入力したAdobe Target アクティビティ
* どのエクスペリエンスを見たか
* どのコンバージョンに到達したか

Adobe Experience Platform Web SDK では、Analytics for Target （A4T）のユースケースに対して、次の 2 種類の Analytics ログをサポートしています。

| ログメソッド | 説明 |
| --- | --- |
| サーバーサイド分析ログ | Edge Networkを介して送信されるすべての Analytics ヒットは、ヒットのステッチプロセスを経ることなく、サーバーサイドで Target の詳細で拡張されます。 |
| クライアントサイド分析ログ | Target データがクライアントサイドで返されるので、[Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html) を使用して手動でデータを補強し、Analytics に送信できます。 |

ログの方式は、設定済みの [datastream](../../../../datastreams/overview.md) でAdobe Analyticsが有効になっているかどうかによって決まります。

![ メソッドの決定フローの記録 ](../assets/analytics-logging.png)

## 次の手順

このドキュメントでは、Web SDK での A4T データの様々なログ取得方法について簡単に説明しました。 これらの各方法について詳しくは、次のドキュメントを参照してください。

* [Platform Web SDK での A4T データのサーバーサイドログ](./server-side.md)
* [Platform Web SDK での A4T データのクライアントサイドログ](./client-side.md)
