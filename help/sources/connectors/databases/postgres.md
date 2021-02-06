---
keywords: Experience Platform；ホーム；人気の高いトピック；PostgreSQL;postgresql
solution: Experience Platform
title: PostgreSQLソースコネクタの概要
topic: overview
description: APIまたはユーザインターフェイスを使用してPostgreSQLをAdobe Experience Platformに接続する方法を学びます。
translation-type: tm+mt
source-git-commit: c7fb0d50761fa53c1fdf4dd70a63c62f2dcf6c85
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 22%

---


# （ベータ版） [!DNL PostgreSQL]コネクタ

>[!NOTE]
>
>[!DNL PostgreSQL]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダのサポートは[!DNL PostgreSQL]です。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

次のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL PostgreSQL]を[!DNL Platform]に接続する方法に関する情報を提供しています。

## APIを使用して[!DNL PostgreSQL]を[!DNL Platform]に接続

- [Flow Service APIを使用したPostgreSQLソース接続の作成](../../tutorials/api/create/databases/postgres.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL PostgreSQL]を[!DNL Platform]に接続

- [UIでのPostgreSQLソース接続の作成](../../tutorials/ui/create/databases/postgres.md)
- [UIでのデータベース接続用のデータフローの構成](../../tutorials/ui/dataflow/databases.md)