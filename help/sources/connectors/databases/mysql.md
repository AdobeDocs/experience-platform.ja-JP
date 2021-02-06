---
keywords: Experience Platform；ホーム；人気の高いトピック；MySQL;mysql;My sql;My SQL
solution: Experience Platform
title: MySQL Source Connectorの概要
topic: overview
description: APIまたはユーザーインターフェイスを使用してMySQLをAdobe Experience Platformに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: c7fb0d50761fa53c1fdf4dd70a63c62f2dcf6c85
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 11%

---


# （ベータ版） MySQL Connector

>[!NOTE]
>
>MySQL Connectorはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformは、[!DNL Platform]サービスを使用して、外部ソースからデータを取り込むと同時に、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダーのサポートにはMySQLが含まれます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してMySQLを[!DNL Platform]に接続する方法に関する情報を提供しています。

## APIを使用してMySQLを[!DNL Platform]に接続

- [Flow Service APIを使用してMySQLソース接続を作成する](../../tutorials/api/create/databases/mysql.md)
- [Flow Service APIを使用したデータベースシステムの調査](../../tutorials/api/explore/database-nosql.md)
- [Flow Service APIを使用してデータベースからデータを収集する](../../tutorials/api/collect/database-nosql.md)

## UIを使用してMySQLを[!DNL Platform]に接続

- [UIでのMySQLソース接続の作成](../../tutorials/ui/create/databases/mysql.md)
- [UIでのデータベース接続用のデータフローの構成](../../tutorials/ui/dataflow/databases.md)