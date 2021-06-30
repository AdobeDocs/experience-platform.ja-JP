---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft SQL;Microsoft SQL;SQL;SQL
solution: Experience Platform
title: SQL Serverソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してMicrosoft SQL ServerをAdobe Experience Platformに接続する方法について説明します。
exl-id: 8a77f108-7e82-4e14-a470-a4ea97def89d
source-git-commit: 5821f9304a37c1a03d17f0113d09548799662a2e
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 9%

---

# [!DNL Microsoft] SQL Serverコネクタ

Adobe Experience Platformを使用すると、[!DNL Platform]サービスを使用して、受信データの構造化、ラベル付け、拡張を行いながら、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] は、リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダのサポートには、[!DNL Microsoft] SQL Serverが含まれます。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

>[!IMPORTANT]
>
>[!DNL Microsoft] SQL Serverソースコネクタは、現在、Platformへの同じ領域接続をサポートしていません。 つまり、AzureインスタンスがPlatformと同じネットワーク領域を使用している場合、Platformソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Microsoft] SQL Serverを[!DNL Platform]に接続する方法に関する情報を提供します。

## APIを使用して[!DNL Microsoft] SQL Serverを[!DNL Platform]に接続します

- [フローサービスAPIを使用したMicrosoft SQL Serverベース接続の作成](../../tutorials/api/create/databases/sql-server.md)
- [フローサービスAPIを使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービスAPIを使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Microsoft] SQL Serverを[!DNL Platform]に接続します

- [UIでのMicrosoft SQL Serverソース接続の作成](../../tutorials/ui/create/databases/sql-server.md)
- [UIでのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
