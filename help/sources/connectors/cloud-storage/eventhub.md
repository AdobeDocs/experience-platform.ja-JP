---
keywords: Experience Platform；ホーム；人気の高いトピック；Azureイベントハブ；Azureイベントハブ；イベントハブ；イベントハブ
solution: Experience Platform
title: Azureイベントハブソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してAzureイベントハブをAdobe Experience Platformに接続する方法を説明します。
exl-id: b4d4bc7f-2241-482d-a5c2-4422c31705bf
translation-type: tm+mt
source-git-commit: 6dac267be93241bffb4eb5092a6e8da5093c63a6
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 1%

---


# [!DNL Azure Event Hubs] コネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーに対してネイティブの接続を提供します。 これらのシステムのデータを[!DNL Platform]に取り込むことができます。

Cloudストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを[!DNL Platform]に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] データをリアルタイムで取り込むこ [!DNL Azure Event Hubs] とができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Azure Event Hubs]を[!DNL Platform]に接続

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Azure Event Hubs]を[!DNL Platform]に接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してAzureイベントハブのソース接続を作成する](../../tutorials/api/create/cloud-storage/eventhub.md)
- [Flow Service APIを使用してストリーミングデータを収集する](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UIにAzureイベントハブのソース接続を作成する](../../tutorials/ui/create/cloud-storage/eventhub.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
