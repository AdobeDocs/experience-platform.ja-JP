---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 0c507a26f551af1eb17889e8e77a036e3c106240
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 66%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 10 月 27 日**

Adobe Experience Platform の既存の機能のアップデート：

- [ソース](#sources)

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Amazon S3] ソースの機能強化 | これで、 `s3SessionToken` 接続するためのパラメーター [!DNL Amazon S3] 一時的なセキュリティ資格情報を使用して Platform にアカウントします。 このトークンを使用すると、 [!DNL Amazon S3] 信頼できない環境のユーザーに対するリソース。 詳しくは、[[!DNL Amazon S3]  のドキュメント](../../sources/connectors/cloud-storage/s3.md#prerequisites)を参照してください。 |
| [!DNL Generic REST API] （ベータ版） | これで、 [!DNL Generic REST API] ソース接続 [[!DNL Flow Service] API](../../sources/tutorials/api/create/protocols/generic-rest.md) または [ユーザーインターフェイス](../../sources/tutorials/ui/create/protocols/generic-rest.md) を使用して、汎用の REST アプリケーションから Platform にデータを取り込みます。 詳しくは、[[!DNL Generic REST API]  の概要](../../sources/connectors/protocols/generic-rest.md)を参照してください。 |
| [!DNL Zoho CRM] （ベータ版） | これで、 [!DNL Zoho CRM] ソース接続 [[!DNL Flow Service] API](../../sources/tutorials/api/create/crm/zoho.md) または [ユーザーインターフェイス](../../sources/tutorials/ui/create/crm/zoho.md) データを取り込む [!DNL Zoho CRM] アカウントを Platform に送信します。 詳しくは、[[!DNL Zoho CRM]  の概要](../../sources/connectors/crm/zoho.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
