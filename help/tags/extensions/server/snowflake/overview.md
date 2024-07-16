---
title: Snowflakeの概要
description: Adobe Experience Platformでのイベント転送のSnowflakeについて説明します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: abddf0c4-3d4c-4f66-a6e0-c10b54ea3430
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 1%

---

# [!DNL Snowflake] の概要

[[!DNL Snowflake]](https://www.snowflake.com/en/) は、すべてのデータレコードを 1 か所に保存して分析できるクラウドデータウェアハウスです。 データウェアハウス、データレイク、データエンジニアリング、データサイエンス、データアプリケーション開発、リアルタイムまたは共有データの安全な共有と消費に使用できます。

[!DNL Snowflake] は、Amazon Web Services（AWS）、Microsoft Azure （Azure）、Google Cloud Platform （GCP）をベースに構築されています。

![[!DNL Snowflake] のデータアーキテクチャを示す図。](../../../images/extensions/server/snowflake/snowflake.png)

## Snowflakeの統合

Snowflakeアカウントは、次のいずれかのクラウドプラットフォームでホストできます。

- [Amazon Web Services（[!DNL AWS]） ](https://aws.amazon.com/jp/) - [!DNL AWS] は、分散コンピューティング、データベースストレージ、コンテンツ配信、カスタマー関係管理（CRM）およびエンタープライズリソースプランニング（ERP）のための SaaS （software-as-a-service）統合サービスなど、幅広いサービスを提供するクラウドコンピューティングプラットフォームです。

拡張機能のインストールとイベント転送ルールの設定について詳しくは、[[!DNL AWS]  概要 ](../aws/overview.md) を参照してください。

- [Microsoft Azure （[!DNL Azure]） ](https://azure.microsoft.com/en-us/products/event-hubs/#overview) - [!DNL Azure] は、Microsoftが提供するクラウドサービスおよびリソースにアクセスして管理できる、クラウドコンピューティングプラットフォームおよびオンラインポータルです。

拡張機能のインストールとイベント転送ルールの設定について詳しくは、[[!DNL Azure]  概要 ](../azure/overview.md) を参照してください。

- [[!DNL Google Cloud Platform]](https://cloud.google.com/) - [!DNL Google Cloud Platform] はクラウドコンピューティングプラットフォームであり、分散コンピューティング、データベースストレージ、コンテンツ配信、顧客関係管理（CRM）およびエンタープライズリソースプランニング（ERP）のためのサービスである SaaS （software-as-a-service）統合サービスなど、幅広いサービスを提供します。

拡張機能のインストールとイベント転送ルールの設定について詳しくは、[[!DNL Google Cloud Platform]  概要 ](../google-cloud-platform/overview.md) を参照してください。

ネイティブの [[!DNL AWS]](../aws/overview.md)、[[!DNL Azure]](../azure/overview.md) および [[!DNL Google Cloud Platform]](../google-cloud-platform/overview.md) イベント転送拡張機能を使用すると、お客様は、イベントデータをサーバーサイドでリアルタイムに収集、強化し、[!DNL Snowflake] で使用するクラウドサービスに転送できます。 次を参照してください。

![[!DNL AWS] と [!DNL Azure] の間のリンクを示す [!DNL Snowflake] レポート図 ](../../../images/extensions/server/snowflake/snowflake-workflow.png)

## 次の手順

このガイドでは、[!DNL Snowflake] の概要と [!DNL AWS] および [!DNL Azure] イベント転送拡張機能の使用方法を説明しました。

Extension の [!DNL AWS] および [!DNL Azure] イベント転送機能について詳しくは、[[!DNL AWS] Experience Platformの概要 ](../aws/overview.md) および [[!DNL Azure]  拡張機能の概要 ](../azure/overview.md) を参照してください。
