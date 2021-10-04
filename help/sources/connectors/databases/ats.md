---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Table Storage;Azure Table Storage;ATS;ATS
solution: Experience Platform
title: Azure テーブルストレージソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Azure Table Storage をAdobe Experience Platformに接続する方法を説明します。
exl-id: 096e01b1-7e95-4e30-87de-d0976f8b438a
source-git-commit: 7af79b9e0d6ed29b796ac7c98b4df1dda09f3513
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 18%

---

# [!DNL Azure Table Storage] コネクタ

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Platform は、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダのサポートは [!DNL Azure Table Storage] です。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Table Storage] ソースコネクタは、現在、Platform への同じ領域接続をサポートしていません。 つまり、Azure インスタンスが Platform と同じネットワーク領域を使用している場合、Platform ソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Azure Table Storage] を Platform に接続する方法について説明します。

## API を使用して [!DNL Azure Table Storage] を Platform に接続

- [フローサービス API を使用した Azure テーブルストレージベース接続の作成](../../tutorials/api/create/databases/ats.md)
- [フローサービス API を使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービス API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用して [!DNL Azure Table Storage] を Platform に接続

- [UI での Azure テーブルストレージソース接続の作成](../../tutorials/ui/create/databases/ats.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
