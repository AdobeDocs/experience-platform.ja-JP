---
keywords: Experience Platform；ホーム；人気の高いトピック；汎用 REST；汎用 REST
solution: Experience Platform
title: 汎用 REST API ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して、汎用 REST API をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 0c7bb3d6f0a1bc4154bff0e4d79cc4c3c0b0ab71
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 8%

---

# (ベータ) [!DNL Generic REST API]

>[!NOTE]
>
>この [!DNL Generic REST API] ソースはベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

Adobe Experience Platformを使用すると、データを外部ソースから取り込みながら、 [!DNL Platform] サービス。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

Platform は、次のようなプロトコルアプリケーションからデータを取り込む機能を備えています。 [!DNL Generic REST API].

この [!DNL Generic REST API] ソースを使用すると、REST ベースのアプリケーションから Platform にデータを取り込むことができます。 [!DNL Generic REST API] は、基本認証と OAuth 2 更新コードベースの認証の両方をサポートしています。

## IP アドレスの許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、 [IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

以下のドキュメントでは、 [!DNL Generic REST API] API を使用した Platform へのソース。

## 接続 [!DNL Generic REST API] から [!DNL Platform] API の使用

- [フローサービス API を使用して汎用の REST API ベース接続を作成する](../../tutorials/api/create/protocols/generic-rest.md)
- [フローサービス API を使用したプロトコルソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/protocols.md)
- [フローサービス API を使用したプロトコルソースのデータフローの作成](../../tutorials/api/collect/protocols.md)