---
title: Snowflakeの概要
description: Adobe Experience Platformでのイベント転送のSnowflakeについて説明します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: abddf0c4-3d4c-4f66-a6e0-c10b54ea3430
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 2%

---

# [!DNL Snowflake] の概要

[[!DNL Snowflake]](https://www.snowflake.com/en/) は、すべてのデータレコードを 1 か所に保存および分析できるクラウドデータウェアハウスです。 データウェアハウス、データレイク、データエンジニアリング、データサイエンス、データアプリケーション開発、リアルタイムまたは共有データの安全な共有と消費に使用できます。

[!DNL Snowflake] は、Amazon Web Services(AWS)、Microsoft Azure(Azure) およびGoogle Cloud Platform(GCP) をベースに構築されています。

![以下を示す図： [!DNL Snowflake] データアーキテクチャ。](../../../images/extensions/server/snowflake/snowflake.png)

## アドビのSnowflake統合

Snowflakeアカウントは、次のクラウドプラットフォームのいずれかでホストできます。

- [Amazon Web Services ([!DNL AWS])](https://aws.amazon.com/) - [!DNL AWS] は、顧客関係管理 (CRM) およびエンタープライズリソース計画 (ERP) 向けの、分散コンピューティング、データベースストレージ、コンテンツ配信、SaaS(Software-as-a-S) 統合サービスなど、幅広いサービスを提供するクラウドコンピューティングプラットフォームです。

詳しくは、 [[!DNL AWS] 概要](../aws/overview.md) 拡張機能のインストールとイベント転送ルールの設定について詳しくは、を参照してください。

- [Microsoft Azure ([!DNL Azure])](https://azure.microsoft.com/en-us/products/event-hubs/#overview) - [!DNL Azure] は、Microsoftが提供するクラウドサービスとリソースにアクセスして管理できるクラウドコンピューティングプラットフォームおよびオンラインポータルです。

詳しくは、 [[!DNL Azure] 概要](../azure/overview.md) 拡張機能のインストールとイベント転送ルールの設定について詳しくは、を参照してください。

- [[!DNL Google Cloud Platform]](https://cloud.google.com/) - [!DNL Google Cloud Platform] は、顧客関係管理 (CRM) およびエンタープライズリソース計画 (ERP) 向けの、分散コンピューティング、データベースストレージ、コンテンツ配信、SaaS(Software-as-a-S) 統合サービスなど、幅広いサービスを提供するクラウドコンピューティングプラットフォームです。

詳しくは、 [[!DNL Google Cloud Platform] 概要](../google-cloud-platform/overview.md) 拡張機能のインストールとイベント転送ルールの設定について詳しくは、を参照してください。

アドビのネイティブ [[!DNL AWS]](../aws/overview.md), [[!DNL Azure]](../azure/overview.md)、および [[!DNL Google Cloud Platform]](../google-cloud-platform/overview.md) イベント転送拡張機能を使用すると、ユーザーは、イベントデータをサーバーサイドでリアルタイムに収集、エンリッチメント、転送して、 [!DNL Snowflake]. 次を参照してください。

![The [!DNL Snowflake] 次の間のリンクを示すレポート図 [!DNL AWS] および [!DNL Azure].](../../../images/extensions/server/snowflake/snowflake-workflow.png)

## 次の手順

このガイドでは、 [!DNL Snowflake] そして、 [!DNL AWS] および [!DNL Azure] イベント転送拡張機能。

詳しくは、 [!DNL AWS] および [!DNL Azure] イベント転送機能 (Experience Platform: [[!DNL AWS] 拡張機能の概要](../aws/overview.md) そして [[!DNL Azure] 拡張機能の概要](../azure/overview.md).
