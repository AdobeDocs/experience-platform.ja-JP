---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Microsoft Dynamics ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Microsoft Dynamics をAdobe Experience Platformに接続する方法を説明します。
exl-id: 6ca162ce-2016-4270-b741-301cf4230233
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 7%

---

# Microsoft Dynamics コネクタ

Adobe Experience Platformを使用すると、[!DNL Platform] サービスを使用して、受信データの構造化、ラベル付け、強化を行うことができ、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティの CRM システムからデータを取り込む機能を備えています。CRM プロバイダーのサポートは [!DNL Microsoft Dynamics] です。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Microsoft Dynamics] ソースコネクタは、現在、Platform への同じ領域接続をサポートしていません。 つまり、Azure インスタンスが Platform と同じネットワーク領域を使用している場合、Platform ソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Microsoft Dynamics] を [!DNL Platform] に接続する方法について説明します。

## API を使用して [!DNL Microsoft Dynamics] を [!DNL Platform] に接続

- [フローサービス API を使用した Microsoft Dynamics ベース接続の作成](../../tutorials/api/create/crm/ms-dynamics.md)
- [フローサービス API を使用した CRM ソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/crm.md)
- [フローサービス API を使用した CRM ソースのデータフローの作成](../../tutorials/api/collect/crm.md)

## UI を使用して [!DNL Microsoft Dynamics] を [!DNL Platform] に接続します

- [UI での Microsoft Dynamics ソース接続の作成](../../tutorials/ui/create/crm/dynamics.md)
- [UI での CRM 接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
