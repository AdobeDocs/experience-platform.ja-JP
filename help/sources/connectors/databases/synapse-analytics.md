---
title: Azure synapse Analytics Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAzure synapse分析をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 5b94ae74-e5a7-40e9-a952-41eddf06dcde
source-git-commit: 9a8139c26b5bb5ff937a51986967b57db58aab6c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 46%

---

# [!DNL Azure Synapse Analytics] ソース

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Azure Synapse Analytics] ソースを利用できます。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] では、サードパーティのデータベースからデータを取り込むことができます。 リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続で [!DNL Platform] ます。 データベースプロバイダーのサポートには、[!DNL Azure Synapse Analytics] が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Azure Synapse Analytics] を [!DNL Platform] に接続する方法について説明しています。

## API を使用して [!DNL Azure Synapse Analytics] と [!DNL Platform] を接続する

- [Flow Service API を使用したAzure synapse分析ベース接続の作成](../../tutorials/api/create/databases/synapse-analytics.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して [!DNL Azure Synapse Analytics] と [!DNL Platform] を接続する

- [UI でのAzure synapse Analytics ソースコネクタの作成](../../tutorials/ui/create/databases/synapse-analytics.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
