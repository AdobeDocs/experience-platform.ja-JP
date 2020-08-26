---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: HubSpotコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 690ddbd92f0a2e4e06b988e761dabff399cd2367
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 18%

---


# （ベータ版） [!DNL HubSpot] コネクタ

>[!NOTE]
>
>コネクタ [!DNL HubSpot] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform ] は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。マーケティング自動化プロバイダーのサポートには以下が含まれ [!DNL HubSpot]ます。

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

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL HubSpot] を [!DNL Platform] 使用して接続する方法に関する情報を提供しています。

## API [!DNL HubSpot] を [!DNL Platform] 使用した接続

- [Flow Service APIを使用してHubSpotコネクタを作成する](../../tutorials/api/create/marketing-automation/hubspot.md)
- [Flow Service APIを使用したマーケティング自動化システムの調査](../../tutorials/api/explore/marketing-automation.md)
- [Flow Service APIを使用してマーケティング自動化データを収集する](../../tutorials/api/collect/marketing-automation.md)

## UI [!DNL HubSpot] を [!DNL Platform] 使用して接続

- [UI での HubSpot ソースコネクタの作成](../../tutorials/ui/create/marketing-automation/hubspot.md)
- [UIでのマーケティング自動化コネクタのデータフローの設定](../../tutorials/ui/dataflow/marketing-automation.md)