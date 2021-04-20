---
keywords: Experience Platform；ホーム；人気のあるトピック；AmazonKinesis;amazon kinesis;Kinesis;kinesis
solution: Experience Platform
title: AmazonKinesisソースコネクタの概要
topic: overview
description: APIまたはユーザーインターフェイスを使用して、AmazonKinesisをAdobe Experience Platformに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 1%

---


# （ベータ版） [!DNL Amazon Kinesis]コネクタ

>[!NOTE]
>
>[!DNL Amazon Kinesis]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーに対してネイティブの接続を提供します。 これらのシステムのデータを[!DNL Platform]に取り込むことができます。

Cloudストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを[!DNL Platform]に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] データをリアルタイムで取り込むこ [!DNL Amazon Kinesis] とができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Amazon Kinesis]を[!DNL Platform]に接続

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Amazon Kinesis]を[!DNL Platform]に接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してAmazonKinesisのソース接続を作成する](../../tutorials/api/create/cloud-storage/kinesis.md)
- [Flow Service APIを使用してストリーミングデータを収集する](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UIでのAmazonKinesisソース接続の作成](../../tutorials/ui/create/cloud-storage/kinesis.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)