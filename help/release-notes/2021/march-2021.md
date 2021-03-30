---
title: Adobe Experience Platform リリースノート
description: 2021年3月31日Experience Platformリリースノート
doc-type: release notes
last-update: March 31, 2021
author: ens72741
translation-type: tm+mt
source-git-commit: 8d4270d9168a570fcf92ba60d70dbc8e9af98136
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 50%

---


# Adobe Experience Platform リリースノート

**リリース日：2021年3月31日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Sources]](#sources)

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform には、様々なデータプロバイダーへのソース接続を簡単に設定できる RESTful API とインタラクティブ UI が用意されています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

Experience Platformの2021年3月リリースには、次の情報源の更新が含まれています。

| 機能 | 説明 |
| ------- | ----------- |
| ベータ版ソースをGAに移行 | 以下の資料はベータ版からGA版に進められている。 <ul><li>[[!DNL MySQL]](../../sources/connectors/databases/mysql.md)</li><li>[[!DNL PostGres]](../../sources/connectors/databases/postgres.md)</li><li>[[!DNL Salesforce Service Cloud]](../../sources/connectors/customer-success/salesforce-service-cloud.md)</li><li>[[!DNL SFTP]](../../sources/connectors/cloud-storage/sftp.md)</li><li>[[!DNL Shopify]](../../sources/connectors/ecommerce/shopify.md)</li></ul> |
| 圧縮ファイル取り込みのためのAPIのサポート | クラウドストレージソースを使用して、圧縮されたJSONまたは区切り形式のファイルをプレビューおよび取り込めるようになりました。 詳しくは、[API](../../sources/tutorials/api/collect/cloud-storage.md)を使用したクラウドストレージデータの収集に関するチュートリアルを参照してください。 |
| 再帰的なファイルのアップロードに対するUIのサポート | クラウドストレージソースを使用する場合に、フォルダー全体を再帰的に取り込めるようになりました。 フォルダー全体を取り込む場合は、そのコンテンツが同じスキーマーを共有していることを確認する必要があります。 詳しくは、UI](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md)で、[クラウドストレージコネクタ用のデータフローの設定に関するチュートリアルを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
