---
title: Azure Synapse Analytics Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAzure Synapse Analytics をAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 5b94ae74-e5a7-40e9-a952-41eddf06dcde
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 46%

---

# [!DNL Azure Synapse Analytics] ソース

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Synapse Analytics] ソースを利用できます。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] では、サードパーティのデータベースからデータを取り込むことができます。 リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続で [!DNL Experience Platform] ます。 データベースプロバイダーのサポートには、[!DNL Azure Synapse Analytics] が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Azure Synapse Analytics] を [!DNL Experience Platform] に接続する方法について説明しています。

## API を使用して [!DNL Azure Synapse Analytics] と [!DNL Experience Platform] を接続する

- [Flow Service API を使用したAzure Synapse Analytics ベース接続の作成](../../tutorials/api/create/databases/synapse-analytics.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して [!DNL Azure Synapse Analytics] と [!DNL Experience Platform] を接続する

- [UI でのAzure Synapse Analytics ソースコネクタの作成](../../tutorials/ui/create/databases/synapse-analytics.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
