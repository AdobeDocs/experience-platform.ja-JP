---
keywords: Experience Platform;home;popular topics;Azure Synapse Analytics;azure synapse analytics;Synapse;synapse
solution: Experience Platform
title: Azure Synapse Analyticsコネクタ
topic: overview
description: 以下のドキュメントは、APIまたはユーザーインターフェイスを使用してAzure Synapse Analyticsをプラットフォームに接続する方法に関する情報を提供しています。
translation-type: tm+mt
source-git-commit: d3ece56d10b1940a5992906a65a50ffe2f7e4346
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 12%

---


# （ベータ版） [!DNL Azure Synapse Analytics] コネクタ

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートには以下が含まれ [!DNL Azure Synapse Analytics]ます。

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

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Azure Synapse Analytics] を [!DNL Platform] 使用して接続する方法に関する情報を提供しています。

## API [!DNL Azure Synapse Analytics] を [!DNL Platform] 使用した接続

- [Flow Service APIを使用してAzure Synapse Analyticsコネクタを作成する](../../tutorials/api/create/databases/synapse-analytics.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UI [!DNL Azure Synapse Analytics] を [!DNL Platform] 使用して接続

- [UIにAzure Synapse Analyticsソースコネクタを作成する](../../tutorials/ui/create/databases/synapse-analytics.md)
- [UIでのデータベースコネクタのデータフローの設定](../../tutorials/ui/dataflow/databases.md)