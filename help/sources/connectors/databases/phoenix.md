---
title: Phoenix Sourceの概要
description: API またはユーザーインターフェイスを使用して Phoenix アカウントをAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: 45e6ef18-a0b7-4bb2-b099-b2a878e96637
source-git-commit: a32d0d7ed7d18454099d2b55b3f6809cfbcd9b62
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 30%

---

# [!DNL Phoenix]

>[!IMPORTANT]
>
>[!DNL Phoenix] ソースは 2025 年 5 月末に非推奨（廃止予定）になります。

Adobe Experience Platform ソースは、[[!DNL Phoenix]](https://phoenix.apache.org/index.html) などのサードパーティデータベースからのデータ取り込みをサポートしています。 このドキュメントでは、[!DNL Flow Service] API またはExperience Platformユーザーインターフェイスを使用して [!DNL Phoenix] アカウントに接続する前に、必要な情報を提供します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Phoenix] をExperience Platformに接続する方法について説明しています。

## API を使用して [!DNL Phoenix] をExperience Platformに接続する

* [Flow Service API を使用した Phoenix ベース接続の作成](../../tutorials/api/create/databases/phoenix.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

## UI を使用した [!DNL Phoenix] のExperience Platformへの接続

* [Experience Platformユ  [!DNL Phoenix]  ザーインターフェイスを使用したアカウントの接続](../../tutorials/ui/create/databases/phoenix.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
