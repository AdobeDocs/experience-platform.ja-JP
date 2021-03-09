---
keywords: Experience Platform；ホーム；人気の高いトピック；Microsoft SQL;microsoft sql;SQL;sql
solution: Experience Platform
title: SQL Serverソースコネクタの概要
topic: 概要
description: APIまたはユーザーインターフェイスを使用してMicrosoft SQL ServerをAdobe Experience Platformに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: 0fb97fcf5d3f8230ff86906aeef245e4a7f44f30
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 9%

---


# （ベータ版） [!DNL Microsoft] SQL Serverコネクタ

Adobe Experience Platformは、[!DNL Platform]サービスを使用して、外部ソースからデータを取り込むと同時に、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダーのサポートには[!DNL Microsoft] SQL Serverが含まれます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Microsoft] SQL Serverソースコネクタは、現在、プラットフォームとの同じ領域接続をサポートしていません。 つまり、Azureインスタンスがプラットフォームと同じネットワーク領域を使用している場合、プラットフォームソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、Adobeのアカウントマネージャーにお問い合わせください。

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Microsoft] SQL Serverを[!DNL Platform]に接続する方法に関する情報を提供しています。

## APIを使用して[!DNL Microsoft] SQL Serverを[!DNL Platform]に接続

- [Flow Service APIを使用してMicrosoft SQL Serverソース接続を作成する](../../tutorials/api/create/databases/sql-server.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Microsoft] SQL Serverを[!DNL Platform]に接続

- [UIでのMicrosoft SQL Serverソース接続の作成](../../tutorials/ui/create/databases/sql-server.md)
- [UIでのデータベース接続用のデータフローの構成](../../tutorials/ui/dataflow/databases.md)