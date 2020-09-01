---
keywords: Experience Platform;home;popular topics;Apache Spark;apache spark;Azure HDInsights;azure hdinsights
solution: Experience Platform
title: Azure HDInsightsコネクタ上のApache Spark
topic: overview
description: 以下のドキュメントは、APIまたはユーザーインターフェイスを使用してAzure HDInsightsのApache Sparkをプラットフォームに接続する方法に関する情報を提供しています。
translation-type: tm+mt
source-git-commit: d3ece56d10b1940a5992906a65a50ffe2f7e4346
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 11%

---


# （ベータ版） [!DNL Apache Spark] on [!DNL Azure HDInsights] connector

>[!NOTE]
>
>オン [!DNL Apache Spark] の [!DNL Azure HDInsights] コネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダのサポートには、が含 [!DNL Apache Spark] まれ [!DNL Azure HDInsights]ます。

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

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Apache Spark] を使用し [!DNL Azure HDInsights] てに接続する方法に関する情報を提供し [!DNL Platform] ています。

## API [!DNL Apache Spark] を使用し [!DNL Azure HDInsights][!DNL Platform] た接続

- [Flow Service APIを使用してAzure HDInsights ConnectorでApache Sparkを作成する](../../tutorials/api/create/databases/spark.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UI [!DNL Apache Spark] を使用 [!DNL Azure HDInsights] して接続 [!DNL Platform] する

- [UIでAzure HDInsightsソースコネクタ上にApache Sparkを作成します](../../tutorials/ui/create/databases/spark.md)
- [UIでのデータベースコネクタのデータフローの設定](../../tutorials/ui/dataflow/databases.md)