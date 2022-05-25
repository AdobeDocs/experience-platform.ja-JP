---
title: Platform Web SDK でのAdobe Analytics for Target(A4T) ログ
description: Experience PlatformWeb SDK を使用して、Adobe Analytics for Target(A4T) データのコレクションを制御する方法について説明します。
seo-title: Adobe Analytics for Target (A4T) Logging in the Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t；ログ；analytics;sdk;web sdk;
exl-id: f1c90ccd-48a9-4668-b2ac-eacd5bec0b91
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 2%

---

# Platform Web SDK でのAdobe Analytics for Target(A4T) のログ

パーソナライゼーションにAdobe Targetを使用する場合、パフォーマンス測定に使用するシステムを選択できます。 各 [Target アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) では、Target レポートとAdobe Analyticsレポートのどちらを選択できます。

Analytics レポートを使用している場合、Adobe Targetは Analytics に次の情報を伝える必要があります。

* 訪問者が入力したAdobe Targetアクティビティ
* どのエクスペリエンスを閲覧したか
* コンバージョンに達したもの

Adobe Experience Platform Web SDK は、 Analytics for Target(A4T) の 2 種類の Analytics ログの使用例をサポートしています。

| ログメソッド | 説明 |
| --- | --- |
| サーバー側分析ログ | Edge ネットワークを通じて送信される Analytics のヒットは、ヒットのステッチプロセスを経ずに、サーバー側の Target の詳細で拡張されます。 |
| クライアント側分析ログ | Target データがクライアント側で返されるので、 [Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html). |

ログの記録方法は、設定したでAdobe Analyticsを有効にしているかどうかによって異なります [datastream](../../../datastreams/overview.md):

![ログメソッドの決定フロー](../assets/analytics-logging.png)

## 次の手順

このドキュメントでは、Web SDK 内の A4T データの様々なログメソッドについて簡単に説明しました。 これらの各メソッドについて詳しくは、次のドキュメントを参照してください。

* [Platform Web SDK での A4T データのサーバー側ログ](./server-side.md)
* [Platform Web SDK での A4T データのクライアント側ログ](./client-side.md)
