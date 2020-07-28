---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Azure Data LakeストレージGen2コネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 4d3899e8a91d15da7e40523a03285f3ccec27191
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 1%

---


# Azure Data LakeストレージGen2コネクタ

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供し、これらのシステムからデータを取得でき [!DNL Azure]ます。

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] (ADLS-Gen2)から [!DNL Azure Data Lake Storage Gen2] (ADLS-Gen2)のデータをバッチに取り込むことができます。

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

## 接続 [!DNL Azure Data Lake Storage Gen2] 先 [!DNL Platform]

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Azure Data Lake Storage Gen2] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してADLS-Gen2コネクタを作成する](../../tutorials/api/create/cloud-storage/adls-gen2.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

## UI の使用

- [UIでADLS-Gen2ソースコネクタを作成する](../../tutorials/ui/create/cloud-storage/adls-gen2.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)