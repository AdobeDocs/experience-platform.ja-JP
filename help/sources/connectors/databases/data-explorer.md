---
keywords: Experience Platform；ホーム；人気のあるトピック；AzureData Explorer;Azure Data Explorer
solution: Experience Platform
title: AzureData Explorerソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して AzureData ExplorerをAdobe Experience Platformに接続する方法を説明します。
exl-id: 869bd8bb-51e6-4e0c-a3ec-ff083dda5789
source-git-commit: 5821f9304a37c1a03d17f0113d09548799662a2e
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# （ベータ版） [!DNL Azure Data Explorer] コネクタ

>[!NOTE]
>
>[!DNL Azure Data Explorer] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platformは、[!DNL Microsoft]、MySQL、[!DNL Azure] などのデータベースプロバイダーにネイティブ接続を提供します。 これらのシステムのデータを [!DNL Platform] に取り込むことができます。

リレーショナル、NoSQL、データウェアハウスなど、様々な種類のサードパーティデータベースがサポートされています。 データベースプロバイダのサポートには [!DNL Azure Data Explorer] が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Data Explorer] ソースコネクタは、現在、Platform への同じ領域接続をサポートしていません。 つまり、Azure インスタンスが Platform と同じネットワーク領域を使用している場合、Platform ソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Azure Data Explorer] を [!DNL Platform] に接続する方法について説明します。

## API を使用して [!DNL Azure Data Explorer] を [!DNL Platform] に接続

- [フローサービス API を使用した AzureData Explorerベース接続の作成](../../tutorials/api/create/databases/data-explorer.md)
- [フローサービス API を使用したデータベースソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/database-nosql.md)
- [フローサービス API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用して [!DNL Azure Data Explorer] を [!DNL Platform] に接続します

- [UI での AzureData Explorerソース接続の作成](../../tutorials/ui/create/databases/data-explorer.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
