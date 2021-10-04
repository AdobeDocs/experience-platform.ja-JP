---
keywords: Experience Platform；ホーム；人気のあるトピック；Google PubSub;google pubsub
solution: Experience Platform
title: Google PubSub Source Connector の概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Google PubSub をAdobe Experience Platformに接続する方法を説明します。
exl-id: 7c78173d-2639-47cb-8935-77fb7841a121
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 2%

---

# （ベータ版） [!DNL Google PubSub] コネクタ

>[!NOTE]
>
>[!DNL Google PubSub] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformは、[!DNL AWS]、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを Platform に取り込んで、ダウンストリームのサービスや宛先で使用できます。

クラウドストレージソースを使用すると、ダウンロード、形式設定、アップロードをおこなう必要なく、データを Platform に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で指定できます。 プロセスの各ステップは、ソースワークフローに統合されます。 Platform では、[!DNL Azure Event Hubs] からデータをリアルタイムで取り込むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## [!DNL Google PubSub] を Platform に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Google PubSub] を Platform に接続する方法について説明します。

### API の使用

- [フローサービス API を使用した Google PubSub ソース接続の作成](../../tutorials/api/create/cloud-storage/google-pubsub.md)
- [フローサービス API を使用したストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI での Google PubSub ソース接続の作成](../../tutorials/ui/create/cloud-storage/google-pubsub.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
