---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Table Storage;azureテーブルストレージ；ATS;ats
solution: Experience Platform
title: Azureテーブルストレージソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してAzure Table StorageをAdobe Experience Platformに接続する方法を説明します。
exl-id: 096e01b1-7e95-4e30-87de-d0976f8b438a
source-git-commit: 7af79b9e0d6ed29b796ac7c98b4df1dda09f3513
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 18%

---

# [!DNL Azure Table Storage] コネクタ

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。Platformは、リレーショナル、NoSQL、データウェアハウスなど、様々なタイプのデータベースに接続できます。 データベースプロバイダのサポートは[!DNL Azure Table Storage]です。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Table Storage]ソースコネクタは、現在、Platformへの同じ領域接続をサポートしていません。 つまり、AzureインスタンスがPlatformと同じネットワーク領域を使用している場合、Platformソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Azure Table Storage]をPlatformに接続する方法について説明します。

## APIを使用して[!DNL Azure Table Storage]をプラットフォームに接続

- [フローサービスAPIを使用したAzureテーブルストレージベース接続の作成](../../tutorials/api/create/databases/ats.md)
- [フローサービスAPIを使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービスAPIを使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Azure Table Storage]をPlatformに接続します

- [UIでのAzure Table Storageソース接続の作成](../../tutorials/ui/create/databases/ats.md)
- [UIでのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
