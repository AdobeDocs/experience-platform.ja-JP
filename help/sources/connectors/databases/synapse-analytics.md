---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure synapse分析；azure synapse分析；Synapse;synapse
solution: Experience Platform
title: azure synapse分析ソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してAzure synapseAnalyticsをAdobe Experience Platformに接続する方法について説明します。
exl-id: 5b94ae74-e5a7-40e9-a952-41eddf06dcde
source-git-commit: e150f05df2107d7b3a2e95a55dc4ad072294279e
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 9%

---

# [!DNL Azure Synapse Analytics] コネクタ

Adobe Experience Platformを使用すると、[!DNL Platform]サービスを使用して、受信データの構造化、ラベル付け、拡張を行いながら、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] は、リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダのサポートは[!DNL Azure Synapse Analytics]です。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Synapse Analytics]ソースコネクタは、現在、Platformへの同じ領域接続をサポートしていません。 つまり、AzureインスタンスがPlatformと同じネットワーク領域を使用している場合、Platformソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Azure Synapse Analytics]を[!DNL Platform]に接続する方法について説明します。

## APIを使用して[!DNL Azure Synapse Analytics]を[!DNL Platform]に接続します

- [フローサービスAPIを使用したAzure synapse分析ソース接続の作成](../../tutorials/api/create/databases/synapse-analytics.md)
- [フローサービスAPIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービスAPIを使用したデータベースからのデータの収集](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Azure Synapse Analytics]を[!DNL Platform]に接続します

- [UIでのAzure synapseAnalyticsソース接続の作成](../../tutorials/ui/create/databases/synapse-analytics.md)
- [UIでのデータベース接続のデータフローの設定](../../tutorials/ui/dataflow/databases.md)
