---
keywords: Experience Platform；ホーム；人気の高いトピック；汎用 REST；汎用 REST
solution: Experience Platform
title: 汎用 REST API ソースコネクタの概要
description: API またはユーザーインターフェイスを使用して、汎用 REST API をAdobe Experience Platformに接続する方法を説明します。
exl-id: e3449e33-7261-4aa2-bce9-5530eb4fcc68
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 55%

---

# （ベータ版）[!DNL Generic REST API]

>[!NOTE]
>
>[!DNL Generic REST API] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Platform は、次のようなプロトコルアプリケーションからデータを取り込む機能を備えています。 [!DNL Generic REST API].

この [!DNL Generic REST API] ソースを使用すると、REST ベースのアプリケーションから Platform にデータを取り込むことができます。 [!DNL Generic REST API] は、基本認証と OAuth 2 更新コードベースの認証の両方をサポートしています。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、 [!DNL Generic REST API] API を使用した Platform へのソース。

## API を使用して [!DNL Generic REST API] と [!DNL Platform] を接続する

- [フローサービス API を使用して汎用の REST API ベース接続を作成する](../../tutorials/api/create/protocols/generic-rest.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [フローサービス API を使用したプロトコルソースのデータフローの作成](../../tutorials/api/collect/protocols.md)
