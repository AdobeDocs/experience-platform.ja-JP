---
keywords: Experience Platform；ホーム；人気のあるトピック；Amazon Redshift;Amazon redshift;redshift;Redshift
solution: Experience Platform
title: Snowflakeソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してSnowflakeをAdobe Experience Platformに接続する方法を説明します。
exl-id: df066463-1ae6-4ecd-ae0e-fb291cec4bd5
source-git-commit: db483110b8bfd5290f6a9a30fdb008f478fdbbf4
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 21%

---

# （ベータ版） [!DNL Snowflake] ソース

>[!NOTE]
>
>[!DNL Snowflake] ソースはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダのサポートは [!DNL Snowflake] です。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Snowflake] を Platform に接続する方法について説明します。

## API を使用して [!DNL Snowflake] を Platform に接続

- [フローサービス API を使用したSnowflakeベース接続の作成](../../tutorials/api/create/databases/snowflake.md)
- [フローサービス API を使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービス API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用して [!DNL Snowflake] を Platform に接続

- [UI でのSnowflakeソース接続の作成](../../tutorials/ui/create/databases/snowflake.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
