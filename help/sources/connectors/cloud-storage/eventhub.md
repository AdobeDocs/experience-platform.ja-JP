---
title: Azure Event Hubs Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Azure Event Hubs をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: b4d4bc7f-2241-482d-a5c2-4422c31705bf
source-git-commit: 84d09038ded1f35269ebf67c6bc1a5dacaafe4ac
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 18%

---

# [!DNL Azure Event Hubs] ソース

>[!IMPORTANT]
>
>* Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Event Hubs] ソースを利用できます。
>
>* Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Azure Event Hubs] ソースを使用できるようになりました。 AWSで実行されるExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platformインフラストラクチャについて詳しくは、[Experience Platformマルチクラウドの概要 ](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続を提供します。 これらのシステムから Platform にデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータを Platform に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。Platform では、[!DNL Event Hubs] からリアルタイムにデータを取り込むことができます。

## [!DNL Event Hubs] を使用したスケーリング

大量のデータを取り込む、並列性を高める、取り込みプラットフォームの速度を上げる必要がある場合は、[!DNL Event Hubs] インスタンスのスケールファクターを上げる必要があります。

### 大量のデータを取り込む

現在、[!DNL Event Hubs] アカウントから Platform に取り込めるデータの最大量は、1 秒あたり 2,000 レコードです。 大量のデータをスケールアップして取り込むには、Adobe担当者にお問い合わせください。

### [!DNL Event Hubs] と Platform の並列処理を強化

並列処理とは、複数の処理ユニットで同じタスクを同時に実行することで、処理の高速化と高性能化を図ることを指します。 パーティションを増やすか、[!DNL Event Hubs] アカウントの処理単位を増やすことで、[!DNL Event Hubs] 側の並列処理を増やすことができます。 詳しくは、この [[!DNL Event Hubs]  拡大縮小に関するドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-scalability) を参照してください。

Platform 側で取り込み速度を上げるには、Platform はソースコネクタ内のタスク数を増やして、[!DNL Event Hubs] パーティションから読み取る必要があります。 [!DNL Event Hubs] 側の並列性を高めたら、Adobe担当者に連絡して、新しいパーティションに基づいて Platform タスクを拡張してください。 現在、このプロセスは自動化されていません。

## 仮想ネットワークを使用して [!DNL Event Hubs] と Platform を接続する

ファイアウォール測定を有効にしながら [!DNL Event Hubs] を Platform に接続するように、仮想ネットワークを設定できます。 仮想ネットワークを設定するには、この [[!DNL Event Hubs]  ネットワークのルールセットのドキュメント ](https://learn.microsoft.com/en-us/azure/event-hubs/network-security) を参照し、次の手順に従います。

* REST API パネルから **試す** を選択します。
* 同じブラウザーの資格情報を使用して [!DNL Azure] アカウントを認証します。
* Platform に取り込む [!DNL Event Hubs] 名前空間、リソースグループ、サブスクリプションを選択してから、「**実行**」を選択します。
* 表示される JSON 本文で、`properties` 内の `virtualNetworkRules` の下に次のプラットフォームサブネットを追加します。


>[!IMPORTANT]
>
>既存の IP フィルタリングルールが含まれているので、Platform サブネットで `virtualNetworkRules` を更新する前に、受信する JSON 本文のバックアップを作成する必要があります。 それ以外の場合、ルールは呼び出し後に削除されます。


```json
{
    "subnet": {
        "id": "/subscriptions/93f21779-b1fd-49ee-8547-2cdbc979a44f/resourceGroups/ethos_12_prod_va7_network/providers/Microsoft.Network/virtualNetworks/ethos_12_prod_va7_network_10_19_144_0_22/subnets/ethos_12_prod_va7_network_10_19_144_0_22"
    },
    "ignoreMissingVnetServiceEndpoint": true
}
```

Platform サブネットの様々な地域については、以下のリストを参照してください。

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

## [!DNL Event Hubs] を Platform に接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Event Hubs] と Platform を接続する方法について説明します。

### API の使用

* [Flow Service API を使用した Event Hubs ソース接続の作成](../../tutorials/api/create/cloud-storage/eventhub.md)
* [Flow Service API を用いたストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

* [UI での Event Hubs ソース接続の作成](../../tutorials/ui/create/cloud-storage/eventhub.md)
* [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
