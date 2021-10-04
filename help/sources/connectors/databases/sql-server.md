---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft SQL;Microsoft SQL;SQL;SQL
solution: Experience Platform
title: SQL Server ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Microsoft SQL Server をAdobe Experience Platformに接続する方法を説明します。
exl-id: 8a77f108-7e82-4e14-a470-a4ea97def89d
source-git-commit: 5821f9304a37c1a03d17f0113d09548799662a2e
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 9%

---

# [!DNL Microsoft] SQL Server コネクタ

Adobe Experience Platformを使用すると、[!DNL Platform] サービスを使用して、受信データの構造化、ラベル付け、強化を行うことができ、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。[!DNL Platform] は、リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのデータベースに接続できます。データベースプロバイダのサポートには、[!DNL Microsoft] SQL Server が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Microsoft] SQL Server ソースコネクタは、現在、Platform への同じ領域接続をサポートしていません。 つまり、Azure インスタンスが Platform と同じネットワーク領域を使用している場合、Platform ソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Microsoft] SQL Server を [!DNL Platform] に接続する方法に関する情報を提供します。

## API を使用して [!DNL Microsoft] SQL Server を [!DNL Platform] に接続

- [フローサービス API を使用した Microsoft SQL Server ベース接続の作成](../../tutorials/api/create/databases/sql-server.md)
- [フローサービス API を使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービス API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用して [!DNL Microsoft] SQL Server を [!DNL Platform] に接続

- [UI での Microsoft SQL Server ソース接続の作成](../../tutorials/ui/create/databases/sql-server.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
