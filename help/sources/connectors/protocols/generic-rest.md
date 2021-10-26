---
keywords: Experience Platform；ホーム；人気のあるトピック；汎用 REST；汎用 REST
solution: Experience Platform
title: 汎用 REST API ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して汎用 REST API をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 127c2764b8414ee9b59d49ec04cbbd28269ca496
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 7%

---

# (ベータ) [!DNL Generic REST API]

>[!NOTE]
>
>10. [!DNL Generic REST API] ソースはベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベルのコネクタの使用に関する詳細

Adobe Experience Platformを使用すると、データを外部ソースから取り込みながら、 [!DNL Platform] サービス アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

Platform は、次のようなプロトコルアプリケーションからデータを取り込む機能を提供します。 [!DNL Generic REST API].

10. [!DNL Generic REST API] ソースを使用すると、REST ベースのアプリケーションからデータを Platform に取り込むことができます。 [!DNL Generic REST API] は、基本認証と OAuth 2 更新コードベースの認証の両方をサポートしています。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、 [IP アドレス許可リスト](../../ip-address-allow-list.md) ページを参照してください。

以下のドキュメントでは、 [!DNL Generic REST API] API を使用して Platform にソース。

## 接続 [!DNL Generic REST API] を [!DNL Platform] API の使用

- [フローサービス API を使用した汎用 REST API ベース接続の作成](../../tutorials/api/create/protocols/generic-rest.md)
- [フローサービス API を使用したプロトコルソースのデータ構造と内容の調査](../../tutorials/api/explore/protocols.md)
- [フローサービス API を使用したプロトコルソースのデータフローの作成](../../tutorials/api/collect/protocols.md)

## 接続 [!DNL Generic REST API] を [!DNL Platform] UI の使用

- [UI での汎用 REST API ソース接続の作成](../../tutorials/ui/create/protocols/generic-rest.md)
- [UI でのプロトコルソース接続のデータフローの作成](../../tutorials/ui/dataflow/protocols.md)


