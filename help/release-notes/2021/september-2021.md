---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platformの最新のリリースノートです。
exl-id: 96375409-803f-45af-805e-900207d972e4
source-git-commit: b616a0c0d49d980644f82bc3af5995b3b17b4c80
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 50%

---

# Adobe Experience Platform リリースノート

**リリース日：2021年9月29日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [ソース](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ストリーミングデータフローのサポート | [!DNL Amazon Kinesis]、[!DNL Azure Event Hubs]および[!DNL Google PubSub]のストリーミングデータフローを作成する際に、データ準備関数を使用できるようになりました。 詳しくは、[クラウドストレージソース用のストリーミングデータフローの作成](../../sources/tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルを参照してください。 |

[!DNL Data Prep]について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Data Landing Zone] | [[!DNL Flow Service] API](../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md)または[ユーザーインターフェイス](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md)を使用して、[!DNL Data Landing Zone]ソース接続を作成できるようになりました。 [!DNL Data Landing Zone] は、Platformによっ [!DNL Azure Blob] てプロビジョニングされたストレージインターフェイスで、Platformの内外でファイルを取り込み、出力するための、セキュリティで保護されたクラウドベースのファイルストレージ機能にアクセスできます。詳しくは、[[!DNL Data Landing Zone] 概要](../../sources/connectors/cloud-storage/data-landing-zone.md)を参照してください。 |
| [!DNL Snowflake] | [[!DNL Flow Service] API](../../sources/tutorials/api/create/databases/snowflake.md)または[ユーザーインターフェイス](../../sources/tutorials/ui/create/databases/snowflake.md)を使用して[!DNL Snowflake]ソース接続を作成し、[!DNL Snowflake]データベースからPlatformにデータを取り込めるようになりました。 詳しくは、[[!DNL Snowflake] 概要](../../sources/connectors/databases/snowflake.md)を参照してください。 |
| [!DNL SFTP] ソースの機能強化 | [!DNL SFTP]ソース接続を作成する際に、カスタムポート番号を手動で設定できます。 詳しくは、[[!DNL SFTP] 概要](../../sources/connectors/cloud-storage/sftp.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
