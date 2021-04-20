---
keywords: Experience Platform；ホーム；人気のあるトピック；Google PubSub;google pubsub
solution: Experience Platform
title: Google PubSub Source Connectorの概要
topic: overview
description: APIまたはユーザーインターフェイスを使用してGoogle PubSubをAdobe Experience Platformに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 1%

---


# （ベータ版） [!DNL Google PubSub]コネクタ

>[!NOTE]
>
>[!DNL Google PubSub]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformは、[!DNL AWS]、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーに対してネイティブの接続を提供し、これらのシステムのデータをプラットフォームに取り込み、ダウンストリームのサービスや宛先で使用できます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、プラットフォームにデータを取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。 プラットフォームでは、[!DNL Azure Event Hubs]からデータをリアルタイムで取り込むことができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Google PubSub]をプラットフォームに接続

以下のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Google PubSub]をプラットフォームに接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してGoogle PubSubソース接続を作成する](../../tutorials/api/create/cloud-storage/google-pubsub.md)
- [Flow Service APIを使用してストリーミングデータを収集する](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UIでのGoogle PubSubソース接続の作成](../../tutorials/ui/create/cloud-storage/google-pubsub.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)