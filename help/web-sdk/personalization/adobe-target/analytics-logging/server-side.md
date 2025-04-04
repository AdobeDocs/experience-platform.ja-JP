---
title: Experience Platform Web SDKでの A4T データのサーバーサイドログ
description: Experience Platform web SDKを使用して、Adobe Analytics for Target （A4T）のサーバーサイドログを有効にする方法を説明します。
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk;platform；ログ；
exl-id: 26c25f58-e43c-4147-8595-69ea85af561f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# Experience Platform Web SDKでの A4T データのサーバーサイドログ

Adobe Experience Platform web SDKを使用すると、Experience Platform Edge NetworkにAdobe Analytics for Target （A4T）機能を実装できます。 サーバーサイドログが有効な場合、Edge Networkを介して送信されるすべての Analytics ヒットが、ヒットのステッチプロセスを経ることなく、サーバーサイドの Target の詳細で拡張されます。

Analytics のサーバーサイドログは、データストリーム設定で Analytics が有効になっている場合に有効になります。

![Analytics データストリーム設定が有効 ](../assets/enable-analytics-datastream.png)

次の図は、サーバーサイド Analytics ログが有効な場合のシステム内でのデータのフローを示しています。

![ サーバーサイドログフロー ](../assets/analytics-server-side-logging.png)

## 次の手順

このガイドでは、Web SDKでの A4T データのサーバーサイドログについて説明しました。 クライアントサイドでの A4T データの処理方法について詳しくは、[ クライアントサイドログ ](./client-side.md) に関するガイドを参照してください。
