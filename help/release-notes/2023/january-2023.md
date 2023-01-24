---
title: Adobe Experience Platformリリースノート 2023 年 1 月
description: Adobe Experience Platformの 2023 年 1 月のリリースノート。
source-git-commit: 01c220147312108649e036d93288823df5389235
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 41%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年1月25 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ソース](#sources)

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込むことができ、Platform Services を使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| クラウドストレージソースのサブフォルダーへのユーザーアクセスを許可する | 新しいアカウントを作成する際に、クラウドストレージソースの特定のサブフォルダーへのアクセスを定義できるようになりました。 作成したユーザーは、許可されたサブフォルダーのデータにのみアクセスできます。 この機能は、次のクラウドストレージソースで使用できます。 [Azure Blob ストレージ](../../sources/connectors/cloud-storage/blob.md), [Google Cloud Storage](../../sources/connectors/cloud-storage/google-cloud-storage.md), [Google PubSub](../../sources/connectors/cloud-storage/google-pubsub.md)、および [SFTP](../../sources/connectors/cloud-storage/sftp.md). |
| ベータ版の可用性 [!DNL SugarCRM] | [!DNL SugarCRM] ソースがベータ版で利用できるようになりました。 以下を使用： [!DNL SugarCRM Accounts & Contacts] そして [!DNL SugarCRM Events] ソースからデータを取り込む [!DNL SugarCRM] アカウントからExperience Platformへ。 詳しくは、 [[!DNL SugarCRM] 概要](../../sources/connectors/crm/sugarcrm.md). |