---
keywords: Experience Platform；ホーム；人気のトピック；Azure Event Hubs;Azure イベントハブ；Event Hubs；イベントハブ
solution: Experience Platform
title: Azure Event Hubs ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Azure Event Hubs をAdobe Experience Platformに接続する方法を説明します。
exl-id: b4d4bc7f-2241-482d-a5c2-4422c31705bf
source-git-commit: 832e32c31be944fff1101fa409e56f5c3e27d325
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 4%

---


# [!DNL Azure Event Hubs] コネクタ

Adobe Experience Platformは、AWSなどのクラウドプロバイダーにネイティブの接続を提供します。 [!DNL Google Cloud Platform]、および [!DNL Azure]. これらのシステムのデータを Platform に取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードをおこなう必要なく、独自のデータを Platform に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順が、ソースワークフローに統合されます。 Platform では、次の場所からデータを取り込むことができます： [!DNL Event Hubs] リアルタイムで。

## 拡大/縮小 [!DNL Event Hubs]

のスケール係数 [!DNL Event Hubs] 大量のデータに入る、並列性を高める、取り込みプラットフォームの速度を上げる必要がある場合は、インスタンスを増やす必要があります。

### 入力の高いボリュームデータ

現在、 [!DNL Event Hubs] Platform へのアカウントは、1 秒あたり 2,000 レコードです。 大量のデータをスケールアップして取り込むには、Adobe担当者にお問い合わせください。

### の並列性の向上 [!DNL Event Hubs] および Platform

並列処理とは、複数の処理単位で同じタスクを同時に実行し、速度とパフォーマンスを向上させることを指します。 並列性を [!DNL Event Hubs] パーティションを増やす、またはより多くの処理単位を取得することで、並んで [!DNL Event Hubs] アカウント 参照 [[!DNL Event Hubs] 拡大・縮小に関する文書](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-scalability) を参照してください。

Platform 側での取り込み速度を上げるには、Platform で、ソースコネクタで読み込むタスクの数を増やす必要があります [!DNL Event Hubs] パーティション。 次に、 [!DNL Event Hubs] 新しいパーティションに基づいてAdobeタスクをスケールするには、Platform 担当者にお問い合わせください。 現在、この処理は自動化されていません。

## 仮想ネットワークを使用して接続する [!DNL Event Hubs] Platform へ

仮想ネットワークを設定して接続できます [!DNL Event Hubs] ファイアウォール対策を有効にしている間に、Platform に接続します。 仮想ネットワークを設定するには、次の手順に従います [[!DNL Event Hubs] ネットワークルールセットドキュメント](https://docs.microsoft.com/en-us/rest/api/eventhub/preview/namespaces-network-rule-set/create-or-update-network-rule-set#code-try-0) 次に、 **所要時間** をクリックします。 次に、 [!DNL Azure] アカウントを使用して資格情報を選択し、 [!DNL Event Hubs] Platform に取り込む名前空間、リソースグループ、サブスクリプションです。

設定が完了したら、 **リクエスト本文** 次のリストから、ご使用のネットワーク地域に対応する JSON を使用できます。

>[!TIP]
>
>この呼び出しの後に削除されるので、既存のファイアウォール IP フィルタリングルールのバックアップを作成する必要があります。

### VA7:北米

```json
{
  "properties": {
    "defaultAction": "Deny",
    "virtualNetworkRules": [
      {
        "subnet": {
          "id": "/subscriptions/93f21779-b1fd-49ee-8547-2cdbc979a44f/resourceGroups/ethos_12_prod_va7_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_va7_network_10_19_144_0_22/subnets/ethos_12_prod_va7_network_10_19_144_0_22"
        },
        "ignoreMissingVnetServiceEndpoint": true
      },
    ],
    "ipRules": []
  }
}
```

### NLD2:ヨーロッパ

```json
{
  "properties": {
    "defaultAction": "Deny",
    "virtualNetworkRules": [
      {
        "subnet": {
          "id": "/subscriptions/40bde086-46ad-44c3-afba-c306f54b64ec/resourceGroups/ethos_12_prod_va7_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_nld2_network_10_20_40_0_23/subnets/ethos_12_prod_nld2_network_10_20_40_0_23"
        },
        "ignoreMissingVnetServiceEndpoint": true
      },
    ],
    "ipRules": []
  }
}
```

### AUS5:オーストラリア

```json
{
  "properties": {
    "defaultAction": "Deny",
    "virtualNetworkRules": [
      {
        "subnet": {
          "id": "/subscriptions/1618ef18-9edc-48bf-88dd-61cc979629b5/resourceGroups/ethos_12_prod_aus5_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_aus5_network_10_21_116_0_22/subnets/ethos_12_prod_aus5_network_10_21_116_0_22"
        },
        "ignoreMissingVnetServiceEndpoint": true
      },
    ],
    "ipRules": []
  }
}
```

次を参照してください。 [[!DNL Event Hubs] 文書](https://docs.microsoft.com/en-us/rest/api/eventhub/preview/namespaces-network-rule-set/create-or-update-network-rule-set) を参照してください。

## 接続 [!DNL Event Hubs] Platform へ

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Event Hubs] API またはユーザーインターフェイスを使用して Platform に接続するには、次の手順を実行します。

### API の使用

- [フローサービス API を使用したイベントハブソース接続の作成](../../tutorials/api/create/cloud-storage/eventhub.md)
- [フローサービス API を使用したストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

- [UI での Event Hubs ソース接続の作成](../../tutorials/ui/create/cloud-storage/eventhub.md)
- [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
