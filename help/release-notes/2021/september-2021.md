---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: 96375409-803f-45af-805e-900207d972e4
source-git-commit: 4959b5227f777a2c8cab1317d67795678d1a6eea
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 9 月 29 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [データ取得](#ingestion)
- [[!DNL Data Prep]](#data-prep)
- [ソース](#sources)

## データ取得 {#ingestion}

Adobe Experience Platform のデータ取得は、Platform が様々なソースからデータを取得する複数の方法と、そのデータがデータレイク内でどのように保持されてダウンストリームの Platform サービスで使用されるかを表します。

**新機能**

| 機能 | 説明 |
|------- | -----------|
| バッチ取得を使用したプロファイルレコードのアップサートまたはパッチ適用 | リアルタイム顧客プロファイルで、バッチ取得を使用して個々のプロファイルレコードデータのプロファイル属性を更新できるようになりました。詳しくは、[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)を参照してください。 |

データを Platform に取り込む方法については、[データ取得に関するドキュメント](../../ingestion/home.md)を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ストリーミングデータフローのサポート | [!DNL Amazon Kinesis]、[!DNL Azure Event Hubs]、[!DNL Google PubSub] のストリーミングデータフローを作成する際に、データ準備関数を使用できるようになりました。詳しくは、[クラウドストレージソースのストリーミングデータフローの作成](../../sources/tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)に関するチュートリアルを参照してください。 |

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep]  概要](../../data-prep/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Data Landing Zone] | [[!DNL Flow Service] API](../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md) または[ユーザーインターフェイス](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md)を使用して、[!DNL Data Landing Zone] ソース接続を作成できるようになりました。[!DNL Data Landing Zone] は、Platform によってプロビジョニングされた [!DNL Azure Blob] ストレージインターフェイスで、Platform の内外でファイルを取得し、出力するための、セキュリティで保護されたクラウドベースのファイルストレージ機能にアクセスできます。詳しくは、[[!DNL Data Landing Zone]  の概要](../../sources/connectors/cloud-storage/data-landing-zone.md)を参照してください。 |
| [!DNL Snowflake] | [[!DNL Flow Service] API](../../sources/tutorials/api/create/databases/snowflake.md) または[ユーザーインターフェイス](../../sources/tutorials/ui/create/databases/snowflake.md)を使用して [!DNL Snowflake] ソース接続を作成し、[!DNL Snowflake] データベースから Platform にデータを移行できるようになりました。詳しくは、[[!DNL Snowflake]  の概要](../../sources/connectors/databases/snowflake.md)を参照してください。 |
| [!DNL SFTP] ソースの機能強化 | [!DNL SFTP] ソース接続を作成する際に、カスタムポート番号を手動で設定できます。詳しくは、[[!DNL SFTP]  の概要](../../sources/connectors/cloud-storage/sftp.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
