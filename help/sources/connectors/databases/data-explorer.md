---
keywords: Experience Platform；ホーム；人気のトピック；Azure Data Explorer;azure data explorer
solution: Experience Platform
title: Azure Data ExplorerSourceの概要
description: API またはユーザーインターフェイスを使用して Azure Data ExplorerをAdobe Experience Platformに接続する方法について説明します。
exl-id: 869bd8bb-51e6-4e0c-a3ec-ff083dda5789
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 33%

---

# [!DNL Azure Data Explorer] ソース

Adobe Experience Platformは、[!DNL Microsoft]、MySQL、[!DNL Azure] などのデータベースプロバイダーとのネイティブ接続を提供します。 これらのシステムから [!DNL Platform] にデータを取り込むことができます。

リレーショナル、NoSQL、データ・ウェアハウスなど、様々なタイプのサード・パーティ・データベースがサポートされています。 データベースプロバイダーのサポートには、[!DNL Azure Data Explorer] が含まれます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Azure Data Explorer] を [!DNL Platform] に接続する方法について説明しています。

## API を使用して [!DNL Azure Data Explorer] と [!DNL Platform] を接続する

- [Flow Service API を使用した Azure Data Explorerベース接続の作成](../../tutorials/api/create/databases/data-explorer.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UIを使用して [!DNL Azure Data Explorer] と [!DNL Platform] を接続する

- [UI での Azure Data Explorerソースコネクタの作成](../../tutorials/ui/create/databases/data-explorer.md)
- [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
