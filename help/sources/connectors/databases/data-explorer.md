---
keywords: Experience Platform；ホーム；人気のあるトピック；AzureData Explorer;Azure Data Explorer
solution: Experience Platform
title: AzureData Explorerソースコネクタの概要
topic: overview
description: APIまたはユーザーインターフェイスを使用してAzureData ExplorerをAdobe Experience Platformに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: 0fb97fcf5d3f8230ff86906aeef245e4a7f44f30
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# （ベータ版） [!DNL Azure Data Explorer]コネクタ

>[!NOTE]
>
>[!DNL Azure Data Explorer]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformは、[!DNL Microsoft]、MySQL、[!DNL Azure]などのデータベースプロバイダに対してネイティブの接続を提供します。 これらのシステムのデータを[!DNL Platform]に取り込むことができます。

リレーショナル、NoSQL、データ・ウェアハウスなど、サード・パーティ製のデータベースは、それぞれ異なるタイプがサポートされます。 データベースプロバイダーのサポートには[!DNL Azure Data Explorer]が含まれます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Data Explorer]ソースコネクタは、現在、プラットフォームとの同じ領域接続をサポートしていません。 つまり、Azureインスタンスがプラットフォームと同じネットワーク領域を使用している場合、プラットフォームソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、Adobeのアカウントマネージャーにお問い合わせください。

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Azure Data Explorer]を[!DNL Platform]に接続する方法に関する情報を提供しています。

## APIを使用して[!DNL Azure Data Explorer]を[!DNL Platform]に接続

- [Flow Service APIを使用してAzureData Explorerソース接続を作成する](../../tutorials/api/create/databases/data-explorer.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Azure Data Explorer]を[!DNL Platform]に接続

- [UIでAzureData Explorerソース接続を作成する](../../tutorials/ui/create/databases/data-explorer.md)
- [UIでのデータベース接続用のデータフローの構成](../../tutorials/ui/dataflow/databases.md)