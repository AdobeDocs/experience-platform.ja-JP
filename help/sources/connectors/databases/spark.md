---
keywords: Experience Platform；ホーム；人気のトピック；Apache Spark;Apache Spark;Azure HDInsights;azure hdinsights
solution: Experience Platform
title: Azure HDInsights での Apache Spark ソースコネクタの概要
description: API またはユーザーインターフェイスを使用して、Azure HDInsights 上の Apache Spark をAdobe Experience Platformに接続する方法を説明します。
exl-id: c4a2a14e-5e16-44b7-b3f1-a98b7229f69e
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 46%

---

# （ベータ版） [!DNL Apache Spark] オン [!DNL Azure HDInsights] コネクタ

>[!NOTE]
>
>この [!DNL Apache Spark] オン [!DNL Azure HDInsights] コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] は、リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダーのサポートは次のとおりです [!DNL Apache Spark] オン [!DNL Azure HDInsights].

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Apache Spark] オン [!DNL Azure HDInsights] から [!DNL Platform] API またはユーザーインターフェイスを使用する場合：

## 接続 [!DNL Apache Spark] オン [!DNL Azure HDInsights] から [!DNL Platform] API の使用

- [フローサービス API を使用して、Azure HDInsights ベースの接続に Apache Spark を作成する](../../tutorials/api/create/databases/spark.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [フローサービス API を使用して、データベースソースのデータフローを作成します](../../tutorials/api/collect/database-nosql.md)

## 接続 [!DNL Apache Spark] オン [!DNL Azure HDInsights] から [!DNL Platform] UI の使用

- [UI での Azure HDInsights ソース接続での Apache Spark の作成](../../tutorials/ui/create/databases/spark.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
