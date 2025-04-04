---
title: Azure Event Hubs Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Azure Event Hubs をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: b4d4bc7f-2241-482d-a5c2-4422c31705bf
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 9%

---

# [!DNL Azure Event Hubs] ソース

>[!IMPORTANT]
>
>* Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Event Hubs] ソースを利用できます。
>
>* Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Azure Event Hubs] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続を提供します。 これらのシステムからExperience Platformにデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをExperience Platformに取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。Experience Platformでは、[!DNL Event Hubs] からリアルタイムにデータを取り込むことができます。

## [!DNL Event Hubs] を使用したスケーリング

大量のデータを取り込む、並列性を高める、Experience Platformでの取り込み速度を上げる必要がある場合は、[!DNL Event Hubs] インスタンスのスケール係数を上げる必要があります。

### 大量のデータを取り込む

現在、[!DNL Event Hubs] アカウントからExperience Platformに取り込めるデータの最大量は、1 秒あたり 2,000 レコードです。 大量のデータをスケールアップして取り込むには、Adobe担当者にお問い合わせください。

### [!DNL Event Hubs] とExperience Platformの並列処理を強化

並列処理とは、複数の処理ユニットで同じタスクを同時に実行することで、処理の高速化と高性能化を図ることを指します。 パーティションを増やすか、[!DNL Event Hubs] アカウントの処理単位を増やすことで、[!DNL Event Hubs] 側の並列処理を増やすことができます。 詳しくは、この [[!DNL Event Hubs]  拡大縮小に関するドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-scalability) を参照してください。

Experience Platform側の取り込み速度を上げるには、Experience Platformが [!DNL Event Hubs] パーティションから読み取るソースコネクタのタスク数を増やす必要があります。 [!DNL Event Hubs] 側の並列性を高めたら、Adobeの担当者に連絡して、新しいパーティションに基づいてExperience Platform タスクを拡張してください。 現在、このプロセスは自動化されていません。

## 仮想ネットワークを使用して [!DNL Event Hubs] をExperience Platformに接続する

ファイアウォール対策を有効にしながらExperience Platformに接続する [!DNL Event Hubs] うに、仮想ネットワークを設定できます。 仮想ネットワークを設定するには、この [[!DNL Event Hubs]  ネットワークのルールセットのドキュメント ](https://learn.microsoft.com/en-us/azure/event-hubs/network-security) を参照し、次の手順に従います。

* REST API パネルから **試す** を選択します。
* 同じブラウザーの資格情報を使用して [!DNL Azure] アカウントを認証します。
* Experience Platformに取り込む [!DNL Event Hubs] 名前空間、リソースグループ、サブスクリプションを選択してから、「**実行**」を選択します。
* 表示される JSON 本文で、内 `properties` の下に次のExperience Platform サブネット `virtualNetworkRules` 追加します。


>[!IMPORTANT]
>
>既存の IP フィルタリングルールが含まれているので、Experience Platform サブネットで `virtualNetworkRules` を更新する前に、受け取る JSON 本文のバックアップを作成する必要があります。 それ以外の場合、ルールは呼び出し後に削除されます。


```json
{
    "subnet": {
        "id": "/subscriptions/93f21779-b1fd-49ee-8547-2cdbc979a44f/resourceGroups/ethos_12_prod_va7_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_va7_network_10_19_144_0_22/subnets/ethos_12_prod_va7_network_10_19_144_0_22"
    },
    "ignoreMissingVnetServiceEndpoint": true
}
```

Experience Platform サブネットの様々なリージョンについては、以下のリストを参照してください。

### VA7：北米

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

### NLD2：ヨーロッパ

```json
{
  "properties": {
    "defaultAction": "Deny",
    "virtualNetworkRules": [
      {
        "subnet": {
            "id": "/subscriptions/40bde086-46ad-44c3-afba-c306f54b64ec/resourceGroups/ethos_12_prod_nld2_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_nld2-vnet/subnets/ethos_12_prod_nld2_network_10_20_40_0_23"
        }, 
        "ignoreMissingVnetServiceEndpoint": true
      },
    ],
    "ipRules": []
  }
}
```

### AUS5：オーストラリア

```json
{
  "properties": {
    "defaultAction": "Deny",
    "virtualNetworkRules": [
      {
        "subnet": {
          "id": "/subscriptions/1618ef18-9edc-48bf-88dd-61cc979629b5/resourceGroups/ethos_12_prod_aus5_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_aus5-vnet/subnets/ethos_12_prod_aus5_network_10_21_116_0_22"
        },
        "ignoreMissingVnetServiceEndpoint": true
      },
    ],
    "ipRules": []
  }
}
```

ネットワーク ルール セットの詳細については、次の [[!DNL Event Hubs]  ドキュメント ](https://learn.microsoft.com/en-us/azure/event-hubs/network-security) を参照してください。

## [!DNL Event Hubs] をExperience Platformに接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Event Hubs] をExperience Platformに接続する方法について説明しています。

### API の使用

* [Flow Service API を使用した Event Hubs ソース接続の作成](../../tutorials/api/create/cloud-storage/eventhub.md)
* [Flow Service API を用いたストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

* [UI での Event Hubs ソース接続の作成](../../tutorials/ui/create/cloud-storage/eventhub.md)
* [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
