---
keywords: Experience Platform;home;popular topics;Azure Event Hubs;azure event hubs;Event Hubs;event hubs
solution: Experience Platform
title: Azureイベントハブコネクタ
topic: overview
description: 以下のドキュメントは、APIまたはユーザーインターフェイスを使用してAzureイベントハブをプラットフォームに接続する方法に関する情報を提供しています。
translation-type: tm+mt
source-git-commit: c0c609e3f385665cf88129def0c69e7d153ce201
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 1%

---


# （ベータ版）Azureイベントハブコネクタ

>[!NOTE]
>
>Azureイベントハブコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供 [!DNL Azure]します。 これらのシステムのデータをに取り込むことができ [!DNL Platform]ます。

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] データをリアルタイムで取り込むこ [!DNL Azure Event Hubs] とができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、「 [IPアドレスの許可リスト](../../ip-address-allow-list.md) 」ページを参照してください。

## 接続 [!DNL Azure Event Hubs] 先 [!DNL Platform]

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Azure Event Hubs] を [!DNL Platform] 使用して接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してAzureイベントハブコネクタを作成する](../../tutorials/api/create/cloud-storage/eventhub.md)
- [Flow Service APIを使用してストリーミングデータを収集する](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UIにAzureイベントハブのソースコネクタを作成する](../../tutorials/ui/create/cloud-storage/eventhub.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)