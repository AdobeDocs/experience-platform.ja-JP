---
title: Phoenix ソースの概要
description: API またはユーザーインターフェイスを使用して Phoenix アカウントをAdobe Experience Platformに接続する方法を説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: 45e6ef18-a0b7-4bb2-b099-b2a878e96637
source-git-commit: efffd6ce1ed541ce20ee6500e42165465f2fa6a0
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 32%

---

# [!DNL Phoenix]

Adobe Experience Platform Sources は、次のようなサードパーティのデータベースからのデータ取り込みをサポートしています。 [[!DNL Phoenix]](https://phoenix.apache.org/index.html). このドキュメントでは、 [!DNL Phoenix] を通じて説明する [!DNL Flow Service] API またはExperience Platformユーザーインターフェイス。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Phoenix] API またはユーザーインターフェイスを使用してExperience Platformを設定するには：

## 接続 [!DNL Phoenix] API を使用してExperience Platformを設定するには：

* [フローサービス API を使用した Phoenix ベース接続の作成](../../tutorials/api/create/databases/phoenix.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [フローサービス API を使用して、データベースソースのデータフローを作成します](../../tutorials/api/collect/database-nosql.md)

## 接続 [!DNL Phoenix] UI を使用してExperience Platformを設定するには

* [接続する [!DNL Phoenix] アカウントユーザーインターフェイスを使用したExperience Platform](../../tutorials/ui/create/databases/phoenix.md)
* [UI でのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
