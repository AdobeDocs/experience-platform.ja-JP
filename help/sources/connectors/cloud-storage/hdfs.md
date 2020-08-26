---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: HDFSコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 690ddbd92f0a2e4e06b988e761dabff399cd2367
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---


# （ベータ版） [!DNL Apache] HDFSコネクタ

>[!NOTE]
>
>Apache HDFSコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供し、これらのシステムからデータを取得でき [!DNL Azure]ます。 取り込まれたデータは、JSON、パーケー、区切り形式で形式設定できます。 クラウドストレージプロバイダーのサポートには、 [!DNL Apache] HDFSが含まれます。

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

次のドキュメントは、APIまたはユーザーインターフェイスを [!DNL Apache][!DNL Platform] 使用してHDFSを接続する方法に関する情報を提供しています。

## APIを [!DNL Apache][!DNL Platform] 使用したHDFSへの接続

- [Flow Service APIを使用してHDFSコネクタを作成する](../../tutorials/api/create/cloud-storage/hdfs.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

## UIを使用してHDFSを [!DNL Apache] 接続 [!DNL Platform] する

- [UIでApache HDFSソースコネクタを作成する](../../tutorials/ui/create/cloud-storage/hdfs.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)