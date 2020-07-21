---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: SQL Serverコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 3b5e76afea5689dbd59f64f6192e6ef2a6acb7d3
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---


# （ベータ版） [!DNL Microsoft] SQL Serverコネクタ

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取り込むためのサポートを提供します。 [!DNL Platform] リレーショナル、NoSQL、data warehouseなど、様々な種類のデータベースに接続できます。 データベースプロバイダーのサポートには、 [!DNL Microsoft] SQL Serverが含まれます。

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

次のドキュメントは、APIまたはユーザーインターフェイスを使用して [!DNL Microsoft] SQL Serverを接続する方法に関する情報 [!DNL Platform] を提供しています。

## APIを使用したSQL Serverへの接続 [!DNL Microsoft][!DNL Platform]

- [Flow Service APIを使用してMicrosoft SQL Serverコネクタを作成する](../../tutorials/api/create/databases/sql-server.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用したSQL Serverへの接続 [!DNL Microsoft][!DNL Platform]

- [UIでのMicrosoft SQL Serverソースコネクタの作成](../../tutorials/ui/create/databases/sql-server.md)
- [UIでのデータベースコネクタのデータフローの設定](../../tutorials/ui/dataflow/databases.md)