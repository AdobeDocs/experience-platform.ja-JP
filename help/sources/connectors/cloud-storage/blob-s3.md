---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Azure BlobとAmazonS3コネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 340f5d0611e9e9eb4676018ee10c8a8aa08dbb2d
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 5%

---


# Azure BlobとAmazonS3コネクタ

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供 [!DNL Azure]します。 これらのシステムのデータをに取り込むことができ [!DNL Platform]ます。

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] とS3のデータをバッチ [!DNL Azure Blob] で取り込むことができます。

## IPアドレス許可リスト

ソースコネクタを使用する前に、許可リストに次のIPアドレスを追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。

### 米国東部地域

- `20.41.2.0/23`
- `20.41.4.0/26`
- `20.44.17.80/28`
- `20.49.102.16/29`
- `40.70.148.160/28`
- `52.167.107.224/28`

### 西ヨーロッパ地域

- `13.69.67.192/28`
- `13.69.107.112/28`
- `13.69.112.128/28`
- `40.74.24.192/26`
- `40.74.26.0/23`
- `40.113.176.232/29`
- `52.236.187.112/28`

### オーストラリア東部

- `13.70.74.144/28`
- `20.37.193.0/25`
- `20.37.193.128/26`
- `20.37.198.224/29`
- `40.79.163.80/28`
- `40.79.171.160/28`

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してAzure BlobとS3をPlatformに接続する方法に関する情報を提供しています。

## API [!DNL Azure Blob] を使用してS3を接続 [!DNL Platform] する

- [Flow Service APIを使用してAzure BLOBコネクタを作成する](../../tutorials/api/create/cloud-storage/blob.md)
- [Flow Service APIを使用してS3コネクタを作成する](../../tutorials/api/create/cloud-storage/s3.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

## UI [!DNL Blob] を使用してS3 [!DNL Platform] に接続

- [UI での Azure Blob または Amazon S3 ソースコネクタの作成](../../tutorials/ui/create/cloud-storage/blob-s3.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)