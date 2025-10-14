---
keywords: Experience Platform；ホーム；人気のトピック；汎用 REST；汎用 rest
solution: Experience Platform
title: 汎用 REST API Source コネクタの概要
description: API またはユーザーインターフェイスを使用して汎用 REST API をAdobe Experience Platformに接続する方法について説明します。
exl-id: e3449e33-7261-4aa2-bce9-5530eb4fcc68
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 49%

---

# （ベータ版）[!DNL Generic REST API]

>[!NOTE]
>
>[!DNL Generic REST API] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[&#x200B; ソースの概要 &#x200B;](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、[!DNL Generic REST API] などのプロトコルアプリケーションからのデータ取り込みをサポートしています。

[!DNL Generic REST API] ソースを使用すると、REST ベースのアプリケーションからExperience Platformにデータを取り込むことができます。 [!DNL Generic REST API] は、基本認証と OAuth 2 更新コードベースの認証の両方をサポートしています。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

以下のドキュメントでは、API を使用して [!DNL Generic REST API] ソースをExperience Platformに接続する方法に関する情報を提供します。

## API を使用して [!DNL Generic REST API] と [!DNL Experience Platform] を接続する

- [Flow Service API を使用した汎用 REST API ベース接続の作成](../../tutorials/api/create/protocols/generic-rest.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したプロトコルソースのデータフローの作成](../../tutorials/api/collect/protocols.md)
