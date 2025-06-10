---
keywords: Experience Platform；ホーム；人気のトピック；http api
solution: Experience Platform
title: HTTP API Source コネクタの概要
description: API またはユーザーインターフェイスを使用してAdobe Experience Platformと接続するストリーミングコネクタを作成する方法について説明します。
exl-id: 41e079f3-75b2-4033-8138-73162c31461a
source-git-commit: bad1e0a9d86dcce68f1a591060989560435070c5
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 19%

---

# [!DNL HTTP API] コネクタ

>[!IMPORTANT]
>
>Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL HTTP API] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL HTTP API] ソースコネクタを使用して、データをExperience Platformにストリーミングできます。 [!DNL HTTP API] ソースは [!DNL Data Prep] 関数でサポートされており、XDM に準拠していないデータを XDM 準拠のデータセットにマッピングできます。

>[!NOTE]
>
>ストリーミングデータフローを作成または更新した後、データ損失やデータ削除が発生する可能性を防ぐために、データ取り込みを 5 分間ほど一時停止する必要があります。

以下のドキュメントでは、API またはユーザーインターフェイスを使用してと接続する、HTTP API ストリーミングコネクタの作成方法 [!DNL Experience Platform] ついて説明します。

## API を使用した HTTP API ストリーミングコネクタの作成

- [Flow Service API を使用した HTTP ストリーミング接続の作成](../../tutorials/api/create/streaming/http.md)

## UI を使用した HTTP API ストリーミングコネクタの作成

- [UI での HTTP API ストリーミング接続の作成](../../tutorials/ui/create/streaming/http.md)
