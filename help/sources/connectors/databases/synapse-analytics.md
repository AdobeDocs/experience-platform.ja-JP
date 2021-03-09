---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure synapse分析；azure synapse分析；シナプス；シナプス
solution: Experience Platform
title: azure synapse分析ソースコネクタの概要
topic: 概要
description: APIまたはユーザーインターフェイスを使用して、Azure synapse分析をAdobe Experience Platformに接続する方法について説明します。
translation-type: tm+mt
source-git-commit: 0fb97fcf5d3f8230ff86906aeef245e4a7f44f30
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 10%

---


# （ベータ版） [!DNL Azure Synapse Analytics]コネクタ

Adobe Experience Platformは、[!DNL Platform]サービスを使用して、外部ソースからデータを取り込むと同時に、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダのサポートは[!DNL Azure Synapse Analytics]です。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Synapse Analytics]ソースコネクタは、現在、プラットフォームとの同じ領域接続をサポートしていません。 つまり、Azureインスタンスがプラットフォームと同じネットワーク領域を使用している場合、プラットフォームソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、Adobeのアカウントマネージャーにお問い合わせください。

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Azure Synapse Analytics]を[!DNL Platform]に接続する方法に関する情報を提供しています。

## APIを使用して[!DNL Azure Synapse Analytics]を[!DNL Platform]に接続

- [Flow Service APIを使用してAzure synapse分析ソース接続を作成する](../../tutorials/api/create/databases/synapse-analytics.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Azure Synapse Analytics]を[!DNL Platform]に接続

- [UIでのAzure synapse分析ソース接続の作成](../../tutorials/ui/create/databases/synapse-analytics.md)
- [UIでのデータベース接続用のデータフローの構成](../../tutorials/ui/dataflow/databases.md)