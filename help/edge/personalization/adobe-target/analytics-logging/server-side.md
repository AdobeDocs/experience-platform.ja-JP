---
title: Platform Web SDK での A4T データのサーバー側ログ
description: Experience PlatformWeb SDK を使用して、Adobe Analytics for Target(A4T) のサーバー側ログを有効にする方法について説明します。
seo-title: Server-side logging for A4T data in Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk；プラットフォーム；ログ；
exl-id: 26c25f58-e43c-4147-8595-69ea85af561f
source-git-commit: 38d54b2c793c9dcb1a45ec4acbb9016d1e927d23
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---

# Platform Web SDK での A4T データのサーバー側ログ

Adobe Experience Platform Web SDK を使用すると、Platform Edge ネットワークにAdobe Analytics for Target(A4T) 機能を実装できます。 サーバー側ログが有効な場合、ヒットのステッチプロセスを経ずに、Edge ネットワーク経由で送信されるすべての Analytics ヒットが、サーバー側の Target の詳細で拡張されます。

Analytics のサーバー側ログは、データストリーム設定で Analytics が有効になっている場合に有効になります。

![Analytics データストリーム設定が有効](../assets/enable-analytics-datastream.png)

次の図は、サーバー側分析ログが有効な場合のシステム内でのデータのフローを示しています。

![サーバー側ログのフロー](../assets/analytics-server-side-logging.png)

## 次の手順

このガイドでは、Web SDK の A4T データのサーバー側ログについて説明しました。 詳しくは、 [クライアント側ログ](./client-side.md) を参照してください。
