---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Event Hubs;Azure Event Hubs;Event Hubs；イベントハブ
solution: Experience Platform
title: Azure Event Hubs ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Azure Event Hubs をAdobe Experience Platformに接続する方法を説明します。
exl-id: b4d4bc7f-2241-482d-a5c2-4422c31705bf
source-git-commit: 6dac267be93241bffb4eb5092a6e8da5093c63a6
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 2%

---


# [!DNL Azure Event Hubs] コネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーにネイティブ接続を提供します。 これらのシステムのデータを [!DNL Platform] に取り込むことができます。

クラウドストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを [!DNL Platform] に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で指定できます。 プロセスの各ステップは、ソースワークフローに統合されます。 [!DNL Platform] を使用すると、からデータをリアルタイム [!DNL Azure Event Hubs] で取り込むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## [!DNL Azure Event Hubs] を [!DNL Platform] に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Azure Event Hubs] を [!DNL Platform] に接続する方法について説明します。

### API の使用

- [フローサービス API を使用した Azure Event Hubs ソース接続の作成](../../tutorials/api/create/cloud-storage/eventhub.md)
- [フローサービス API を使用したストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI での Azure Event Hubs ソース接続の作成](../../tutorials/ui/create/cloud-storage/eventhub.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
