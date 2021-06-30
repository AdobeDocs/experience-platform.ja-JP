---
keywords: Experience Platform；ホーム；人気のあるトピック；AzureData Explorer;Azure Data Explorer
solution: Experience Platform
title: AzureData Explorerソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してAzureData ExplorerをAdobe Experience Platformに接続する方法について説明します。
exl-id: 869bd8bb-51e6-4e0c-a3ec-ff083dda5789
source-git-commit: 5821f9304a37c1a03d17f0113d09548799662a2e
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# （ベータ版） [!DNL Azure Data Explorer]コネクタ

>[!NOTE]
>
>[!DNL Azure Data Explorer]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platformは、[!DNL Microsoft]、MySQL、[!DNL Azure]などのデータベースプロバイダーに対してネイティブ接続を提供します。 これらのシステムのデータを[!DNL Platform]に取り込むことができます。

リレーショナル、NoSQL、データウェアハウスなど、様々な種類のサードパーティデータベースがサポートされています。 データベースプロバイダのサポートには[!DNL Azure Data Explorer]が含まれます。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Data Explorer]ソースコネクタは、現在、Platformへの同じ領域接続をサポートしていません。 つまり、AzureインスタンスがPlatformと同じネットワーク領域を使用している場合、Platformソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Azure Data Explorer]を[!DNL Platform]に接続する方法について説明します。

## APIを使用して[!DNL Azure Data Explorer]を[!DNL Platform]に接続します

- [フローサービスAPIを使用したAzureData Explorerベース接続の作成](../../tutorials/api/create/databases/data-explorer.md)
- [フローサービスAPIを使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービスAPIを使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して[!DNL Azure Data Explorer]を[!DNL Platform]に接続します

- [UIでのAzureData Explorerソース接続の作成](../../tutorials/ui/create/databases/data-explorer.md)
- [UIでのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
